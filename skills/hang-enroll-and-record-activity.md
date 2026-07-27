---
name: Enroll a member and record a point-earning activity
description: Create (or find) a Hang program membership for a user and record an activity that changes their points, using an idempotency key so retries never double-credit.
api: openapi/hang-partner-api-openapi.yml
operations:
  - post_v2_program-memberships
  - get_v2_program-memberships_search
  - post_v2_program-memberships_program_membership_id_activities
  - get_v2_program-memberships_program_membership_id_level
---

# Enroll a member and record a point-earning activity

Base URL: `https://loyalty.hang.xyz/partner-api`
Auth: send your per-program key as the `X-API-Key` header on every request.

## Steps

1. **Find an existing membership** — `get_v2_program-memberships_search` (`GET /v2/program-memberships/search`) with the `external_user_id` query param. If found, keep its `program_membership_id`.
2. **Create the membership** if none exists — `post_v2_program-memberships` (`POST /v2/program-memberships`) with a `wallet_address`, `external_user_id`, or both (form-encoded).
3. **Record the activity** — `post_v2_program-memberships_program_membership_id_activities` (`POST /v2/program-memberships/{program_membership_id}/activities`). Required form fields: `activity[transaction_timestamp]`, `activity[value]`, `activity[activity_type_id]`, and an `idempotency_key`.
   - **Always pass a stable `idempotency_key`** (e.g. your order/transaction id). Retrying the same key will not double-credit points. This is the one idempotency guarantee Hang documents.
   - For an immediate result rather than async processing, use `post_v2_program-memberships_program_membership_id_activies_synchronous`.
4. **Confirm the balance** — `get_v2_program-memberships_program_membership_id_level` (`GET /v2/program-memberships/{program_membership_id}/level`) returns the member's current level/tier and earned points (XP).

## Notes
- Requests are `application/x-www-form-urlencoded` / `multipart/form-data`, not JSON bodies.
- Error bodies are plain JSON `{ "message": ... }`; 409 means a conflicting/duplicate resource (see `errors/hang-problem-types.yml`).
