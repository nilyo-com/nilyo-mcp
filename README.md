# Nilyo — your own LinkedIn, WhatsApp, Instagram, Telegram, Email and Calendar, from any agent

[Nilyo](https://nilyo.com) is a remote MCP server that connects the accounts you already use to ChatGPT, Claude, Cursor, OpenClaw, Hermes, n8n or any MCP-compatible agent, so it can **search, read and act for you on your own accounts** — no browser automation.

- **Endpoint:** `https://nilyo.com/mcp` (Streamable HTTP)
- **Auth:** OAuth 2.1 with dynamic client registration (sign in or create your Nilyo account on the authorization page; 7-day free trial, no card). Personal tokens for runtimes without OAuth.
- **Tools:** 170+ intent-named tools with titles and MCP annotations (read-only / destructive / open-world), exact ID resolution, corrective errors, human-like pacing per provider.
- **Registry:** `com.nilyo/nilyo` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.nilyo/nilyo) · [Glama](https://glama.ai/mcp/connectors/com.nilyo/nilyo) · [Smithery](https://smithery.ai/servers/arnaud-ehq0/nilyo) · [mcp.so](https://mcp.so/servers/nilyo)

This repository holds the distribution kit (plugin manifests, skills, guides). The service itself is operated by [Unipile SAS](https://nilyo.com/legal-notice).

## What your agent can do

| Provider | Examples |
| --- | --- |
| **LinkedIn** | search people, companies, posts and jobs; "who do I know at Stripe?"; turn a profile URL into a ready-to-send message or invitation; read and answer conversations; publish, comment, react; Sales Navigator and Recruiter when your account has them; job postings and applicants |
| **WhatsApp** | "send a WhatsApp to Julien saying I'm running late"; read and reply to chats; voice notes, media, groups |
| **Instagram, Telegram** | direct messages; Instagram profiles and followers; Telegram groups and channels |
| **Email** | Gmail, Outlook and any IMAP mailbox: folders, threads, search, attachments, drafts, send and reply |
| **Calendar** | Google and Microsoft calendars: list, create, update events |
| **Cross-channel** | check your email history before replying on LinkedIn; confirm a LinkedIn exchange on WhatsApp; a morning brief of every inbox with the replies to approve |

## Connect

### Claude (claude.ai, Claude Desktop, Claude Code)
Settings → Connectors → *Add custom connector* → `https://nilyo.com/mcp` → sign in on the Nilyo page. Claude Code: `claude mcp add --transport http nilyo https://nilyo.com/mcp`.

### ChatGPT
Settings → Plugins (Developer mode) → add MCP server `https://nilyo.com/mcp` with OAuth, or install the published **Nilyo** plugin from the directory. The portable plugin package is in [`chatgpt/`](chatgpt/) and the skills in [`skills/`](skills/).

### Cursor
This repository is a Cursor plugin (`.cursor-plugin/plugin.json` + `mcp.json`): install it from the Cursor marketplace, or add to `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "nilyo": { "url": "https://nilyo.com/mcp" } } }
```

### OpenClaw, Hermes, Codex and other MCP runtimes
Point the runtime at `https://nilyo.com/mcp`. Runtimes without an OAuth flow can use a personal token created on [nilyo.com/account](https://nilyo.com/account) (*Agent access*) as `Authorization: Bearer ab_…`. The generic skill for these runtimes is [`skills/nilyo/SKILL.md`](skills/nilyo/SKILL.md).

### n8n
Community node `n8n-nodes-nilyo` (Nilyo credential = personal token; *Nilyo* node for actions, *Nilyo Trigger* for realtime events). Realtime events can also be delivered to any HTTPS receiver through the `webhook_*` tools.

## Skills

Task-oriented skills (SKILL.md + `agents/openai.yaml`), usable in ChatGPT, Codex, Claude Code and OpenClaw:

| Skill | Use it for |
| --- | --- |
| `nilyo-linkedin-outreach` | prospecting, network, messages and invitations |
| `nilyo-linkedin-content` | posts, comments, reactions, engagement routines |
| `nilyo-recruiting` | applicants, resumes, Recruiter projects, sourcing |
| `nilyo-messaging-contacts` | WhatsApp, Telegram and Instagram messages by contact name |
| `nilyo-inbox-triage` | summaries, counts, unanswered messages, scheduled check-ins |
| `nilyo-account-setup` | connecting and reconnecting accounts, plan |

## Good to know

- Nilyo never guesses between two of your accounts: the agent lists them and passes the `account_id` you mean.
- Sends, posts, invitations and deletions are separate tools flagged `destructiveHint`; agents show the text and wait for approval.
- Pacing: LinkedIn and Instagram ~100 actions/day per account, WhatsApp 20 new conversations/day, calls serialized per account.
- Accounts are connected once through a secure link (Hosted Auth) or a WhatsApp/Telegram QR code shown in the conversation; credentials never transit through the agent.

Setup guide: https://nilyo.com/setup-for-agents · Pricing: https://nilyo.com/pricing · Privacy: https://nilyo.com/privacy · Support: https://nilyo.com/support
