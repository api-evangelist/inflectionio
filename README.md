# Inflection.io

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

Inflection.io is a B2B marketing automation platform positioned as a modern, AI-native replacement for legacy tools like Marketo. It unifies target accounts, product users, customers, and leads with product-usage, sales, and behavioral signals so marketing teams can build audiences, draft campaigns, map customer journeys, score accounts, and report to the CMO — executing in minutes rather than weeks. The Inflection Developer API is a JSON-over-HTTPS REST API (OpenAPI 3.1, base https://api.inflection.io/v1) for reading and writing the people in a workspace: their profiles, product and marketing activity, static lists, emails, and per-contact personalized email versions, authenticated with scoped Personal Access Tokens or an OAuth 2.1 connected app. Alongside it Inflection runs a first-party remote MCP server at mcp.inflection.io — OAuth 2.1 with PKCE, its own trust and security page — that exposes the authoring surface the REST API does not: journeys, audiences, segments, tokens and analytics, driven by specialist agents. Surfaced as a version-one-ventures portfolio company and enriched from the provider's public developer, agent, and trust surfaces.

**Areas:** Company · Software-as-a-Service · Marketing · Marketing Automation · Email Marketing · Customer Data · B2B · Contacts · MCP · Agents · Artificial Intelligence · Customer Journey

## Inflection.io on APIs.io

Everything profiled for Inflection.io is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/inflectionio/](https://apis.io/providers/inflectionio/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [Inflection.io Contact Activity API](https://apis.io/apis/inflectionio/inflectionio-contact-activity-api/) — The Contact Activity API from Inflection.io — 4 operation(s) for contact activity.
- [Inflection.io Contacts API](https://apis.io/apis/inflectionio/inflectionio-contacts-api/) — The Contacts API from Inflection.io — 6 operation(s) for contacts.
- [Inflection.io Emails API](https://apis.io/apis/inflectionio/inflectionio-emails-api/) — The Emails API from Inflection.io — 2 operation(s) for creating and reading HTML emails.
- [Inflection.io Email Versions API](https://apis.io/apis/inflectionio/inflectionio-email-versions-api/) — The Email Versions API from Inflection.io — 1 operation for pushing a per-contact personalized version of a Personalized Email Asset.
- [Inflection.io Lists and Members API](https://apis.io/apis/inflectionio/inflectionio-lists-and-members-api/) — The Lists and Members API from Inflection.io — 7 operation(s) for lists and members.
- [Inflection MCP](https://apis.io/apis/inflectionio/inflectionio-mcp/) — Inflection's first-party remote MCP server — the agent-facing authoring surface for journeys, audiences, segments, emails, tokens and analytics, gated by…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Inflectionio Webhooks](https://apis.io/asyncapis/inflectionio/inflectionio-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Inflection MCP](https://apis.io/servers/inflectionio/inflection-mcp/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Inflectionio Agentic Access](https://apis.io/agentic-access/inflectionio/inflectionio-agentic-access/)

#### Collections (3)

Postman and Open Collection request collections.

- [Inflection Developer Contact Activity API](https://apis.io/collections/inflectionio/open-inflectionio-contact-activity-api/)
- [Inflection Developer Contact Activity Contacts API](https://apis.io/collections/inflectionio/open-inflectionio-contacts-api/)
- [Inflection Developer Contact Activity Lists and Members API](https://apis.io/collections/inflectionio/open-inflectionio-lists-and-members-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Inflectionio Openapi Overlay](https://apis.io/overlays/inflectionio/inflectionio-openapi-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Inflectionio Authentication](https://apis.io/security/inflectionio/inflectionio-authentication/)
- [Inflectionio Domain Security](https://apis.io/security/inflectionio/inflectionio-domain-security/)
- [Inflectionio Trust Center](https://apis.io/security/inflectionio/inflectionio-trust-center/)
- [Inflectionio Vulnerability Disclosure](https://apis.io/security/inflectionio/inflectionio-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Inflectionio Scopes](https://apis.io/scopes/inflectionio/inflectionio-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Inflectionio Plans Pricing](https://apis.io/plans/inflectionio/inflectionio-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Inflectionio Rate Limits](https://apis.io/rate-limits/inflectionio/inflectionio-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.inflection.io)
- [Documentation](https://docs.inflection.io/api-reference/introduction)
- [Getting started](https://docs.inflection.io/api-reference/quickstart)
- [API reference](https://docs.inflection.io/api-reference/introduction)
- [Pricing](https://www.inflection.io/pricing)
- [Sign up](https://app.inflection.io/login/start)
- [Support](https://docs.inflection.io)
- [Changelog](https://docs.inflection.io/whats-new/july-2026)
- [Blog](https://www.inflection.io/blog)
- [Security](https://docs.inflection.io/agents/mcp-trust-security)
- [Compliance](https://docs.inflection.io/agents/mcp-trust-security)
- [Terms of service](https://www.inflection.io/legal/terms)
- [Privacy policy](https://www.inflection.io/legal/privacy-policy)
- [GitHub](https://github.com/inflectionio)

## API Evangelist rating

**Kin Score 65.0** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 59.1 |
| Contract governance | 18.2 |
| Operational transparency | 57.9 |
| Developer ergonomics | 64.3 |
| Access clarity | 68.4 |

**Agent readiness 57.1** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | verified |
| Event Surface Described | yes |
| Agent Skills | yes |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | paginated |
| Operation Distinctiveness | distinct |

**Regulatory alignment 41.8** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/inflectionio/](https://apis.io/providers/inflectionio/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Inflection.io: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/inflectionio/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
