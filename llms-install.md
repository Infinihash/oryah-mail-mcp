# Installing Oryah Mail (instructions for an AI agent)

You are installing access to Oryah Mail, a hosted MCP server that gives an AI agent governed access to email accounts and calendars its operator already owns. There is nothing to download and no local server to run. Installation means adding one remote server entry to your client's MCP configuration and completing the sign-in your operator provides.

Server facts:

- Remote endpoint: `https://mail-mcp.infinihash.com/mcp`
- Transport: HTTP (streamable HTTP MCP)
- Official MCP Registry name: `com.infinihash/oryah-mail`
- Authentication: the operator's Oryah Mail account sign-in (OAuth), or a scoped agent key the operator issues. The operator chooses the scope. A key can never exceed the permissions set on the mailbox itself.

## Step 1: Add the server to your client

Use the block that matches your client. Do not invent a different endpoint or transport.

### Claude Code

```bash
claude mcp add --transport http oryah-mail https://mail-mcp.infinihash.com/mcp
```

### Generic MCP configuration (`.mcp.json`, `mcp.json`, or your client's equivalent)

```json
{
  "mcpServers": {
    "oryah-mail": {
      "type": "http",
      "url": "https://mail-mcp.infinihash.com/mcp"
    }
  }
}
```

### Cursor

```json
{
  "mcpServers": {
    "oryah-mail": {
      "url": "https://mail-mcp.infinihash.com/mcp"
    }
  }
}
```

### Gemini CLI

```json
{
  "name": "oryah-mail",
  "mcpServers": {
    "oryah-mail": {
      "httpUrl": "https://mail-mcp.infinihash.com/mcp"
    }
  }
}
```

## Step 2: Authenticate as the operator directs

Complete the account sign-in if your client offers it, or accept the scoped agent key the operator gives you. Do not ask the operator for their mailbox password. Oryah Mail never needs it, and no step in this install uses it.

## Step 3: Verify before you do any work

List the tools the server exposes to your key. The list tells you your scope:

- A read-only key lists 13 of the 30 tools. If you see 13 tools and no send or draft tools, your key is read-only and that is correct, not an error.
- If you expected a wider scope and see fewer tools, stop and tell the operator. Do not try to work around the scope.

Then make one read-only call (for example, list the connected accounts or list recent messages on one mailbox) and confirm it returns real data.

## Rules you operate under once connected

- Sends are checked against the operator's domain allowlist before they happen. To, Cc, Bcc, and Reply-To are all checked. A send to a domain that is not on the list will be refused and the refusal will be logged under your key. A refusal is the system working, not a fault to retry around.
- Sends are idempotent. If a send call times out, check the audit log or the sent folder before retrying; a retry will not duplicate a message that already went out.
- Your key may have a daily send cap and an expiry. Hitting the cap is a stop condition, not a problem to route around.
- The operator can pause every key at once. If your calls start failing across the board, assume the pause switch was used and wait for the operator.

## If something fails

- Tools missing entirely: the server entry is wrong or the client did not reload its configuration. Recheck the endpoint URL character by character, then restart the client.
- Authentication errors: the key is expired, revoked, or paused, or the sign-in session ended. Report the exact error to the operator and stop.
- Nothing here requires, or permits, changing the permissions on the mailbox from the agent side. Scope changes are the operator's action only.
