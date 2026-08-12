---
name: Browse the EFFECT Photonics optical product catalogue
description: >-
  Read EFFECT Photonics' coherent optical laser assembly catalogue over its public WordPress REST
  API, and filter it by the seven hardware taxonomies the site attaches to each product — product
  line, output power, form factor, tuning range, target application, operating temperature grade and
  management interface.
api: openapi/effect-photonics-wp-v2-api-openapi.yml
operations:
  - getWpV2Product
  - getWpV2ProductId
  - getWpV2ProductLine
  - getWpV2OutputPower
  - getWpV2FormFactor
  - getWpV2TuningRange
  - getWpV2TargetApplication
  - getWpV2Temperature
  - getWpV2ManagementInterface
  - getWpV2MediaId
generated: '2026-08-12'
method: generated
source: openapi/_original/effect-photonics-wp-rest-openapi.yml + conventions/effect-photonics-conventions.yml
---

# Browse the EFFECT Photonics optical product catalogue

EFFECT Photonics has **no developer API programme**. What it does have is a WordPress REST API on
`https://effectphotonics.com/wp-json/` whose `product` collection is a real, publicly readable
catalogue of coherent optical laser assemblies. This skill reads it correctly.

## Before you start

- **Base URL:** `https://effectphotonics.com/wp-json`
- **Auth:** none. Published content reads anonymously. Do **not** send credentials.
- **Read-only.** Every write operation on this host is capability-gated and none of them belong to
  you. Use `GET` only.
- **Terms:** https://effectphotonics.com/terms-conditions/

## Steps

1. **Confirm the surface is what you think it is.** `GET /wp-json/wp/v2/types` (`getWpV2Types`)
   returns the registered post types. Look for `product` and read its `taxonomies` array — that is
   the authoritative list of the hardware facets, and it can change.

2. **List products.** `GET /wp-json/wp/v2/product` (`getWpV2Product`).
   - Trim the payload: `?_fields=id,slug,link,title,product_line,form_factor,output_power,tuning_range,target_application,temperature,management_interface`
   - Paginate with `page` and `per_page` (max 100). Read `X-WP-Total` and `X-WP-TotalPages` from the
     response headers; they are exposed via `Access-Control-Expose-Headers`.
   - The catalogue is small — 2 products on 2026-08-12 — so one page is normally enough.

3. **Resolve taxonomy terms before filtering.** The product record carries **term ids**, not names.
   Fetch the vocabulary you care about, e.g. `GET /wp-json/wp/v2/management_interface`
   (`getWpV2ManagementInterface`) or `GET /wp-json/wp/v2/target_application`
   (`getWpV2TargetApplication`), each returning `{id, slug, name, count}`.

4. **Filter products by term id.** `GET /wp-json/wp/v2/product?target_application=<term_id>`. Combine
   facets by repeating the query parameter for each taxonomy. Filtering by slug does **not** work —
   resolve to an id first.

5. **Read one product.** `GET /wp-json/wp/v2/product/{id}` (`getWpV2ProductId`). Add `?_embed` to
   inline the featured image and the resolved taxonomy terms in `_embedded`, which saves the round
   trips in step 3.

6. **Fetch product imagery** with `GET /wp-json/wp/v2/media/{id}` (`getWpV2MediaId`) using the
   `featured_media` id.

## Rules

- **Nothing here is a datasheet.** The API returns marketing page content. Optical performance
  figures, register maps and integration guides are not on this API — the FAQ says they ship with the
  part or come under NDA. Do not synthesise specifications from titles.
- **No idempotency contract exists** on this surface, which is another reason to stay read-only.
- **No rate limits are published** and no `RateLimit-*` or `Retry-After` header is returned. Be
  conservative anyway: this is a marketing site behind a LiteSpeed cache, not a metered API.
- **Cache politely.** Collection reads return an `ETag`; send `If-None-Match` on refresh.
- **Errors** come back as `{"code":…,"message":…,"data":{"status":…}}` — the WordPress envelope, not
  RFC 9457 problem+json. See `errors/effect-photonics-problem-types.yml`.
- **Counts drift.** Treat every count in this repository as an observation dated 2026-08-12.
