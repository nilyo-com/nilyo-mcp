---
name: nilyo-setup
description: Use when the Nilyo plugin was just installed or the Nilyo MCP is not connected yet, when a Nilyo tool answers that the user must sign in, connect an account or reconnect one, or when the user asks how to set up Nilyo. Guides the OAuth sign-in and the connection of LinkedIn, WhatsApp, Instagram, Telegram, email or calendar accounts.
---
# Set up Nilyo

Nilyo is a remote MCP server (`https://nilyo.com/mcp`) that gives Claude the user's own accounts. Setup is two steps and never involves pasting credentials in the chat.

## 1. Sign in to Nilyo (once per Claude client)
- The first Nilyo tool call triggers OAuth: Claude opens `nilyo.com`, the user signs in or creates an account (7-day free trial, no card), then clicks **Allow**. If Claude Code shows the MCP as needing authentication, run `/mcp` and choose **Authenticate** for `nilyo`.
- Nothing else to configure: no API key, no client id.

## 2. Connect the accounts the user needs
Call `list_connected_accounts` first. For a missing provider:
- LinkedIn, Instagram, Gmail, Microsoft 365/Outlook, calendars → `account_connect(provider)` returns a secure link; the user signs in on the provider side.
- WhatsApp → `whatsapp_connect` returns a QR code image to scan from WhatsApp › Linked devices (or a pairing code with `phone_number`); poll `account_connection_status` every 10–20 s.
- Telegram → `telegram_connect` (QR or phone code).
- Generic IMAP mailbox → `imap_connect` with the mailbox login only if the user gives it explicitly; otherwise prefer the secure link.
Then retry the user's original request unchanged.

## Reconnection
A `reconnect_account` result names the account: reconnect the SAME `account_id` (`account_connect(provider, account_id)` / `whatsapp_connect(account_id)`), never a duplicate. `account_list_attention` lists everything that needs action.

## Rules
- Never ask for provider passwords in the chat (except the explicit IMAP case above).
- Several accounts of one provider: Nilyo lists them by name; pass the `account_id` the user means, never guess.
- Plan and billing questions: `account_get_subscription`; plans are managed on nilyo.com.
