---
name: nilyo-inbox-triage
description: Use when the user wants a summary, count, search or follow-up across THEIR OWN inboxes through Nilyo: 'what came in today', 'unanswered messages', 'find the email from Sarah', 'read the latest email', 'how many emails today', scheduled daily check-ins, drafting replies to email/LinkedIn/WhatsApp, mailbox folders and attachments on Gmail, Outlook or IMAP. Do not use for sending a single chat message by name (messaging skill) or LinkedIn prospecting (outreach skill).
---
# Inbox triage with Nilyo

## Email (Gmail, Outlook, IMAP)
- Several mailboxes: `list_connected_accounts` → pass the `account_id` the user means (Nilyo asks with a `choose_account` card otherwise).
- IMAP is read live: start with `email_list_folders` (folder ids, `total_count`, `unread_count`), then `email_list_folder_messages(folder_id, limit ≤ 20, after=…)`, then `email_read_message(email_id)`. `email_list_messages` works directly on Gmail/Outlook.
- **Counting**: "how many emails" → use the folder counts, never the length of one page. "Today" → `after` = start of the user's day in UTC ISO (`2026-09-18T00:00:00.000Z`, adjust for their timezone). An empty filtered page means no match for that filter, not an empty mailbox: say which filter was applied.
- Search: `email_list_messages (with from/to/keywords filters) or email_list_folder_messages` (keywords, from, to, dates) before reading; attachments with `email_get_attachment` (images are shown inline, other files summarized).
- Reply: `email_send` with `reply_to_message_id` for a true reply; drafts with `email_create_draft` / `email_send_draft`. On IMAP an email id changes after move/flag: reuse the id returned by the last result.

## LinkedIn and chats
- `linkedin_list_conversations` (primary inbox by default), `linkedin_read_conversation`; `messaging_list_chats` / `messaging_list_messages` for WhatsApp, Instagram, Telegram. Flag threads where the last message is not from the user as "needs an answer".

## Output
Group by channel, newest first, with sender, subject/first line, date and a one-line "why it matters"; then the list of threads needing a reply with a proposed draft each. Ask before sending anything.

## Scheduling
For a recurring check-in, tell the user to schedule this prompt in ChatGPT tasks; each run re-reads live data (no cache), so keep pages small and use `after` = last run time.
