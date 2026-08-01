---
name: Import and compare BOMs
description: Import a bill of materials from a spreadsheet into Resourcly and run a BOM comparison to surface differences, then export the result.
api: openapi/resourcly-openapi-original.json
operations:
  - POST /boms/preview
  - POST /boms/import
  - GET /boms
  - POST /bom-comparisons
  - GET /bom-comparisons/{id}
  - GET /bom-comparisons/{id}/lines
  - GET /bom-comparisons/{id}/export
---

# Import and compare BOMs

Load bills of materials into Resourcly and diff two BOMs to find changed, added or
removed lines.

## Auth
Send a Firebase JWT as `Authorization: Bearer {token}`. Base URL `https://api.resourcly.com/v1`.

## Steps
1. **Preview the file** — `POST /boms/preview` with the CSV/XLSX to confirm column
   detection before committing.
2. **Import** — `POST /boms/import` to create the BOM from the spreadsheet.
3. **List BOMs** — `GET /boms` (paginate with `page` + `page_size`; filter with
   `business_node_id`, `search`, `status`) to get the two BOM ids to compare.
4. **Create the comparison** — `POST /bom-comparisons` with the two BOM ids.
5. **Read results** — `GET /bom-comparisons/{id}` for the summary and
   `GET /bom-comparisons/{id}/lines` for the paginated line-level diff.
6. **Export** — `GET /bom-comparisons/{id}/export` to download the comparison as XLSX.

## Conventions & errors
BOM listing uses `page`/`page_size` pagination. Errors return `{code, error, details}`;
handle `400` (bad file/columns), `401`, `404` and `429`.
