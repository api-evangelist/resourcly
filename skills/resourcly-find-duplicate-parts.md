---
name: Find duplicate and similar parts
description: Use Resourcly to discover duplicate, similar and substitute parts across harmonised inventory, and pull an AI engineering comparison for a candidate pair.
api: openapi/resourcly-openapi-original.json
operations:
  - GET /items
  - GET /items/similarity-clusters
  - GET /items/{id}/similar
  - GET /items/{id1}/compare/{id2}
  - GET /items/{id1}/compare/{id2}/ai-analysis
  - PUT /items/{id1}/comparison-status/{id2}
---

# Find duplicate and similar parts

Resourcly ingests PLM/ERP parts data and clusters similar items so you can retire
duplicates and choose substitutes.

## Auth
Send a Firebase JWT as `Authorization: Bearer {token}` on every request
(see `authentication/resourcly-authentication.yml`). Base URL `https://api.resourcly.com/v1`.

## Steps
1. **List items** — `GET /items` (paginate with `limit` + `offset`; scope with
   `business_node_ids`) to find the part you care about, or `GET /items/similarity-clusters`
   to browse AI-classified clusters of near-duplicate items.
2. **Get candidates** — `GET /items/{id}/similar` for a ranked list of similar items,
   or `GET /items/{id}/visual-similar` for image-based matches.
3. **Compare a pair** — `GET /items/{id1}/compare/{id2}` for a side-by-side attribute
   comparison, then `GET /items/{id1}/compare/{id2}/ai-analysis` for the AI engineering
   verdict (whether the two parts are interchangeable).
4. **Record the decision** — `PUT /items/{id1}/comparison-status/{id2}` to mark the pair
   reviewed/confirmed so the duplicate set is tracked.

## Conventions & errors
Pagination is `limit`/`offset` on `/items`. On errors expect a JSON envelope
`{code, error, details}` (see `errors/resourcly-problem-types.yml`); handle `401`
(expired token), `404` (unknown item id) and `429` (back off).
