---
name: Search Dailyhunt content and capture user feedback
description: Run a keyword search across the Dailyhunt catalogue, render the results, report the views back, and collect a user's feedback signal on a card so Dailyhunt can improve that user's personalized feed.
api: openapi/dailyhunt-content-syndication-openapi.yml
operations:
  - searchContent
  - listFeedbackOptions
  - submitFeedback
  - trackViewedItems
generated: '2026-08-04'
method: generated
source: openapi/dailyhunt-content-syndication-openapi.yml, https://api-syndication.dailyhunt.in/
---

# Search Dailyhunt content and capture user feedback

Use this to add a search box over syndicated Dailyhunt content, and to wire the "why am I seeing
this?" feedback affordance that feeds the personalization loop.

Prerequisites, signing, `puid` and the `dhFeedV1` cookie are identical to
`dailyhunt-syndicate-a-channel-feed.md` — read that first. Everything below assumes each request is
signed with `Authorization: key=<API Key>` and a Base64 HMAC-SHA1 `Signature` over the sorted query
string plus the uppercased method, with `ts` in the signed set.

## Step 1 — search (`searchContent`)

`GET /api/v2/syndication/search?partner={partner}&langCode={lang}&puid={puid}&ts={ts}&query={query}`

`query` is the user's raw search string; URL-encode it before signing, and sign the encoded form.
Search was added in document version 2.3 (2021-08-24) and is the newest operation on the surface.

The response is the same card envelope the channel feed returns —
`data.rows[] / count / pageNumber / nextPageUrl / trackUrl` — so you can reuse the feed renderer.
Search results skew toward the DailyShare-flavoured card fields: `thumbnail` (with width and height),
`tags`, `nsfw`, `contentType` (STORY, VIDEO, VHMEME), `shareCount`, `authorName`, `shareInfo`.

**Respect `nsfw`.** It is a boolean on the card and Dailyhunt does not filter for you.

Paginate by following `nextPageUrl` verbatim, refreshing and re-signing only `ts`.

## Step 2 — offer sharing correctly

Search and DailyShare cards carry `shareInfo`:

- `shareInfo.shareUrl` — a shortened URL for the item.
- `shareInfo.shareString` — a pre-composed share message with the short URL, title and source.

Use `shareString` for a share sheet rather than composing your own; it is what carries Dailyhunt's
attribution.

## Step 3 — report the views (`trackViewedItems`)

Search result pages are list views, so the tracking obligation applies exactly as it does for channel
feeds. POST to the response's `trackUrl` with `partner`, `puid`, `ts` appended:

```json
{
  "viewedDate": 1723200000000,
  "stories": [{ "id": "<card id>", "trackData": "<verbatim trackData>" }]
}
```

204 No Content is success. Fire the `data.track.comscoreUrls[]` pixels for the same page.

## Step 4 — load the feedback options (`listFeedbackOptions`)

`GET /api/v2/syndication/feedback?partner={partner}&langCode={lang}&puid={puid}&ts={ts}`

Returns `data.rows[]` of `{id, title, value}`. `title` is a unicode label already localized to the
`langCode` you requested — render it as-is, do not translate it yourself. Cache the list per language
rather than fetching it per card.

## Step 5 — submit the user's selection (`submitFeedback`)

`POST /api/v2/syndication/feedback?partner={partner}&langCode={lang}&puid={puid}&ts={ts}`

```json
{
  "itemId": "<the card the feedback is about>",
  "options": [{ "id": "<option id>", "value": "<option value>" }]
}
```

Send `Content-Type: application/json`. `options` is an array — a user may pick more than one reason.
`itemId` is the `id` of the card the user reported, not the channel.

Dailyhunt uses these signals to improve that specific user's personalized feed, which is why the
`puid` and the `dhFeedV1` cookie have to be the same ones used to fetch the card. Feedback submitted
under a different `puid` lands on a different profile.

## Failure handling

- 401 — API key wrong for this environment.
- 403 — signature mismatch. Rebuild the base string; check `ts` freshness and that nothing was
  appended after signing.
- 500 — Dailyhunt-side. Retry with backoff.
- There is no response body on failure and no machine-readable error code, so branch on status alone.
- There is no idempotency key on `submitFeedback`. Duplicate-submission behaviour is undefined —
  debounce in your own UI rather than relying on the server to de-duplicate.
