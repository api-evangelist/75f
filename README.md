# 75F

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

75F is an IoT-based building management and automation company (Burnsville, Minnesota; acquired by Carrier Global in July 2026) whose vertically integrated platform pairs wireless sensors, thermostats and Central Control Units with cloud software — Facilisight, Portfolio Analytics, Hayloft and Saffron AI — to control and optimize HVAC, lighting and equipment in commercial buildings. Its developer surface is a Project Haystack-compliant REST API published through an Azure API Management gateway at api.75f.io, exposing Haystack ops (read, hisReadMany, pointWrite, hisWriteMany) over Haystack-tagged JSON and ZINC grids, plus a v2 Special Schedules API for exception-based scheduling. Access is OAuth 2.0 client-credentials bearer tokens layered on a product-scoped APIM subscription key, and integrators also reach the system over BACnet IP/MS-TP, Modbus and a Niagara driver.

**Areas:** Company · Building Automation · Smart Buildings · HVAC · IoT · Energy Management · Project Haystack · Facilities Management · Sensors · Building Management System

## 75F on APIs.io

Everything profiled for 75F is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/75f/](https://apis.io/providers/75f/)** — or go straight to one of the APIs or properties below.

### APIs (2)

- [75F Haystack API](https://apis.io/apis/75f/75f-haystack-api/) — 75F's Project Haystack-compliant external REST API. Reads Haystack entity records by filter or by id (read), reads time-series history for many points at…
- [75F Special Schedules API](https://apis.io/apis/75f/75f-special-schedules-api/) — A v2 REST API for exception-based ("special") schedules applied at building or zone level — create, update and delete date-bounded occupancy overrides with…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [75f](https://apis.io/servers/75f/75f/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [75F Authentication](https://apis.io/security/75f/75f-authentication/)
- [75F Domain Security](https://apis.io/security/75f/75f-domain-security/)
- [75F Trust Center](https://apis.io/security/75f/75f-trust-center/)

#### OAuth scopes (1)

The permissions the API asks for.

- [75F Scopes](https://apis.io/scopes/75f/75f-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [75F Plans Pricing](https://apis.io/plans/75f/75f-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [75F Rate Limits](https://apis.io/rate-limits/75f/75f-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.75f.io/)
- [Documentation](https://support.75f.io/hc/en-us/sections/5484889695123-API-s-for-Integrations)
- [Getting started](https://support.75f.io/hc/en-us/articles/5509604551443-Accessing-the-API)
- [API reference](https://support.75f.io/hc/en-us/articles/5459129016595-75F-External-API-s-Overview)
- [Sign up](https://api-management-75f-dev.developer.azure-api.net/)
- [Support](https://support.75f.io/hc/en-us)
- [Changelog](https://support.75f.io/hc/en-us/categories/11037028410771-Feature-Release-History)
- [Blog](https://www.75f.io/news/)
- [Compliance](https://www.75f.io/software/security/)
- [Terms of service](https://www.datocms-assets.com/55631/1648491708-75f-terms-of-service-pdf-2022.pdf)
- [Privacy policy](https://www.datocms-assets.com/55631/1650987646-75f-website-privacy-notice-march-2022.pdf)
- [Status](https://75f.statuspage.io/)

## API Evangelist rating

**Kin Score 39.9** — developing · rubric 0.24.0 · scored 2026-10-20

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 0.0 |
| Contract governance | 18.2 |
| Operational transparency | 31.6 |
| Developer ergonomics | 66.1 |
| Access clarity | 50.0 |

**Agent readiness 8.8** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | — |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
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

**Regulatory alignment 38.5** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/75f/](https://apis.io/providers/75f/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for 75F: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/75f/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
