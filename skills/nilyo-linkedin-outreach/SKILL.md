---
name: nilyo-linkedin-outreach
description: Use when the user wants to find, research, message, invite or follow up with people or companies on THEIR OWN LinkedIn through Nilyo: prospecting, recruiting, 'who do I know at X', 'open this LinkedIn profile URL', 'send a connection request', 'reply on LinkedIn', Sales Navigator or Recruiter searches, publishing or commenting posts. Do not use for generic LinkedIn advice without an account action, or for WhatsApp/email tasks (see the other Nilyo skills).
---
# LinkedIn outreach with Nilyo

Nilyo gives ChatGPT the user's own LinkedIn account (tools `linkedin_*`). Work with exact provider IDs, one call at a time, and confirm before anything is sent.

## Chain
1. **Account**: if the user names an account ("my recruiting LinkedIn"), take its `account_id` from `list_connected_accounts`; otherwise let Nilyo pick (it asks with a `choose_account` card when several exist).
2. **Resolve people**: profile URL → `linkedin_get_profile_from_url` → `profile.id` (keep it for every later action). Name → `linkedin_search_people` (keywords, location, company) → show a shortlist when several match; never guess.
3. **Companies**: `linkedin_search_companies` / `linkedin_get_company` → `company.id`; employees via `linkedin_search_people` with the company filter; warm paths via `linkedin_list_my_connections`.
4. **Context before writing**: `linkedin_list_conversations` / `linkedin_read_conversation` with that person, their recent posts (`linkedin_list_user_posts`) if relevant.
5. **Write**: draft the message or invitation note, show it, wait for an explicit "send", then `linkedin_send_message` (existing chat), `linkedin_start_conversation` (new chat, inbox chosen explicitly for Sales Navigator/Recruiter) or `linkedin_send_invitation` (note ≤ 300 characters).
6. **Posts**: `linkedin_create_post`, `linkedin_comment_on_post`, `linkedin_react_to_post` (reaction `linkedin_like`, …) only after the user approves the text.

## Rules
- Pacing: LinkedIn calls are serialized with a daily budget (~100 actions including profile views and searches). Prefer one precise search over many broad ones; never bulk-invite. Explain the budget when the user asks for hundreds of actions and propose a small reviewed batch.
- `PROFILE_NOT_ACCESSIBLE` = private/restricted profile, do not retry the same identifier.
- Results carry `next_tools` and corrective errors: follow them instead of retrying blindly.
- Never paste tokens, cookies or credentials; the account is connected through a secure link (`account_connect(provider="linkedin")`) when missing, reconnected with the same `account_id` when `reconnect_account` is returned.
