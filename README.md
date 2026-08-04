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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Dailyhunt is India's largest local-language content and news aggregation platform, operated by VerSe Innovation (Bengaluru). It aggregates news, video and short-form "DailyShare" cards from more than 600 publisher partners across fourteen Indian languages, and syndicates that catalogue to approved partners through a signed HTTP API. Alongside syndication it runs an advertising stack: the Dailyhunt Direct self-serve ad platform, a JavaScript Tracker SDK for down-funnel conversion tracking, and an E-Commerce Shopping Catalog API for vendor product feeds.

Two public API surfaces are profiled here:

- **Content Syndication API** — `feed.dailyhunt.in/api/v2/syndication` — channel discovery, content fetch, search, languages, feedback, live cricket, and a mandatory view-tracking callback. Documented at https://api-syndication.dailyhunt.in/.
- **E-Commerce Shopping Catalog API** — `money.dailyhunt.in/shopping-catalog/v1` — vendor catalog creation and asynchronous product batch ingestion. Documented at https://developer.dailyhunt.in/ads/docs/shopping-catalog/.

Access to both is partner-gated: keys are provisioned over email at onboarding, and the production feed host does not resolve in public DNS. Dailyhunt publishes no OpenAPI — the specifications in `openapi/` were **derived by API Evangelist from Dailyhunt's own published documentation** and are labelled `x-apievangelist-provider-published: false`. Dailyhunt's own Postman collection is harvested verbatim in `postman/`.

- https://www.dailyhunt.in/
- https://developer.dailyhunt.in/ads/
- https://api-syndication.dailyhunt.in/
- https://github.com/dailyhunt
