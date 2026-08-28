# Tool onboarding

Use this workflow before adding any agent, CLI, MCP server, service integration, or development tool to Docker Sandboxes.

## Discover before building

Search in this order:

1. Existing kits in the current workspace or organization.
2. [Docker Hub's Sandbox Kit catalog](https://hub.docker.com/search?type=sbx_kit), searching the product name, executable name, and common aliases.
3. Docker's [`sbx-kits-contrib`](https://github.com/docker/sbx-kits-contrib) repository, including open pull requests when appropriate.
4. The tool vendor's repositories and trusted community sources.

There is no documented `sbx kit search` command as of this writing; check current `sbx kit --help` in case one has been added. Do not conclude that no kit exists from a single exact-name search.

If a kit exists, inspect it before use with the current `sbx kit inspect`/`validate` workflow. Review provenance, publisher, source, version pinning, install and startup commands, files, agent instructions, requested network access, credential injection, and whether it is a sandbox or mixin kit. Prefer a maintained, signed or verifiable, narrowly scoped kit over recreating it. Ask before using an untrusted third-party kit.

If no suitable kit exists, onboard experimentally first; create a kit only after the working behavior and required access are understood. For authoring, load Docker's maintained [`kit-author` skill](https://github.com/docker/sbx-kits-contrib/blob/main/skills/kit-author/SKILL.md).

## Observe, review, allow, verify

1. Start from `balanced` or `deny-all`, according to the user's security needs. Do not use allow-all for discovery.
2. Scope the experiment to a disposable named sandbox and use clone mode for an untrusted project when supported.
3. Install or run the smallest meaningful operation: first `--version`/`--help`, then authentication if needed, then one representative task.
4. Watch the TUI network panel (`sbx` or `sbx tui`) or `sbx policy log` for denied connections.
5. For each destination, identify its owner and purpose. Check redirects, updater/CDN hosts, telemetry, model APIs, authentication endpoints, and package registries separately. Do not allow a host merely because the tool attempted it.
6. Reject unnecessary telemetry and suspicious or unexplained destinations. Ask the user when purpose or trust is unclear.
7. Add the narrowest sandbox-scoped hostname and port rule, then use `sbx policy check network --sandbox <name> <host:port>` and retry only the operation that required it.
8. Repeat until the representative workflow succeeds and no unexplained traffic remains.
9. Record the justified runtime, install-time, authentication, and update hosts. Move those rules into the maintained kit only when they belong to every user of that capability.
10. Re-test the kit in a fresh disposable sandbox under `deny-all`. Verify both success on required destinations and denial of undeclared destinations.

Keep real credentials in sbx's host-side secret store and use proxy-managed injection. Never copy them into the sandbox to get past an authentication failure.

## Local examples

This workspace currently contains `agy-kit`, an Antigravity CLI agent kit, and `exa`, an Exa MCP mixin kit. Treat them as candidates to inspect—not automatically as current or trusted releases. Both use legacy schema v1 at the time of writing, so check for a newer Docker Hub or upstream version and validate compatibility with the installed sbx before recommending them.
