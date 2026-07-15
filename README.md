# Shadrix Skills

Official agent skills for the hosted [Shadrix MCP](https://shadrix.xyz).

## Install

Discover the available skills:

```bash
npx skills add guilhermemarch/shadrix-skills --list
```

Install the Shadrix MCP skill for your detected coding agents:

```bash
npx skills add guilhermemarch/shadrix-skills --skill shadrix-mcp
```

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
