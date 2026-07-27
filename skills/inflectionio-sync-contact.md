---
name: Sync a contact into Inflection
description: Look a person up by email, then create or update them, handling Inflection's asynchronous contact-write transaction.
api: openapi/inflectionio-openapi-original.yml
operations: [getContactByEmail, createContact, updateContact, getContactTransaction]
---

# Sync a contact into Inflection

Keep a person's profile in sync with your source of truth.

## Auth
Send a Personal Access Token with the **WRITE** scope: `Authorization: Bearer inf_pat_...`. Base URL `https://api.inflection.io/v1`.

## Steps
1. **Look them up** — `getContactByEmail` (`GET /v1/contacts/by-email/{email}`). URL-encode the `@` as `%40`. A missing contact returns **400** with code `BAS-E-002` (not 404) — treat that as "create".
2. **Write** — if absent, `createContact` (`POST /v1/contacts`); if present, `updateContact` (`PATCH /v1/contacts/{id}`). Contact attributes go under `properties` using **snake_case** keys.
3. **Writes are asynchronous** — the call returns `200` with a **PENDING** transaction (`data.transactionId`), not the finished contact.
4. **Poll** — `getContactTransaction` (`GET /v1/contacts/transactions/{transactionId}`) until `data.status` is `DONE`. An unknown id returns `NOT_EXIST` (still HTTP 200).

## Rules
- Every response is wrapped in a `data` / `pagination` / `errors` / `meta` envelope; check `meta.status` (`SUCCESS`/`FAILURE`).
- `401` = bad/missing token (empty body); `403` = valid token missing the WRITE scope.
- No idempotency key — dedupe on `email` yourself and rely on upsert semantics.
