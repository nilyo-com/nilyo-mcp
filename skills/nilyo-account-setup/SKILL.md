---
name: nilyo-account-setup
description: Use when the user wants to connect, reconnect, list or remove accounts on Nilyo, scan a WhatsApp/Telegram QR code, connect an IMAP mailbox with its login, check why an account is disconnected, or understand their Nilyo plan and trial. Do not use for the tasks themselves (messaging, email, LinkedIn skills).
---
# Connect accounts to Nilyo

## First use
Nilyo is the user's own Nilyo account (created during the ChatGPT sign-in, 7-day free trial, no card). Start with `list_connected_accounts`: it shows provider, display name, identifier, status and whether reconnection is needed.

## Connect
- LinkedIn, Instagram, Gmail, Outlook, calendars: `account_connect(provider)` → a `connect_account` card with a secure link (Hosted Auth). The user signs in on the provider side; ChatGPT never sees credentials. Then `account_connection_status` and retry the original request.
- WhatsApp: `whatsapp_connect` → QR code image to scan from WhatsApp › Linked devices (or `phone_number` for a pairing code). Poll `account_connection_status` every 10–20 s; the QR refreshes automatically.
- Telegram: `telegram_connect` (QR or phone code).
- Instagram: `account_connect(provider="instagram")` (secure link; Instagram may ask for a verification code on the provider side, never in the chat).
- Generic IMAP: `imap_connect(email, password, imap_host, smtp_host…)` when the user gives the mailbox credentials in the conversation; they are forwarded once and never stored by ChatGPT.

## Reconnect
A `reconnect_account` result names the account: reconnect the SAME `account_id` (`account_connect(provider, account_id)`, `whatsapp_connect(account_id)`). Never create a second account for the same identity. `account_list_attention` lists everything needing action.

## Plan
`account_get_subscription` explains plan, trial end, limits and seats. In ChatGPT, plan changes and payment happen on the Nilyo website (account page); never show a payment link or ask for card details here.

## Feedback
If something fails unexpectedly, offer to report it: `feedback_report_bug` (error code + details) or `feedback_request_feature`, only with the user's consent.
