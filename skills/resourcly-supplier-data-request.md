---
name: Request and review supplier data
description: Send a supplier a secure data request, track it, and review the supplier's uploaded response inside Resourcly.
api: openapi/resourcly-openapi-original.json
operations:
  - POST /v1/email/supplier-data-request
  - GET /v1/supplier-data-requests
  - GET /v1/supplier-data-requests/{id}
  - POST /v1/supplier-data-requests/{id}/review
  - POST /v1/supplier-data-requests/{id}/extend-expiry
---

# Request and review supplier data

Collect missing part or catalog data from a supplier through a passcode-protected
request and review what they submit.

## Auth
Send a Firebase JWT as `Authorization: Bearer {token}`. Base URL `https://api.resourcly.com/v1`.
(The supplier-facing response endpoints under `/v1/supplier/request/{id}` use a
per-request passcode instead, verified via `POST /v1/supplier/verify-passcode`.)

## Steps
1. **Send the request** — `POST /v1/email/supplier-data-request` to email a supplier a
   secure data-request link.
2. **Track requests** — `GET /v1/supplier-data-requests` to list open requests and
   `GET /v1/supplier-data-requests/{id}` for the detail and current status.
3. **Extend if needed** — `POST /v1/supplier-data-requests/{id}/extend-expiry` to push
   out the expiry when a supplier needs more time.
4. **Review the response** — once the supplier submits, `POST /v1/supplier-data-requests/{id}/review`
   to accept or reject the uploaded data.

## Conventions & errors
Errors return `{code, error, details}` (see `errors/resourcly-problem-types.yml`);
handle `401`, `403` (not your request), `404` and `409` (already reviewed/expired).
