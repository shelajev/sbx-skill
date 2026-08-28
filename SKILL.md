---
name: sbx
description: Set up, configure, operate, troubleshoot, and extend Docker Sandboxes with the sbx CLI. Use for host-side installation and onboarding, sandbox lifecycle, network policy, credentials, project environments, templates, or authoring and debugging kits; do not use for ordinary Docker containers that do not involve Docker Sandboxes.
---

# Docker Sandboxes (`sbx`)

Help the user reach a working, least-surprising sandbox from the host machine. Docker Sandboxes and kits are Early Access/experimental and change quickly: inspect the installed CLI's `--help` before emitting commands, and consult the current [Docker Sandboxes docs](https://docs.docker.com/ai/sandboxes/) when the installed version or requested feature may differ from these references.

## Operating rules

- Run `sbx version` and inspect relevant `sbx <command> --help` output first when `sbx` is available. Never invent a flag from memory. If the CLI is absent, detect the host OS and architecture before proposing installation.
- Explain commands before running ones that sign in, create or remove sandboxes, change global policy, store credentials, publish ports, upload diagnostics, or reset state. Get authorization where the surrounding agent policy requires it.
- Prefer read-only discovery (`sbx version`, `sbx ls`, `sbx policy ls`, `sbx policy check`, `sbx diagnose`) before mutations. Do not run `sbx reset`, `sbx policy reset`, `sbx logout`, bulk removal, or diagnostic upload unless the user explicitly requests the consequence.
- Never print, request in chat, commit, or place real secret values in kit YAML, environment variables inside a sandbox, command history, or generated files. Use `sbx secret`, credential bindings, or host environment references as supported by the installed version.
- Treat the mounted workspace as writable host data. For untrusted repositories or risky agent work, recommend clone mode when supported and remind the user to review executable project files before running them on the host.
- Start network access narrowly. Observe blocked destinations, allow only required hosts/ports, then verify. Do not solve ordinary connectivity failures with `**` unless the user knowingly chooses unrestricted egress.
- Preserve organization governance. Local and kit allows cannot broaden organization allow rules; local and kit denies can restrict further.

## Route the task

- For install, first run, agent selection, lifecycle, host/workspace safety, or basic diagnostics, read [onboarding-and-operations.md](references/onboarding-and-operations.md).
- Before installing or onboarding any agent, CLI, MCP server, or development tool, read [tool-onboarding.md](references/tool-onboarding.md). Search for an existing kit before designing installation steps or a new kit.
- For allow/deny rules, policy precedence, monitoring, proxies, ports, or connectivity failures, read [network-policy.md](references/network-policy.md).
- For any kit work, load Docker's maintained [`kit-author` skill](https://github.com/docker/sbx-kits-contrib/blob/main/skills/kit-author/SKILL.md) and follow the references it routes to. Do not rely on duplicated local kit-schema guidance.
- For reproducible `.sbxenv.yaml` project setup, secrets, templates, and deciding between environments, kits, and templates, read [project-configuration.md](references/project-configuration.md).

Read only the references relevant to the request. A task may require more than one—for example, a kit that calls an authenticated API needs both kit authoring and network policy.

## Working style

Discover the repository and existing sbx configuration before proposing new files. Preserve existing conventions and unrelated changes. State whether each command runs on the host or inside the sandbox. When changing configuration, show the effective scope (global, one sandbox, one project, or one kit) and validate the observable result.

For kit work, defer authoring, validation, testing, and distribution details to Docker's maintained `kit-author` skill. Still inspect the locally installed CLI because it may not match the upstream repository revision.
