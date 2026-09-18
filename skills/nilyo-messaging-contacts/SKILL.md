---
name: nilyo-messaging-contacts
description: Use when the user wants to send, read or reply to WhatsApp, Telegram or Instagram messages from THEIR OWN account through Nilyo, or to look at Instagram profiles and followers: 'send a WhatsApp to Julien', 'what did Paul answer on Telegram', 'reply to the last Instagram DM from…', 'is this number on WhatsApp', 'who follows me on Instagram', 'research this creator on Instagram', voice notes, media, group chats, 'connect my WhatsApp / Telegram / Instagram'. Do not use for email (inbox skill) or for LinkedIn messages and prospecting (outreach skill).
---
# Message contacts with Nilyo

## Send to a person by name
`messaging_send_to_contact(provider, name, text)` resolves the contact on the user's own account: recent conversations first, then contacts. It sends only when exactly one person matches and returns `status: sent`. With several matches it returns the candidates (most recent conversation first): ask the user which one, then call again with the chosen `chat_id`/`attendee_id`. Never pick one yourself.

## Read and reply
1. `messaging_resolve_recipient(provider, name)` or `messaging_list_chats` → `chat.id`.
2. `messaging_list_messages(chat_id)` for context (ids, dates, who wrote what).
3. `messaging_send_message(chat_id, text)`; attachments with `message_send_native_media`, audio messages with `message_send_voice_note` (native WhatsApp voice note). Reactions: `message_add_reaction` / `message_remove_reaction`.
4. Show the draft and wait for approval before sending unless the user already gave the exact text ("send a WhatsApp to X saying …" is an approval).

## Per provider
- **WhatsApp**: `whatsapp_list_conversations`, `whatsapp_read_conversation`, `whatsapp_send_message`, `whatsapp_start_conversation` (new chat by phone number, checked first with `whatsapp_is_number_registered`), `whatsapp_list_contacts`, `whatsapp_get_profile`. Voice notes and media are native WhatsApp messages.
- **Telegram**: same generic chat tools (`messaging_*`, `message_*`) on the Telegram account; groups and channels appear in the chat list with `is_group` / `is_channel`; connection by QR or phone code (`telegram_connect`).
- **Instagram**: DMs with `instagram_list_conversations`, `instagram_read_conversation`, `instagram_send_message`; research with `instagram_get_profile(username)`, `instagram_list_followers`, `instagram_list_following`, `instagram_get_my_profile`; `instagram_update_my_profile` only on explicit request. Instagram limits new conversations with people who do not follow the user: say so instead of retrying.

## Accounts
- Several accounts of one provider: `list_connected_accounts` lists names and identifiers; pass the `account_id` the user means. Nilyo never guesses.
- Missing account: `whatsapp_connect` / `telegram_connect` return a QR code image (or a pairing code with `phone_number`); poll `account_connection_status` every 10–20 s until connected. Other providers: `account_connect(provider)` returns a secure link.
- Disconnected account (`reconnect_account`): reconnect the SAME `account_id`, never create a duplicate.

## Pacing
WhatsApp: 20 new conversations per day, 15 s between them; existing chats are not limited. Instagram: ~100 actions per day, calls serialized. Telegram: no local limit. Do not loop on `PROVIDER_ACTION_LIMIT`.
