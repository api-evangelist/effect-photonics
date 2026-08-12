---
name: Discover the EFFECT Photonics API surface
description: >-
  Enumerate what is actually callable on effectphotonics.com — the WordPress REST namespaces, the
  first-party effect/v1 endpoints, the two Model Context Protocol servers and the OAuth 2.1
  discovery documents that guard them — and establish what is gated before attempting anything.
api: openapi/_original/effect-photonics-wp-rest-openapi.yml
operations:
  - getRoot
  - getWpV2Types
  - getWpV2Taxonomies
  - getMcp
  - getWpAbilitiesV1Abilities
  - getEffectV1GlbPositionsId
generated: '2026-08-12'
method: generated
source: openapi/_original/effect-photonics-wp-rest-openapi.yml + mcp/effect-photonics-mcp.yml + well-known/effect-photonics-well-known.yml
---

# Discover the EFFECT Photonics API surface

There is no developer portal, no documentation and no SDK. The surface describes itself, or not at
all. This skill establishes the ground truth in five calls.

## Steps

1. **Route discovery.** `GET /wp-json/` (`getRoot`) returns the site name, the `namespaces` array and
   all 290 routes with their methods and argument schemas. Everything in `openapi/` in this
   repository is derived from this one document. Namespaces observed on 2026-08-12: `wp/v2`,
   `wp-abilities/v1`, `mcp`, `oembed/1.0`, `effect/v1`, plus plugin namespaces (`akismet/v1`,
   `litespeed/v1`, `litespeed/v3`, `yoast/v1`, `connector/v1`, `duplicate-post/v1`,
   `wp-site-health/v1`, `wp-block-editor/v1`) that are vendor internals and are excluded from the
   derived specs.

2. **Content model.** `GET /wp-json/wp/v2/types` (`getWpV2Types`) and
   `GET /wp-json/wp/v2/taxonomies` (`getWpV2Taxonomies`) give you the first-party types — `product`,
   `career`, `event`, `partner`, `team`, `faq` — and the taxonomies bound to each.

3. **MCP.** `GET /wp-json/mcp` (`getMcp`) lists two servers:
   `/wp-json/mcp/mcp-oauth-server` and `/wp-json/mcp/mcp-adapter-default-server`. Both answer an
   anonymous `tools/list` with **HTTP 401**. The OAuth server returns a correct RFC 9728 challenge:
   `WWW-Authenticate: Bearer realm="https://effectphotonics.com", resource_metadata="https://effectphotonics.com/.well-known/oauth-protected-resource"`.
   Follow it to `/.well-known/oauth-protected-resource` and then
   `/.well-known/oauth-authorization-server` (both HTTP 200) to learn the flow: authorization_code +
   refresh_token, PKCE `S256`, single scope `mcp`, public clients only
   (`token_endpoint_auth_methods_supported: ["none"]`), registration by client_id metadata document.

4. **Abilities.** `GET /wp-json/wp-abilities/v1/abilities` (`getWpAbilitiesV1Abilities`) is the
   registry the MCP adapter projects into tools. It is **401 anonymously**, so the tool names and
   input schemas behind those servers are not publicly discoverable. Do not guess them.

5. **First-party namespace.** `GET /wp-json/effect/v1` lists three routes.
   `POST /effect/v1/careers/sync` is capability-gated (401). `GET /effect/v1/glb-positions/{id}`
   (`getEffectV1GlbPositionsId`) is anonymously reachable and returns the site's own error code
   `effect_invalid_attachment` (HTTP 404) for an id that is not a media attachment.

## Rules

- **A 401 is a finding, not a failure.** Record it. Do not retry with fabricated credentials, and do
  not attempt the WordPress login, application-password or nonce paths — none of them are yours.
- **Stay on GET.** Every POST/PUT/PATCH/DELETE on this host mutates a company's live website.
- **Do not treat the derived OpenAPI as a provider contract.** The files in `openapi/` carry
  `x-apievangelist-method: derived`; EFFECT Photonics publishes no OpenAPI.
- `/.well-known/security.txt`, `/.well-known/openid-configuration`, `/.well-known/api-catalog`,
  `/.well-known/agent-card.json`, `/.well-known/agent.json`, `/llms.txt` and `/graphql` were all
  probed on 2026-08-12 and all returned 404. There is no agent card and no GraphQL surface.
