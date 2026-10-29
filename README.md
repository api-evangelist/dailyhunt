# Dailyhunt

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Dailyhunt is India's largest local-language content and news aggregation platform, operated by VerSe Innovation (Bengaluru). It aggregates news, video, and short-form "DailyShare" cards from more than 600 publisher partners across fourteen Indian languages and distributes them through its own apps, mobile web and PWA, and — for approved partners — through a signed Content Syndication API that pushes the same catalogue into partner apps, OEM device home screens and browsers. Alongside syndication, Dailyhunt runs an advertising stack: the Dailyhunt Direct self-serve ad platform, a JavaScript Tracker SDK (with a Google Tag Manager path) for down-funnel conversion tracking on advertiser sites, and an E-Commerce Shopping Catalog API for vendors to batch-upload product feeds. VerSe Innovation also operates the Josh short-video app. API access is partner-gated: keys, secrets and partner codes are provisioned during onboarding rather than self-service, and the production feed host is not publicly resolvable.

**Areas:** Company · News · Media · Content Syndication · Content · Advertising · Video · Localization · India · Mobile

## Dailyhunt on APIs.io

Everything profiled for Dailyhunt is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/dailyhunt/](https://apis.io/providers/dailyhunt/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [Dailyhunt Catalog API](https://apis.io/apis/dailyhunt/dailyhunt-catalog-api/) — Create and read a vendor catalog.
- [Dailyhunt Channels API](https://apis.io/apis/dailyhunt/dailyhunt-channels-api/) — Discovery of the content channels a partner is entitled to consume.
- [Dailyhunt Content Fetch API](https://apis.io/apis/dailyhunt/dailyhunt-content-fetch-api/) — Paginated retrieval of the content cards inside a channel, plus keyword search.
- [Dailyhunt Cricket API](https://apis.io/apis/dailyhunt/dailyhunt-cricket-api/) — Live cricket match score and commentary streaming.
- [Dailyhunt Feedback API](https://apis.io/apis/dailyhunt/dailyhunt-feedback-api/) — Fetch the feedback options to display to a user and submit the user's selection.
- [Dailyhunt Languages API](https://apis.io/apis/dailyhunt/dailyhunt-languages-api/) — The languages Dailyhunt's content feeds support.
- [Dailyhunt Products API](https://apis.io/apis/dailyhunt/dailyhunt-products-api/) — Batch create, update and delete products inside a catalog, and poll batch status.
- [Dailyhunt Tracking API](https://apis.io/apis/dailyhunt/dailyhunt-tracking-api/) — Partner callback reporting which cards a user actually saw.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [dailyhunt](https://apis.io/servers/dailyhunt/dailyhunt/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Dailyhunt Agentic Access](https://apis.io/agentic-access/dailyhunt/dailyhunt-agentic-access/)

#### Collections (9)

Postman and Open Collection request collections.

- [Dailyhunt E-Commerce Shopping Catalog API](https://apis.io/collections/dailyhunt/open-dailyhunt-catalog-api/)
- [Dailyhunt Content Syndication Channels API](https://apis.io/collections/dailyhunt/open-dailyhunt-channels-api/)
- [Dailyhunt Content Syndication Content Fetch API](https://apis.io/collections/dailyhunt/open-dailyhunt-content-fetch-api/)
- [Dailyhunt Content Syndication Cricket API](https://apis.io/collections/dailyhunt/open-dailyhunt-cricket-api/)
- [Dailyhunt Content Syndication Feedback API](https://apis.io/collections/dailyhunt/open-dailyhunt-feedback-api/)
- [Dailyhunt Content Syndication Languages API](https://apis.io/collections/dailyhunt/open-dailyhunt-languages-api/)
- [Dailyhunt E-Commerce Shopping Catalog Products API](https://apis.io/collections/dailyhunt/open-dailyhunt-products-api/)
- [Dailyhunt Content Syndication Tracking API](https://apis.io/collections/dailyhunt/open-dailyhunt-tracking-api/)
- [Dailyhunt Content Syndication](https://apis.io/collections/dailyhunt/postman-dailyhunt-content-syndication/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Dailyhunt Content Syndication Overlay](https://apis.io/overlays/dailyhunt/dailyhunt-content-syndication-overlay/)
- [Dailyhunt Shopping Catalog Overlay](https://apis.io/overlays/dailyhunt/dailyhunt-shopping-catalog-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Dailyhunt Authentication](https://apis.io/security/dailyhunt/dailyhunt-authentication/)
- [Dailyhunt Domain Security](https://apis.io/security/dailyhunt/dailyhunt-domain-security/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.dailyhunt.in/)
- [Documentation](https://developer.dailyhunt.in/ads/)
- [Getting started](https://api-syndication.dailyhunt.in/)
- [API reference](https://api-syndication.dailyhunt.in/)
- [Sign up](https://direct.dailyhunt.in/login)
- [Support](https://verse.in/contact-us.php)
- [Blog](http://developer.dailyhunt.in/ads/blog/)
- [Terms of service](https://direct.dailyhunt.in/help/v2/ad-terms-and-conditions)
- [Privacy policy](https://m.dailyhunt.in/privacy)
- [GitHub](https://github.com/dailyhunt)

## API Evangelist rating

**Kin Score 34.7** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 13.9 |
| Contract governance | 4.5 |
| Operational transparency | 18.4 |
| Developer ergonomics | 70.8 |
| Access clarity | 34.2 |

**Agent readiness 23.6** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/dailyhunt/](https://apis.io/providers/dailyhunt/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Dailyhunt: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/dailyhunt/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
