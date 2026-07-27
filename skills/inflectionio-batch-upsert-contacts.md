---
name: Batch upsert contacts
description: Upsert many people in one call by email and reconcile the per-contact results from the async transaction.
api: openapi/inflectionio-openapi-original.yml
operations: [batchUpsertContacts, getContactTransaction]
---

# Batch upsert contacts

Load or refresh many people at once.

## Auth
Personal Access Token with **WRITE** scope: `Authorization: Bearer inf_pat_...`. Base `https://api.inflection.io/v1`.

## Steps
1. **Submit the batch** — `batchUpsertContacts` (`POST /v1/contacts/batch`). Match on `email`; put attributes under `properties` (snake_case). Returns `200` with a **PENDING** transaction.
2. **Poll** — `getContactTransaction` (`GET /v1/contacts/transactions/{transactionId}`) until `data.status` is `DONE`.
3. **Reconcile** — a completed batch carries `data.data.results[]`, each `{ email, status }` where status is `CREATED`, `UPDATED`, `NO_CHANGE`, or `FAILED`. Retry or report the `FAILED` rows.

## Rules
- Poll with backoff; do not assume the write is done when the POST returns.
- Unknown transaction id → `NOT_EXIST` (HTTP 200), not 404.
- Envelope + `meta.status` apply as with every endpoint.
