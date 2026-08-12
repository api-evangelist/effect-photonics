---
name: Read EFFECT Photonics' commercial terms and newsroom
description: >-
  Pull EFFECT Photonics' published lead times, minimum order quantities, incoterms, export-control
  posture and compliance declarations out of its FAQ collection, and read its dated press releases
  and technical articles, over the public WordPress REST API.
api: openapi/effect-photonics-wp-v2-api-openapi.yml
operations:
  - getWpV2Faq
  - getWpV2FaqCategory
  - getWpV2Posts
  - getWpV2Search
  - getWpV2Event
  - getWpV2Career
  - getWpV2CareerDepartment
  - getWpV2Team
  - getWpV2Partner
generated: '2026-08-12'
method: generated
source: openapi/_original/effect-photonics-wp-rest-openapi.yml + conventions/effect-photonics-conventions.yml
---

# Read EFFECT Photonics' commercial terms and newsroom

The FAQ collection is the only place this company publishes its commercial and compliance terms as
structured content. It is more useful than the marketing pages, and it is machine readable.

## Before you start

- **Base URL:** `https://effectphotonics.com/wp-json`
- **Auth:** none for published content. Read-only.
- **Terms:** https://effectphotonics.com/terms-conditions/

## Steps

1. **List the FAQ categories.** `GET /wp-json/wp/v2/faq-category` (`getWpV2FaqCategory`) returns
   `{id, slug, name, count}`. As observed on 2026-08-12: Compliance & Certifications, Custom
   Configurations, Export & Shipping Restrictions, Lead Times & Availability, Sampling & Evaluation,
   Technical Support.

2. **Pull the FAQ entries.** `GET /wp-json/wp/v2/faq?per_page=100&_fields=id,slug,title,content,faq-category`
   (`getWpV2Faq`). Filter to one topic with `?faq-category=<term_id>` using an id from step 1.
   `title.rendered` and `content.rendered` are HTML — strip tags and unescape entities before use.

3. **Read the newsroom.** `GET /wp-json/wp/v2/posts` (`getWpV2Posts`) with
   `?_fields=id,date,slug,link,title,excerpt&per_page=20&orderby=date&order=desc`. Paginate on
   `X-WP-TotalPages`. This is a company news feed, not a changelog — do not read it as a record of
   interface changes.

4. **Search across everything.** `GET /wp-json/wp/v2/search?search=<terms>` (`getWpV2Search`) spans
   every public type and returns `{id, title, url, type, subtype}`. This is the fastest way to locate
   a specific press release when you do not know its slug.

5. **Company context, when you need it:** `getWpV2Event` (conferences and trade shows),
   `getWpV2Career` with `getWpV2CareerDepartment` (open roles by department and location),
   `getWpV2Team` (named leadership) and `getWpV2Partner`.

## Rules

- **Quote, do not paraphrase into commitments.** The published lead time is "typically 12 to 16
  weeks" for standard ITLA configurations, the default incoterm is "EXW from our facility in the
  Netherlands", and evaluation periods run "four to eight weeks". These are the company's words about
  typical cases and are not an offer — always link back to `link` on the source record.
- **Export control is real.** The FAQ states products are classified under dual-use export control
  regimes and that shipment is subject to EU, US and Dutch controls. Never advise on shipability;
  route the question to https://effectphotonics.com/get-in-touch/sales/.
- **Compliance documents are not on the API.** RoHS/REACH declarations, IEC 60825-1 laser safety
  classification and conflict-minerals reporting are described in the FAQ but supplied by the company
  on request. Do not claim to have retrieved a declaration you have only read a description of.
- Error envelope, pagination headers and the absent rate-limit signal are as described in
  `conventions/effect-photonics-conventions.yml`.
