---
name: Build and populate a static list
description: Create a static list and add contacts to it, then read its members back.
api: openapi/inflectionio-openapi-original.yml
operations: [createList, addListMembers, getListMembers, removeListMember]
---

# Build and populate a static list

## Auth
Personal Access Token with **WRITE** scope (READ is enough for the read-back): `Authorization: Bearer inf_pat_...`. Base `https://api.inflection.io/v1`.

## Steps
1. **Create the list** — `createList` (`POST /v1/lists`). A duplicate name returns `400` with code `LIST_ALREADY_EXISTS`; reuse the existing list instead of retrying.
2. **Add members** — `addListMembers` (`POST /v1/lists/{id}/members`) with the contacts to attach.
3. **Read back** — `getListMembers` (`GET /v1/lists/{id}/members`), paginated with `page_number`/`page_size` (max 200); the window echoes under `pagination`.
4. **Remove** — `removeListMember` (`DELETE /v1/lists/{id}/members/{contactId}`) to detach one.

## Rules
- A missing list returns `404` `NOT_FOUND` (unlike a missing contact, which is `400`).
- Responses use the standard `data`/`pagination`/`errors`/`meta` envelope.
