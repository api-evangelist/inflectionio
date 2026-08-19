---
name: Inflection
description: Use when building customer journeys, managing contacts and audiences, integrating with Salesforce or data warehouses, sending personalized emails at scale, or working with Inflection's AI agents via MCP, Claude, ChatGPT, or Slack. Reach for this skill for marketing automation, audience segmentation, event-driven campaigns, and API-driven contact management.
metadata:
    mintlify-proj: inflection
    version: "1.0"
---

# Inflection Skill

## Product summary

Inflection is a B2B marketing automation platform that unifies product, sales, and marketing data to power personalized customer journeys at scale. It integrates natively with Salesforce, data warehouses (BigQuery, Snowflake, Redshift, Databricks), Segment, Amplitude, and web tracking to build 360-degree contact profiles. Use the Developer API (`https://api.inflection.io/v1`) to read/write contacts, manage lists, create emails, and trigger journeys programmatically. Access Inflection's AI agents via MCP (Model Context Protocol) to build audiences, emails, segments, and tokens through conversation. The platform supports both scheduled and trigger-based journeys, dynamic audience segmentation, and real-time event-driven automation.

## When to use

- **Building journeys**: Draft multi-step customer campaigns with emails, delays, branching, and Salesforce integrations
- **Managing contacts**: Create, update, batch upsert, or look up contacts via API; import CSV files; sync with Salesforce bidirectionally
- **Audience segmentation**: Build dynamic lists, segments, and journey audiences using product events, marketing activity, web behavior, account data, and Salesforce opportunities
- **Sending emails**: Create HTML emails, personalize with tokens, send at scale, manage sender domains and unsubscribe preferences
- **Event-driven automation**: Trigger journeys on form submissions or webhook events; enroll contacts in real time
- **AI-assisted workflows**: Use agents to describe audiences, draft emails, build segments, and create tokens in natural language
- **Data integration**: Sync product activity, user properties, and workspace data from CDPs or data warehouses; write journey activity back for attribution

## Quick reference

### API endpoints and authentication

| Task | Endpoint | Auth |
| --- | --- | --- |
| Look up contact | `GET /v1/contacts/by-email/{email}` or `GET /v1/contacts/{id}` | Bearer token (PAT or OAuth) |
| Create/update contact | `POST /v1/contacts` or `PATCH /v1/contacts/{id}` | Bearer token + WRITE scope |
| Batch upsert | `POST /v1/contacts/batch-upsert` | Bearer token + WRITE scope |
| Poll transaction | `GET /v1/contacts/transactions/{transactionId}` | Bearer token |
| Create list | `POST /v1/lists` | Bearer token + WRITE scope |
| Add list members | `POST /v1/lists/{listId}/members` | Bearer token + WRITE scope |
| Create email | `POST /v1/emails` | Bearer token + WRITE scope |
| Set email version | `POST /v1/email-versions` | Bearer token + WRITE scope |

### Authentication

- **Personal Access Token (PAT)**: Long-lived, scoped `READ` or `WRITE`. Create in Settings → Connected Apps. Format: `inf_pat_…`
- **OAuth 2.1**: For multi-user apps. Use PKCE flow; token acts as the authorizing user's permissions
- **Header format**: `Authorization: Bearer <token>`
- **Auth failures**: `401` = missing/invalid token; `403` = valid token but insufficient permission

### Contact properties

Contact attributes live under `properties` in responses, using **snake_case** keys:
- Standard: `first_name`, `last_name`, `email`, `company_name`, `title`
- Custom: any string, number, boolean, or date field you define
- Prefixed properties: `product_user.*`, `org.*`, `account.*` (read-only, from integrations)

### Audience filters

| Filter | Use case |
| --- | --- |
| Person property | Target by contact/user/org/account attributes (e.g., title, plan, geo) |
| Performed a product event | Target by in-product activity (e.g., signed up, activated feature) |
| Performed a marketing activity | Target by email opens, clicks, form submissions, journey enrollment |
| Performed a web activity | Target by page views, form submissions, link clicks, UTM parameters |
| List membership | Target by static list, dynamic list, or journey audience |
| Salesforce campaign membership | Target by campaign and member status |
| Has Salesforce Opportunity | Target by deal stage, amount, close date; suppress during active deals |
| Person segmentation | Target by segment category |
| Account/Org members | Target contacts by activity/properties of account or org members |

### Journey flow steps

| Step | Trigger | Scheduled | Notes |
| --- | --- | --- | --- |
| Send Email | ✅ | ✅ | Supports personalization tokens, ignore send limits |
| Add Delay | ✅ | ✅ | Pause before next step |
| Branch by filter | ✅ | ✅ | Route contacts based on criteria |
| Update Value | ✅ | ✅ | Write contact field |
| Add to Salesforce Campaign | ✅ | ✅ | Add contact to campaign with status |
| Send Internal Email Alert | ✅ | ✅ | Notify team |
| Wait for Salesforce Sync | ✅ | ✅ | Pause until Salesforce updates |

### Common API response patterns

```json
{
  "data": { /* payload */ },
  "meta": { "status": "SUCCESS", "timestamp": "..." },
  "pagination": { "pageNumber": 1, "pageSize": 20, "totalElements": 100, "totalPages": 5 }
}
```

Contact writes return `PENDING` transaction; poll until `status: DONE`.

## Decision guidance

### When to use contact vs. user journeys

| Scenario | Use contact journey | Use user journey |
| --- | --- | --- |
| One email per person (default) | ✅ | — |
| Person has multiple user IDs in same org | ✅ (collapses to one send) | — |
| Need org/user-specific personalization | — | ✅ (if intentional multi-send) |
| Grouped org logic (Org members filter) | — | ✅ (user grain required) |
| Want one email per person + user-specific content | ✅ (stamp-and-hand-off pattern) | — |

**Default to contact journeys.** Step up to user journeys only when you've accounted for duplicate-send behavior.

### When to use trigger vs. scheduled journeys

| Scenario | Trigger | Scheduled |
| --- | --- | --- |
| Instant follow-up on form/webhook | ✅ | — |
| Event-driven automation | ✅ | — |
| Newsletter or batch campaign | — | ✅ |
| Audience-based send at fixed time | — | ✅ |

### When to use dynamic list vs. segment vs. journey audience

| Scenario | Dynamic list | Segment | Journey audience |
| --- | --- | --- | --- |
| Reusable audience across journeys | ✅ | ✅ | — |
| Mutually-exclusive categories (lifecycle) | — | ✅ | — |
| One-off journey targeting | — | — | ✅ |
| Auto-refresh as data changes | ✅ | ✅ | ✅ |

### When to use PAT vs. OAuth

| Scenario | PAT | OAuth |
| --- | --- | --- |
| Server-side script or internal tool | ✅ | — |
| Multi-user SaaS app | — | ✅ |
| Automation platform (n8n, Zapier) | ✅ | ✅ (preferred) |
| Long-lived, no refresh needed | ✅ | — |
| Acts as specific user | — | ✅ |

## Workflow

### Building a trigger journey

1. **Define the trigger**: Form submission or inbound webhook. Add form field filters if needed (e.g., UTM Source = google).
2. **Set audience criteria**: Add filters to narrow enrollment (e.g., company size > 100, not already a customer). Leave empty to enroll all.
3. **Add flow steps**: Drag steps onto canvas — email, delay, branch, update value, Salesforce campaign add.
4. **Connect steps**: Drag from step handle to next step handle. Branching steps have separate handles per path.
5. **Configure each step**: Click to edit email subject, personalization tokens, delays, branch conditions.
6. **Preview and test**: Use the preview panel to check email rendering and token values.
7. **Publish**: Click Publish. Journey activates within seconds; contacts enroll in real time.

### Building an audience with the API

1. **Understand your schema**: Call `GET /v1/schema` (via MCP) or browse Data Dictionary in UI to see available fields and values.
2. **Compose filter logic**: Combine person properties, product events, marketing activity, web activity, list membership, or Salesforce data.
3. **Create dynamic list**: Use UI or MCP `ask_agent(agent_type='audience', prompt='...')` to describe the audience in natural language.
4. **Preview**: Check the list size and spot-check a few contacts to verify logic.
5. **Reference in journey**: Select the dynamic list as journey audience or use in another dynamic list.

### Syncing contacts via API

1. **Authenticate**: Get PAT from Settings → Connected Apps. Store securely.
2. **Prepare payload**: Email is required. Include any custom fields as snake_case keys under `properties`.
3. **Create or update**: `POST /v1/contacts` (create) or `PATCH /v1/contacts/{id}` (update by ID) or `POST /v1/contacts/batch-upsert` (batch).
4. **Handle async**: Response returns `transactionId` with `status: PENDING`. Poll `GET /v1/contacts/transactions/{transactionId}` until `DONE`.
5. **Check results**: Per-contact status is `CREATED`, `UPDATED`, `NO_CHANGE`, or `FAILED`.

### Sending personalized emails at scale

1. **Create email asset**: Build HTML or visual editor email in Inflection. Add personalization tokens (e.g., `{{first_name}}`).
2. **Create static list or audience**: Enroll contacts who should receive the email.
3. **Add to journey**: Create scheduled journey, select audience, add Send Email step, select email asset.
4. **Or use API**: `POST /v1/email-versions` to push per-contact versions (subject, preheader, HTML) for 1:1 sends. Each version must include `{{unsubscribe_url}}` literal token.
5. **Publish and send**: Activate journey or trigger send. Inflection handles unsubscribe, bounce, and compliance.

### Integrating with Salesforce

1. **Connect**: Connections → Add new connection → Salesforce. Log in with admin credentials.
2. **Choose sync direction**: Recommended settings auto-map standard fields. Custom settings let you map custom fields and control sync timing.
3. **Define scope**: Select which objects (Leads, Contacts, Accounts, Opportunities) and fields to sync.
4. **Map fields**: Use unified mapping to match Salesforce fields to Inflection contact properties.
5. **Start sync**: Sync runs continuously. Monitor health in Salesforce settings tab.
6. **Use in journeys**: Add contacts to Salesforce campaigns, branch on opportunity stage, suppress during active deals.

## Common gotchas

- **Contact writes are async**: `POST /v1/contacts` returns `PENDING` immediately. Always poll the transaction ID until `DONE` before assuming the contact was created.
- **Auth failures have no body**: `401` and `403` responses are empty. Check the status code, not the body. Mistyped paths also return `401` (deny-by-default), not `404`.
- **Contact not found returns 400, list not found returns 404**: Handle both error codes. Don't assume a single not-found code.
- **Contact properties use snake_case**: `first_name`, not `firstName`. Mismatched case silently fails to map.
- **Unsubscribe link is required**: Every email must include `{{unsubscribe_url}}` literal token. Inflection auto-includes it in headers; custom HTML must include it explicitly.
- **Contact journeys collapse to one send per person**: If a person has multiple user IDs, they get one email. Use user journeys only if you intentionally want multi-send.
- **Segment categories are first-match-wins**: A contact can only be in one sub-segment at a time. Order matters — most specific/high-priority segments go first.
- **Dynamic lists auto-refresh**: Filters evaluate continuously. A contact moves in/out as data changes. This is powerful but can cause unexpected re-entries into journeys.
- **Salesforce sync scope is workspace-wide**: Opportunity Matching Strategy (deal team only, account-linked, or Inflection-matched) applies to all filters. Set once in Salesforce connection settings.
- **Rate limits apply**: 429 Too Many Requests. Exponential backoff. Pace large batches instead of firing all at once.
- **Domain authentication is required**: SPF, DKIM, DMARC must be set up before sending. Unapproved domains return `400 UnapprovedEmailDomainException`.
- **Agents draft, you activate**: AI agents build assets and save them. Journeys and sends are activated in the app, not via API or MCP.
- **MCP long-polls ~12 seconds**: `ask_agent` waits for specialist agents to finish. Expect latency; don't fire rapid requests.

## Verification checklist

Before submitting work with Inflection:

- [ ] **Authentication**: PAT or OAuth token is valid and has correct scopes (READ for GET, WRITE for POST/PATCH/DELETE).
- [ ] **Contact data**: Email is present and valid. Custom fields use snake_case and match expected data types.
- [ ] **Audience logic**: Read filters aloud. Preview the list. Spot-check a few contacts. Verify AND/OR grouping is correct.
- [ ] **Journey canvas**: All steps are connected. No orphaned steps. Branching paths are wired to next steps.
- [ ] **Email**: Subject, sender, and reply-to are set. Personalization tokens are valid. Unsubscribe link is present (auto-included in header or explicit `{{unsubscribe_url}}`).
- [ ] **Sender domain**: Domain is authenticated (SPF, DKIM, DMARC). Verified in Email Settings.
- [ ] **Audience size**: Check contact count. If > 6 months old or stale, add activity criteria (signed up, opened email, etc. in past 6 months).
- [ ] **Async writes**: If using API, poll transaction until `status: DONE`. Don't assume immediate completion.
- [ ] **Rate limits**: Batch requests are paced. Not firing all at once.
- [ ] **Integrations**: Salesforce sync is healthy. Data warehouse connection is active. Webhooks are configured and tested.
- [ ] **Compliance**: Unsubscribe preferences are respected. GDPR/privacy requests are handled. Cc/Bcc used responsibly.

## Resources

- **Comprehensive page listing**: https://docs.inflection.io/llms.txt
- **API Introduction & Quickstart**: https://docs.inflection.io/api-reference/introduction
- **Building Audiences**: https://docs.inflection.io/database/building-audiences
- **Journey Canvas & Flow Steps**: https://docs.inflection.io/journeys/journey-canvas

---

> For additional documentation and navigation, see: https://docs.inflection.io/llms.txt