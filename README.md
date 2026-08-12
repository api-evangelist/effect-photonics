# EFFECT Photonics

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

EFFECT Photonics is an optical-semiconductor company headquartered at High Tech Campus 37 in Eindhoven, the Netherlands, with sites in the United Kingdom, the United States and Taiwan. Spun out of Eindhoven University of Technology, it designs and manufactures coherent optical transceiver subsystems and monolithically integrated Indium Phosphide (InP) photonic integrated circuits, including its pico and nano Integrable Tunable Laser Assemblies for 100G, 400G and 800G coherent links.

**EFFECT Photonics operates no developer API programme.** It publishes no product, network-management or transceiver-control API, no developer portal, no documentation, no OpenAPI, no SDK, no CLI, no sandbox and no pricing. This profile exists because the company's corporate website runs a live, self-describing WordPress REST API — 290 routes across 13 namespaces at `https://effectphotonics.com/wp-json/` — which includes a publicly readable optical-product catalogue keyed by seven hardware taxonomies (product line, output power, form factor, tuning range, target application, operating temperature grade and management interface), a first-party `effect/v1` namespace, and two Model Context Protocol servers guarded by an OAuth 2.1 authorization server the host advertises through RFC 8414 and RFC 9728 metadata. Both MCP servers returned HTTP 401 to an anonymous `tools/list` on 2026-08-12, so no tool list is recorded.

Every specification under `openapi/` is an API Evangelist derivation of that live route-discovery document, marked `x-apievangelist-method: derived`. It is not a provider-published contract.

- https://effectphotonics.com/
- https://effectphotonics.com/wp-json/
