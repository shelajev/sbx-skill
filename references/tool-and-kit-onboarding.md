# Tool and kit onboarding

Use this workflow before adding an agent, CLI, development tool, service integration, or kit. MCP servers have a distinct host execution and governance model; use [mcp.md](mcp.md) instead.

## Discover before building

Search existing project/organization kits, [Docker Hub's Sandbox Kit catalog](https://hub.docker.com/search?type=sbx_kit), and Docker's [`sbx-kits-contrib`](https://github.com/docker/sbx-kits-contrib). Check product names, executable names, and aliases; inspect current `sbx kit --help` rather than caching negative capabilities.

Before using a kit, inspect and validate it with installed CLI syntax. Review provenance/signature, version pinning, commands, files/instructions, network access, credentials and bindings, host or sandbox effects, and whether it is a sandbox or mixin kit. Ask before using an untrusted third-party kit.

If no suitable kit exists, prove the tool works in a disposable sandbox before authoring one. For kit work, use Docker's maintained `kit-author` skill only if it is installed. Otherwise consult its [repository documentation](https://github.com/docker/sbx-kits-contrib/blob/main/skills/kit-author/SKILL.md); do not assume or silently install a remote skill.

## Observe, allow, verify

1. Use a restrictive policy and clone mode for untrusted code when available.
2. Run the smallest representative operation and observe denied traffic with policy logs or the dashboard.
3. Identify each destination's owner and purpose; separate auth, registries, APIs, CDNs, updates, and telemetry.
4. Add only justified sandbox-scoped hosts/ports, verify with policy inspection, and retry the operation.
5. Record only access required by every user of the capability; retest a reusable kit in a fresh sandbox.

Keep credentials in the host-side store and use proxy-managed injection. Never copy tokens into the sandbox to bypass authentication failures. Third-party schema-v2 kits require credential bindings; built-in credentials are authorized through provenance. Read [credentials.md](credentials.md) when authentication is involved.
