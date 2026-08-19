---
name: Send a per-contact personalized email version
description: Create an HTML email in Inflection, then push a unique per-contact version of it so each recipient gets a message written for them, with the default email as the fallback.
api: openapi/_original/inflectionio-openapi-original.yml
operations: [createEmail, getEmail, setEmailVersion, getContactByEmail]
---

# Send a per-contact personalized email version

Inflection's Personalized Email Asset holds one default email plus an optional tailored version for
each contact. Contacts without a version receive the default — there is never a blank send. This is
the flow an agent uses to generate hundreds of individualized emails.

## Auth
`Authorization: Bearer <token>` — a Personal Access Token with **WRITE** permission (`inf_pat_...`), or
an OAuth 2.1 connected-app access token acting as an admin or member. Base URL
`https://api.inflection.io/v1`.

## Steps
1. **Create the email** — `createEmail` (`POST /v1/emails`). Required: `name`, `html`, `subject`,
   `fromEmail` (an object with `email` and `name`) and `replyTo`; `preheader` is optional. Only HTML
   emails can be created over the API — pass the full body in `html`. A duplicate name returns
   **409 `TEMPLATE_ALREADY_EXISTS`**; an unapproved sending domain returns **400
   `UnapprovedEmailDomainException`**; an unknown token in the HTML returns **400
   `UnknownVariablesException`**.
2. **Confirm what was stored** — `getEmail` (`GET /v1/emails/{id}`) returns the email including its
   `editorUrl`, the hosted BEE editor link for human restyling. A missing email returns **404**
   (unlike contacts, which report 400).
3. **Resolve each recipient** — `getContactByEmail` (`GET /v1/contacts/by-email/{email}`). URL-encode
   the `@` as `%40`. A contact that does not exist returns **400 `BAS-E-002`**; a version cannot be set
   for an unknown contact, so create the contact first (see `inflectionio-sync-contact.md`).
4. **Push the version** — `setEmailVersion` (`POST /v1/email-versions`) with the contact's subject,
   preheader and HTML. One version per contact per asset; re-posting for the same contact **replaces**
   their version.

## Rules
- **Validated on arrival.** A version is rejected unless the contact is known, an unsubscribe link is
  present in the HTML, no other tokens appear in the body, and the payload is under **500 KB**.
- Every response is wrapped in the `data` / `pagination` / `errors` / `meta` envelope; check
  `meta.status` (`SUCCESS`/`FAILURE`).
- `401` = bad/missing token, empty body — but also what a mistyped path under `/v1` returns. Check the
  path before the credential. `403` = valid token without WRITE (or a viewer's OAuth token).
- **No idempotency key exists.** Re-posting a version is safe because it overwrites; re-posting
  `createEmail` is not — it will 409 on the duplicate name.
- **Pace the run.** The limit is 1,000 requests/second/workspace and there are no rate-limit response
  headers to read; on **429**, back off exponentially with jitter.
- Sending is not an API operation. The asset is sent from a journey inside the app.

## See also
- `conventions/inflectionio-conventions.yml` — envelope, pagination, field-case traps.
- `errors/inflectionio-problem-types.yml` — the full error table, including 200-with-errors cases.
- `rate-limits/inflectionio-rate-limits.yml`
