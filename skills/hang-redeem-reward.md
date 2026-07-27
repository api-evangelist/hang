---
name: Redeem a reward for a member
description: List the rewards available to a Hang program membership, generate a redemption code for a chosen reward, and redeem that code.
api: openapi/hang-partner-api-openapi.yml
operations:
  - get_v2_program-memberships_program_membership_id_rewards
  - post_v2_program-memberships_program_membership_id_redemptions
  - post_v2_redemptions_redemption_uuid_redeem
  - get_v2_program-memberships_program_membership_id_redemptions
---

# Redeem a reward for a member

Base URL: `https://loyalty.hang.xyz/partner-api`
Auth: `X-API-Key` header on every request.

## Steps

1. **List the member's rewards** — `get_v2_program-memberships_program_membership_id_rewards` (`GET /v2/program-memberships/{program_membership_id}/rewards`) to see which rewards the member holds and pick a `reward_id`.
2. **Generate a redemption code** — `post_v2_program-memberships_program_membership_id_redemptions` (`POST /v2/program-memberships/{program_membership_id}/redemptions`) with the `reward_id`. The response carries a redemption record with a `uuid`.
3. **Redeem the code** — `post_v2_redemptions_redemption_uuid_redeem` (`POST /v2/redemptions/{redemption_uuid}/redeem`) using the `uuid` from step 2 to mark the reward redeemed.
4. **Verify** — `get_v2_program-memberships_program_membership_id_redemptions` (`GET /v2/program-memberships/{program_membership_id}/redemptions`) returns the member's redemption history.

## Notes
- A `422` means the reward is not redeemable; a `404` means the redemption code / membership was not found (see `errors/hang-problem-types.yml`).
- Wallet-based flows have parallel operations under `/wallets/{wallet_address}/redemptions`.
