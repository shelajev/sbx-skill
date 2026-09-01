# Onboarding and operations

Use this reference for host setup, lifecycle, SSH integrations, workspace safety, and diagnostics.

## Host discovery and lifecycle

1. Confirm the command is running on the host, then detect OS/architecture and whether `sbx` exists.
2. If installed, run `sbx version` and the relevant command help. Do not upgrade a healthy installation unless the requested capability requires it or the user asks.
3. If absent, use the current [installation guide](https://docs.docker.com/ai/sandboxes/install/) rather than cached platform requirements. Explain that installation and `sbx login` mutate/authenticate the host.
4. For a first session, inspect `sbx run --help`, explain direct mount versus clone mode, and use only an agent/runtime listed by installed help.

For durable sandboxes, derive create, attach, execute, stop, and remove syntax from installed help. Do not assume `run` syntax or available agents match another channel.

## SSH-capable apps

Read the current [integration guide](https://docs.docker.com/ai/sandboxes/integrations/) and `sbx setup ssh --help`. Explain that setup edits host SSH configuration and requires an existing named sandbox. Connect using ordinary SSH to `<sandbox>.sbx`; select the mounted workspace by its absolute path when the app does not open it automatically. Prefer SSH port forwarding for private development services when appropriate.

## Workspace safety

Direct mode gives the sandbox read/write access to the mounted host workspace. Clone mode provides stronger separation when available, but uncommitted host changes do not automatically appear in the clone. After untrusted work, review executable project files—including hooks, build/CI scripts, IDE tasks, agent settings, and environment files—before running them on the host.

## Diagnostic ladder

Stop once the issue is explained:

1. `sbx version`, exact failure output, and relevant `--help`.
2. Targeted state such as `sbx ls` or policy inspection.
3. `sbx diagnose` when broader host/daemon evidence is needed.
4. Explain before restarting the daemon because it interrupts work.
5. Reset is destructive and last-resort; diagnostics upload sends data externally. Both require explicit user intent.

Use the current [troubleshooting guide](https://docs.docker.com/ai/sandboxes/troubleshooting/) for platform-specific remedies.
