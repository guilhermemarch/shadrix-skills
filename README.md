# Shadrix Skills

Official agent skills for the hosted [Shadrix MCP](https://shadrix.xyz).

## Install without a package registry

Clone the public repository and copy the skill into the directory used by your agent:

```bash
git clone --depth 1 https://github.com/guilhermemarch/shadrix-skills.git .shadrix-skills
mkdir -p .agents/skills
cp -R .shadrix-skills/skills/shadrix-mcp .agents/skills/shadrix-mcp
```

Replace `.agents/skills/` when your client uses a dedicated location:

| Client | Project location |
|---|---|
| Claude Code | `.claude/skills/shadrix-mcp/` |
| Cursor | `.cursor/skills/shadrix-mcp/` |
| OpenCode | `.opencode/skills/shadrix-mcp/` |
| Codex and generic agents | `.agents/skills/shadrix-mcp/` |

The skill and the MCP connection are separate. Configure your client with the Streamable HTTP endpoint:

```text
https://mcp.shadrix.xyz/mcp
```

Authenticate with `Authorization: Bearer <SHADRIX_API_KEY>`. Keep the key in an environment variable or secure credential store; never commit it to a project or this repository.

Full client examples and troubleshooting are in [`skills/shadrix-mcp/references/client-setup.md`](skills/shadrix-mcp/references/client-setup.md).

## Contents

- `skills/shadrix-mcp`: teaches coding agents to plan, compose, and validate UI using the actual Shadrix component catalog.

## License

MIT
