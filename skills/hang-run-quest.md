---
name: Opt a member into a quest and claim the prize
description: Discover a program's quests, opt a Hang member into a quest, track their opt-in progress, and claim the quest prize when complete.
api: openapi/hang-partner-api-openapi.yml
operations:
  - get_v2_admin_quests
  - post_v2_program-memberships_program_membership_id_quests_quest_id_opt-in
  - get_v2_program-memberships_program_membership_id_quests_opt-ins
  - post_opt-ins_opt_in_id_claim-quest-prize
---

# Opt a member into a quest and claim the prize

Base URL: `https://loyalty.hang.xyz/partner-api`
Auth: `X-API-Key` header on every request.

## Steps

1. **List the program's quests** — `get_v2_admin_quests` (`GET /v2/admin/quests`) returns every quest for the program tied to your API key; choose a `quest_id`.
2. **Opt the member in** — `post_v2_program-memberships_program_membership_id_quests_quest_id_opt-in` (`POST /v2/program-memberships/{program_membership_id}/quests/{quest_id}/opt-in`). This creates an opt-in with an `opt_in_id`.
3. **Track progress** — `get_v2_program-memberships_program_membership_id_quests_opt-ins` (`GET /v2/program-memberships/{program_membership_id}/quests/opt-ins`) lists the member's opt-ins and their status. Progress is also pushed via the `quest_opt_in.updated` webhook (see `asyncapi/hang-webhooks.yml`).
4. **Claim the prize** when the quest completes — `post_opt-ins_opt_in_id_claim-quest-prize` (`POST /opt-ins/{opt_in_id}/claim-quest-prize`).

## Notes
- Points that drive quest progress are recorded as activities — see the `hang-enroll-and-record-activity` skill and pass an `idempotency_key`.
- `delete_v2_program-memberships_program_membership_id_opt-ins_opt_in_id` clears a quest opt-in status but **only works in dev/staging**.
