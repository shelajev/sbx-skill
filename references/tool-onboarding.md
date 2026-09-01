# Tool onboarding

Use this workflow before adding an agent, CLI, MCP server, service integration, or development tool.

## Discover before building

Search existing project/organization kits, [Docker Hub's Sandbox Kit catalog](https://hub.docker.com/search?type=sbx_kit), and Docker's [`sbx-kits-contrib`](https://github.com/docker/sbx-kits-contrib). Check product names, executable names, and aliases; inspect current `sbx kit --help` rather than caching negative capabilities.

Before using a kit, inspect and validate it with installed CLI syntax. Review provenance, version pinning, commands, files/instructions, network access, credentials, and whether it is a sandbox or mixin kit. Ask before using an untrusted third-party kit.

If no suitable kit exists, prove the tool works in a disposable sandbox before authoring one. Use Docker's maintained [`kit-author` skill](https://github.com/docker/sbx-kits-contrib/blob/main/skills/kit-author/SKILL.md) for kit work.

## Observe, allow, verify

1. Use a restrictive policy and clone mode for untrusted code when available.
2. Run the smallest representative operation and observe denied traffic with policy logs or the dashboard.
3. Identify each destination's owner and purpose; separate auth, registries, APIs, CDNs, updates, and telemetry.
4. Add only justified sandbox-scoped hosts/ports, verify with policy inspection, and retry the operation.
5. Record only access required by every user of the capability; retest a reusable kit in a fresh sandbox.

Keep credentials in the host-side store and use proxy-managed injection. Never copy tokens into the sandbox to bypass authentication failures.
