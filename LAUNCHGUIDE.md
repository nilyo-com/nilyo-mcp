# Nilyo

## Tagline
Your own LinkedIn, WhatsApp, Instagram, Telegram, Email and Calendar accounts, usable from any agent.

## Description
Nilyo is a hosted (remote) MCP server that connects the accounts you already use to Claude, ChatGPT, Cursor, OpenClaw, Hermes, n8n or any MCP-compatible agent, so the agent can search, read and act for you on your own accounts — no browser automation, no scraping.

Reach the right people on LinkedIn (people, companies, posts and jobs; your network; a profile URL turned into a ready-to-send message or invitation; publishing and comments; Sales Navigator and Recruiter when your account has them). Send a WhatsApp to a contact by name, read and answer Instagram and Telegram messages, send voice notes and media. Read, search and send email on Gmail, Outlook or any IMAP mailbox — folders, threads, attachments, drafts — and work with your Google or Microsoft calendar. Combine channels: check your email history before replying on LinkedIn, or get a morning digest of every inbox with the replies to approve.

Built for agents: 170+ intent-named tools with MCP annotations (read-only / destructive / open-world), exact ID resolution before any action, corrective errors instead of guesses, human-like pacing per provider, and no guessing between two of your accounts. Accounts are connected once through a secure link or a WhatsApp/Telegram QR code shown in the conversation; credentials never transit through the agent. For individuals and teams; operated by Unipile SAS (France), the company behind the Unipile API.

## Setup Requirements
- No API key or environment variable. Connect to `https://nilyo.com/mcp` (Streamable HTTP): the server uses OAuth 2.1 with dynamic client registration and client ID metadata documents — sign in or create your Nilyo account on the authorization page (7-day free trial, no card). https://nilyo.com/setup-for-agents
- Optional: a personal bearer token (`ab_…`, created at https://nilyo.com/account → Agent access) for runtimes without an OAuth flow, sent as `Authorization: Bearer <token>`.
- Then connect at least one provider account (LinkedIn, WhatsApp, Instagram, Telegram, Gmail, Microsoft 365/Outlook, IMAP, Google/Microsoft calendar) from the conversation or the Nilyo dashboard.

## Category
Communication

## Features
- Search LinkedIn people, companies, posts and jobs from your own account; see who you already know at a company
- Resolve a LinkedIn profile URL or a name to a stable profile ID before any action
- Read and answer LinkedIn conversations (classic, Sales Navigator and Recruiter inboxes); send invitations with a note
- Publish LinkedIn posts, comment, reply and react; list reactions and comments on your posts
- Recruiting: job postings, applicants and resumes; Recruiter projects and sourcing
- WhatsApp: send to a contact by name, read chats, voice notes, media, groups; check whether a number is on WhatsApp
- Instagram and Telegram direct messages, profiles, followers, groups and channels
- Email on Gmail, Outlook and any IMAP mailbox: folders, listing, search, read with attachments, drafts, send and true replies, move and label
- Google and Microsoft calendars: list, create, update, cancel events, RSVPs
- Cross-channel workflows: email history before a LinkedIn reply, LinkedIn → WhatsApp follow-up, daily digest of every inbox
- Account lifecycle in the conversation: secure connection link, WhatsApp/Telegram QR code or pairing code, reconnection of the same account
- Realtime events (new message, new email, account status) delivered to webhook destinations for persistent agents and n8n
- MCP annotations on every tool (title, readOnlyHint, destructiveHint, openWorldHint); reads of your own data run without confirmation prompts
- Human-like pacing and daily budgets per account (about 100 LinkedIn actions, 20 new WhatsApp conversations) so accounts stay safe
- Never picks between two of your accounts on its own; never sends to an ambiguous contact
- Bug and feature-request tools so the agent can report an issue to the Nilyo team with your consent
- Per-user isolation: one connection scope and one encrypted key per user; team plans centralize billing only

## Getting Started
- "Who do I know at Stripe on LinkedIn? Draft a short intro message to the closest contact."
- "Send a WhatsApp to Julien saying I'll be 10 minutes late."
- "Check my email, LinkedIn and WhatsApp since yesterday, summarize what came in and list the messages that still need an answer."
- "Open https://www.linkedin.com/in/… and tell me what this person does, then check if we ever emailed."
- "Show the latest posts from my top prospects and draft a comment for each."
- Tool: list_connected_accounts — Lists your connected accounts with provider, owner name, identifier and status; use its account_id when you have several accounts of one provider.
- Tool: linkedin_get_profile_from_url — Turns a LinkedIn profile URL into the profile and its stable ID before a message or invitation.
- Tool: messaging_send_to_contact — Sends a WhatsApp/Telegram/Instagram message to a person by name; asks when two people match.
- Tool: email_list_folder_messages — Lists messages in one mailbox folder with date filters; pair with email_read_message.
- Tool: account_connect / whatsapp_connect — Returns a secure connection link or a QR code to connect (or reconnect) an account from the conversation.

## Tags
linkedin, whatsapp, instagram, telegram, email, gmail, outlook, imap, calendar, messaging, sales, prospecting, recruiting, crm, inbox, productivity, communication, oauth, remote, agent, chatgpt, claude, cursor, n8n

## Documentation URL
https://nilyo.com/setup-for-agents

## Health Check URL
https://nilyo.com/health
