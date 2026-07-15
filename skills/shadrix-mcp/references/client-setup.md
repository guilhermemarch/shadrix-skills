# Client setup

The MCP connection and this skill are separate:

- The MCP connection provides authenticated tools, resources, and prompts.
- The skill teaches the coding agent when and how to call those capabilities.

Installing only one of them is not enough. Use this exact Streamable HTTP endpoint:

```text
https://mcp.shadrix.xyz/mcp
```

Send the user-created key as:

```text
Authorization: Bearer <SHADRIX_API_KEY>
```

Prefer an environment variable or the client's secure credential store. Never commit the key.

## OpenCode

Add this to `opencode.jsonc` and restart or reconnect the MCP server:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "shadrix": {
      "type": "remote",
      "url": "https://mcp.shadrix.xyz/mcp",
      "enabled": true,
      "oauth": false,
      "timeout": 30000,
      "headers": {
        "Authorization": "Bearer {env:SHADRIX_API_KEY}"
      }
    }
  }
}
```

OpenCode's Streamable HTTP client supplies the protocol `Accept` header. Do not add a conflicting single-value `Accept: application/json` header.

## Claude Code

```bash
claude mcp add --transport http --scope user \
  --header "Authorization: Bearer $SHADRIX_API_KEY" \
  shadrix https://mcp.shadrix.xyz/mcp

claude mcp get shadrix
```

## VS Code

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "shadrix-key",
      "description": "Shadrix API key",
      "password": true
    }
  ],
  "servers": {
    "shadrix": {
      "type": "http",
      "url": "https://mcp.shadrix.xyz/mcp",
      "headers": {
        "Authorization": "Bearer ${input:shadrix-key}"
      }
    }
  }
}
```

## Generic Streamable HTTP diagnostic

This checks the protocol boundary without exposing a key in the URL:

```bash
curl -i https://mcp.shadrix.xyz/mcp \
  -H "Authorization: Bearer $SHADRIX_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  --data '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"diagnostic","version":"1.0.0"}}}'
```

## Troubleshooting

| Symptom | Meaning | Action |
|---|---|---|
| `404 Cannot POST /` | The client is using the domain root. | Change the URL to `https://mcp.shadrix.xyz/mcp`. |
| `401 Unauthorized` | The bearer key is absent, malformed, revoked, or unknown. | Create a new key, set `SHADRIX_API_KEY`, and reconnect without logging it. |
| `403 Forbidden` | The key authenticated but lacks the capability's scope. | Use the hosted read profile or request the required scope; do not retry blindly. |
| `406 Not Acceptable` | The client did not negotiate Streamable HTTP correctly. | Upgrade the MCP client; for manual HTTP tests send both accepted response types. |
| `429 Too Many Requests` | A safety limit or quota was reached. | Respect `Retry-After`, reduce parallel calls, and retry later. |
| "No tools" or "resources unsupported" | Often an initialization/configuration failure, not proof of an empty server. | Verify `/mcp`, inspect MCP logs/status, reconnect, then run `health_check`. |

## Skill locations

Extract the `shadrix-mcp` directory into the location supported by the client:

| Client | Project location |
|---|---|
| Claude Code | `.claude/skills/shadrix-mcp/` |
| Cursor | `.cursor/skills/shadrix-mcp/` |
| OpenCode | `.opencode/skills/shadrix-mcp/` |
| Codex and generic agents | `.agents/skills/shadrix-mcp/` |

After both pieces are installed, ask the agent to run `health_check`. A successful response is the readiness check; a successful `curl` alone does not prove the coding client registered the tools.
