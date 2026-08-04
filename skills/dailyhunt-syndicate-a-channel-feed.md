---
name: Syndicate a Dailyhunt channel feed
description: Discover the Dailyhunt channels a partner is entitled to, pull a page of content cards from one, render them, and satisfy Dailyhunt's mandatory view-tracking and attribution obligations.
api: openapi/dailyhunt-content-syndication-openapi.yml
operations:
  - listChannels
  - listLanguages
  - fetchItems
  - trackViewedItems
generated: '2026-08-04'
method: generated
source: openapi/dailyhunt-content-syndication-openapi.yml, https://api-syndication.dailyhunt.in/
---

# Syndicate a Dailyhunt channel feed

Use this when you need to render Dailyhunt content inside a partner surface — an app, a browser home
screen, an OEM device feed, a PWA.

## Before you start

You cannot call this API without a partner arrangement. Dailyhunt provisions three things over email
at onboarding: an **API Key**, a **Secret Key** and a **Partner Code**. The production host
`feed.dailyhunt.in` does not resolve in public DNS — if your resolver returns NXDOMAIN, you are not
onboarded, not misconfigured.

Choose an environment first. They are separated by host, not by key prefix, and each has its own key
pair:

- Stage: `http://qa-news.newshunt.com`
- Prod: `http://feed.dailyhunt.in`

## Sign every request

Every call needs two headers, and the signature covers the query string, so build the query string
first and never append a parameter after signing.

1. Append `ts=<current epoch milliseconds>` to your query parameters.
2. URL-encode every key and value.
3. Sort the parameters lexicographically on the encoded key.
4. Join them as `key=value` pairs with `&`.
5. Append the **uppercased** HTTP method to that string. This is the signature base string.
6. `HMAC-SHA1(secretKey, signatureBaseString)`, then Base64-encode it.

Send:

```
Authorization: key=<API Key>
Signature: <base64 HMAC-SHA1>
```

A 401 means the API key is wrong for this environment. A 403 means the signature did not match —
almost always a stale `ts` or a parameter added after signing.

## Carry user identity

Every call takes `puid`, your own unique id for the end user (a device identifier works). Dailyhunt
maps it to an internal profile to personalize the feed.

The first response sets a `dhFeedV1` cookie. **Persist it per user and replay it on every later
call.** Dailyhunt regenerates it if it is missing or expired, and rejects it if it is invalid. Do not
share one cookie across users — it is the user identity.

## Step 1 — resolve the language (`listLanguages`)

`GET /api/v2/syndication/languages?partner={partner}&ts={ts}`

Returns `data.rows[]` of `{name, nameUni, code}`. Fourteen codes are published: `en hi mr gu pa bn
kn ta te ml or ur bh ne`. Let the user pick; do not hardcode `en`. Not every channel has content in
every language, so verify your chosen language against the channels you intend to ship.

## Step 2 — discover channels (`listChannels`)

`GET /api/v2/syndication/channels?partner={partner}&langCode={lang}&puid={puid}&ts={ts}&pfm={pfm}`

`pfm` is the **partner feature mask** — the sum of the feature bits Dailyhunt enabled for you:
LIVE_TV_CARD 1, CRICKET_CARD brief 8, HERO_CARD 16, VIDEO_CHANNELS 32, VIRAL_CHANNELS 64,
CRICKET_CARD detailed 128 (bits 2 and 4 are reserved). Hero cards plus video is `16+32 = 48`. Set
`pfm` here, at the channels call — each returned channel's `contentUrl` already carries the correct
channel-level `fm`.

Each row gives you `id`, `name`, `type`, `contentUrl`, `deepLinkUrl` and `appDeepLinkUrl`. **Keep
`contentUrl`** — it is the input to the next step.

For the sibling channel sets use `listLocationChannels` (`/channels/locations`) or
`listVideoChannels` (`/video/channels`); `listChannels` with `pfm=64` returns the DailyShare viral
channels. All three require Dailyhunt to have enabled them for your partner code.

## Step 3 — fetch cards (`fetchItems`)

Call the channel's `contentUrl` rather than composing `/items?cid=…` yourself — it already has the
right `fm` and `cid`. Add `ts` and re-sign.

`GET /api/v2/syndication/items?partner={partner}&cid={cid}&langCode={lang}&puid={puid}&ts={ts}&fm={fm}&pageSize=10&pageNumber=0`

You get `data.rows[]` of cards. Branch rendering on `type` — STORY, VIDEO, PHOTO, ASTROLOGY, VHMEME,
BANNER, QUESTION_MULTI_CHOICES, HTML — and lay out on `uiType` (NORMAL, HERO, TILE_3, GRID_3, GRID_5
and the VH_* DailyShare layouts). Only QUESTION_MULTI_CHOICES cards populate `collectionItems`.

**Images are templates, not URLs.** `http://bcdn.newshunt.com/{CMD}/{W}x{H}_{Q}/(folder).{EXT}` —
substitute `crop` or `resize`, the width and height for the device, a quality percentage up to 100,
and an extension such as `webp`. Dailyhunt generates and CDN-caches each variant on demand.

**Pagination:** follow `data.nextPageUrl` verbatim. It carries opaque continuation state
(`pageScrollStart`, `psi`, `dsOffset`) you must resend unchanged. The only value you refresh is
`ts` — then re-sign.

## Step 4 — track what was seen (`trackViewedItems`) — not optional

This is a contractual obligation, not telemetry. Fire it for **every list view**: the first page of a
new channel, each next page, and on list load.

Take `data.trackUrl` from the fetch response, append `partner`, `puid` and `ts`, sign it, and POST:

```json
{
  "viewedDate": 1723200000000,
  "stories": [
    { "id": "<card id>", "trackData": "<the card's trackData, verbatim>" }
  ]
}
```

`trackData` is opaque — copy it exactly, never parse or synthesize it. If `pageSize` was 10, the
array has 10 entries.

**204 No Content is success here.** 400 means a malformed payload; 500 is a Dailyhunt-side failure.

There is no idempotency key on this endpoint. Repeat-submission semantics are undefined, so do not
blind-retry a POST that may have succeeded — treat a timeout as ambiguous and log it.

## Step 5 — fire the ComScore pixel — also not optional

The same fetch response carries `data.track.comscoreUrls[]`. Ping each one once per list view,
alongside the tracking call.

- Browser: inject an `<img src="<comscore url>">`.
- Native or non-browser: call the URL directly, then persist the cookie ComScore sets and replay it
  on later calls.

## Step 6 — handle clickouts correctly

When the user taps a card, open its `deepLinkUrl` with your `puid` appended and the `dhFeedV1`
cookie attached. **Preserve every tracking parameter Dailyhunt embedded** — `s`, `ss`, `utm_source`,
`utm_medium`, `utm_campaign`, `partnerRef` — verbatim. Stripping them breaks attribution. The link
opens in the browser, or in the Dailyhunt app if the user has it installed and chooses it.

## What you cannot rely on

- No rate limits are published, and no rate-limit headers are returned. Capacity is negotiated with
  Dailyhunt at onboarding — share your volume estimate rather than probing for a ceiling.
- No error body. You get an HTTP status and nothing else; there is no error code to branch on.
- No status page and no SLA. When something breaks, email is the only channel.
- No webhooks. Nothing is pushed to you; poll, and remember that the write direction on this API is
  you reporting to Dailyhunt, not the reverse.
