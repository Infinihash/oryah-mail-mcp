# Oryah Mail MCP

Governed email and calendar for AI agents, exposed as a [Model Context Protocol](https://modelcontextprotocol.io) server.

Oryah Mail gives an AI agent a real mailbox with guardrails: per-mailbox permissions, a send allowlist, scoped API keys, and a full audit log. Connect Gmail, Outlook, or IMAP accounts once, then let the agent read, search, draft, send, and manage calendar events through 29 MCP tools.

**Endpoint:** `https://mail-mcp.infinihash.com/mcp` (Streamable HTTP)

## Tools

**Mail**

- `send_mail`, `reply`, `forward` — send and reply (subject to the send allowlist)
- `create_draft`, `update_draft`, `send_draft` — draft lifecycle
- `search`, `read`, `thread` — find and read messages and threads
- `inbox` — recent mail across accounts
- `move_message`, `label_message`, `mark_read`, `archive`, `delete_message` — organize

**Calendar**

- `events` — list events
- `create_event`, `update_event`, `delete_event`, `respond_to_event` — manage events
- `find_free_time` — availability lookup

**Accounts**

- `accounts` — list connected accounts

## Authentication

Every request carries `Authorization: Bearer <API_KEY>`. Keys are scoped per mailbox with granular permissions. Get a key from your Oryah Mail account — see [the InfiniHash App Store](https://apps.infinihash.com).

## Use with Claude

Add the remote server in your MCP client:

```json
{
  "mcpServers": {
    "oryah-mail": {
      "url": "https://mail-mcp.infinihash.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_ORYAH_MAIL_API_KEY"
      }
    }
  }
}
```

## Registry

Published to the [official MCP registry](https://registry.modelcontextprotocol.io) as `io.github.Infinihash/oryah-mail-mcp` via `mcp-publisher` from `server.json` in this repo.
