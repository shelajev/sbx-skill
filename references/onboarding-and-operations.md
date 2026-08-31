# Onboarding and operations

Use this reference for host setup, first run, sandbox lifecycle, workspace choices, and troubleshooting.

## Discovery before setup

1. Detect OS, version, CPU architecture, and whether `sbx` exists.
2. If installed, run `sbx version`, `sbx diagnose`, and the relevant command help. Do not reinstall a healthy CLI merely to get a newer version unless requested.
3. Check prerequisites against the current [installation guide](https://docs.docker.com/ai/sandboxes/install/). At the time this reference was written, supported hosts were Apple-silicon macOS 14+, Windows 11 with Windows Hypervisor Platform, and Ubuntu 24.04+ with KVM on x86-64 or Arm64. Verify because support changes.
4. Installation and `sbx login` mutate the host or authenticate externally. Explain the action and follow the active approval policy.

Docker Engine/Desktop is not inherently required just to use sbx. Do not make it a prerequisite unless the selected workflow requires regular Docker commands on the host.

## First useful session

Use the simplest supported built-in agent and current directory after checking `sbx run --help`:

```sh
sbx run <agent>
```

Supported agent names evolve; derive them from current docs/help rather than guessing. Before launching:

- initialize the network preset if needed; Balanced is the normal starting point;
- explain direct mount versus clone mode;
- confirm whether the agent needs host credentials or model configuration;
- avoid silently widening network policy.

For a durable named environment, inspect `sbx create --help`, create it, then show how to attach, execute, stop, and remove it. Do not assume `run` syntax is identical across versions.

## Terminal dashboard

Run `sbx` with no arguments, or `sbx tui`, to open the interactive dashboard. It shows sandbox status plus live CPU and memory use. From the sandbox panel, users can create, start/stop, attach, open a shell, and remove sandboxes. The network governance panel shows outbound connection logs and supports reviewing, allowing, or blocking hosts. Use `Tab` to switch panels and `?` for the version's current shortcuts.

Prefer the dashboard when interactively onboarding a tool: keep the network panel visible, exercise one tool operation at a time, and review each attempted destination before allowing it. Prefer CLI policy commands when the process must be reproducible or auditable.

Do not leave the dashboard running by habit. After monitoring or onboarding is complete, suggest closing it when resource pressure or terminal overhead matters; this is a practical mitigation, not a claim that the dashboard is always the cause.

## Editors and other apps

When the user wants VS Code, Cursor, Claude Desktop, ChatGPT, T3 Code, or another SSH-capable app, read the current [integration guide](https://docs.docker.com/ai/sandboxes/integrations/). Prefer `sbx setup ssh` and a named `<sandbox>.sbx` target over installing the whole app inside the sandbox. Explain that setup edits the host SSH configuration, the sandbox must already exist, stopped sandboxes may start on connection, the workspace may need to be selected manually by its absolute path, and SSH does not forward host environment variables.

## Workspace safety

Direct mount gives the sandboxed agent read/write access to the host workspace. The VM isolates the agent from the rest of the host, not from mounted files. After an agent touches an untrusted project, review Git hooks, build scripts, CI workflows, IDE tasks, agent settings, and `.sbxenv.yaml` before executing them on the host.

Clone mode provides stronger separation from the host checkout when available. Explain how work is brought back before choosing it for the user; do not imply that uncommitted host changes automatically appear in the clone.

## Diagnostic ladder

Stop once the issue is explained or fixed:

1. `sbx version` and relevant `--help` output.
2. `sbx diagnose` (use JSON only if machine parsing helps).
3. `sbx ls` and targeted state inspection.
4. For connectivity, use `sbx policy check network`, `sbx policy log`, and the network reference.
5. For daemon reachability, `sbx daemon restart` is non-destructive to sandbox data but interrupts operation; explain before running it.
6. `sbx reset` deletes sandbox data and cached state. It is a last resort and requires explicit user intent.
7. `sbx diagnose --upload` sends a support bundle externally. Never upload without explicit authorization.

Use the current [troubleshooting guide](https://docs.docker.com/ai/sandboxes/troubleshooting/) for platform-specific remedies.
