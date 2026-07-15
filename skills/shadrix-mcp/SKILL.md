---
name: shadrix-mcp
description: Use the hosted Shadrix MCP to plan, compose, discover, and validate production UI against its real component kits. Trigger for dashboards, admin panels, CRM, analytics, e-commerce, forms, navigation, design-system work, UI refactors, new Next.js/React interfaces, requests mentioning Shadrix tools, or whenever an agent might otherwise invent components that are absent from the selected kit.
---

# Shadrix MCP

Use MCP tools before writing UI. Treat tool results as the source of truth for kit components, helpers, tokens, examples, and gaps. Let the local coding agent inspect and edit the repository; Shadrix does not edit files itself.

## Connect first

If Shadrix tools are unavailable, stop and read [client setup](references/client-setup.md). Never place an API key in this skill, source files, logs, URLs, or committed client configuration.

Run `health_check` before UI work. If it fails, report the exact failure and do not pretend subsequent tools ran. A generic message such as "resources are unsupported" does not prove the Shadrix tools are unavailable: first verify the configured URL ends in `/mcp`, reconnect the server, and check the client's MCP status or logs.

## Choose the workflow

- For a product, multi-page feature, dashboard, or ambiguous specification: call `plan_ui`.
- For one known screen or component: call `resolve_feature`.
- For a new repository or uncertain stack: call `get_bootstrap_guide`, then `diagnose_stack` with the actual package manifest and global CSS.
- For discovery: search the catalog before implementing a component freehand.

## Build from evidence

1. Read the returned pages, navigation, states, components, helpers, tokens, `openQuestions`, and `kitGaps`.
2. Resolve material open questions with the user when they change product behavior.
3. Call `compose_page` for each planned page and reuse `assembled.files`, `mountedExample`, `codeSnippets`, imports, and canonical paths.
4. Adapt domain data and copy without replacing the documented component structure with invented wrappers.
5. Implement every `kitGap` explicitly as custom project code. Never claim a gap is part of the Shadrix kit.
6. Call `check_fidelity` with the resulting code and relevant use case. Apply safe fixes and rerun until it passes or report the remaining findings.

Keep calls focused. Do not enumerate the whole catalog when `plan_ui`, `resolve_feature`, or `search_by_example` can answer the current decision directly. Preserve the returned plan object because the hosted profile is stateless and `compose_page` needs that plan as input.

## Preserve stack compatibility

- Follow the stack diagnosis returned by the MCP.
- For the Radix-based kit, preserve `asChild` trigger composition and `hsl(var(--token))` tokens.
- Do not run `shadcn@latest init` over an existing Shadrix kit or silently migrate primitives.
- Prefer canonical source returned by the MCP over remembered library snippets.
- Keep accessibility, loading, empty, error, mobile, and destructive-action states in the implementation.

## Handle incomplete coverage

When a requested component does not exist:

1. Confirm the absence through discovery or the returned `kitGaps`.
2. Reuse the nearest supported primitive or pattern.
3. Implement only the missing layer as local project code.
4. Label the custom code clearly in the handoff.
5. Validate the final composition with `check_fidelity`.

## Handoff

State which Shadrix tools ran, which kit and canonical artifacts were used, any unresolved `kitGaps`, and whether fidelity validation passed. Never describe a tool call as successful without its actual result.
