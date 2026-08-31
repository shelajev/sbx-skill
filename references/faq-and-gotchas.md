# FAQ and operational gotchas

Read the live [FAQ](https://docs.docker.com/ai/sandboxes/faq/) before answering FAQ or operational questions. Use this page as a checklist, not as a substitute for the current docs.

For “is X supported?”, known-fix, regression, or roadmap-adjacent questions, search [`docker/sbx-releases`](https://github.com/docker/sbx-releases/releases) as well as the stable docs. Check the installed `sbx version`, the latest stable release, and relevant RC/nightly notes. A prerelease can show that Docker has implemented or is testing a capability, but it does not prove stable availability or a delivery date. State the exact version/channel, distinguish documented fact from inference, and offer prerelease installation only when the user accepts the stability tradeoff.

## FAQ answer checklist

- The sbx CLI is free, including commercial use. Centrally managed organization governance is the paid component.
- Sign-in gives sandboxes a verified user identity, enables team/governance features, and authenticates Docker infrastructure access. Docker says the account email is used for authentication, not marketing.
- Organization governance can centrally enforce network, filesystem, and MCP policy. With it active, only organization allows grant access; local and kit denies can restrict further.
- Corporate firewalls may need Docker authentication, Hub/API, registry, image, telemetry, and diagnostics hosts. Copy the current domain list from the FAQ rather than preserving a list here.
- CLI telemetry records command usage, outcome, duration, and signed-in username; it does not read prompts, sessions, or code. `SBX_NO_TELEMETRY=1` opts out according to current docs.
- Environment-variable flags require a sufficiently recent CLI. Persistent environment changes affect only subsequently started shells/agents; restart the agent or sandbox as the current docs direct.
- Agents commonly start without approval prompts because the microVM, network policy, credential isolation, and shared-path boundary are the safety controls. Users can change agent permission mode or define a kit with a different entrypoint.
- To verify isolation, ask the agent whether it is running in a sandbox; use an agent-specific non-interrupting command when available.
- Host user-level agent configuration is not generally imported. Project configuration remains visible through the workspace. Shared supported skills can be imported with the current `sbx skills import` workflow. Host-path symlinks outside the workspace do not bridge the boundary.
- Host image-paste access is opt-in and weakens isolation. Use the current `sbx settings` syntax and explain what host clipboard data becomes readable.
- On headless Linux or some WSL setups without Secret Service, sbx can store secrets in a permissions-protected file rather than a keychain. Treat that directory as sensitive and prefer an available keyring.

## Lifecycle gotchas

- Global service secrets and all-sandboxes registry credentials are creation-time inputs. Existing sandboxes do not receive values added or changed later. For an existing sandbox, set a sandbox-scoped secret/credential if supported, or recreate it. A sandbox-scoped secret takes effect immediately according to current docs.
- `--kit` is also creation-time. A supported mixin can be applied with `sbx kit add`, but this restarts the sandbox and supports only a restricted set of kit fields. Other changes require recreation, and kits cannot currently be removed in place.
- `-e`, `--env-file`, persistent shell configuration, kit environment, SSH sessions, and proxy-injected credentials are different mechanisms. Do not use ordinary environment variables for secrets, and do not assume an SSH client forwards host environment variables.
- A stopped sandbox retains state; removal/reset does not. Before recreation, identify work, agent history, Docker images, volumes, or interactive setup that would be lost or need reproducing.
- Saving a sandbox as a template captures its filesystem. Manually stored tokens can therefore be embedded in the template; inspect and remove them, and prefer proxy-managed secrets.
- User-level tool installers such as `rustup`, `nvm`, and `pyenv` must install as the sandbox's non-root agent user. Root-installed user tools often end up under `/root` and are unavailable to the agent.
- Direct-mode workspaces preserve the host's absolute path inside the sandbox. This matters when selecting a remote folder in an editor.

## Productive workflows

For interactive editors and desktop apps, prefer SSH integration when the app supports remote SSH:

1. Check current integration docs and `sbx setup ssh --help`, then run `sbx setup ssh` on the host with permission because it edits SSH configuration.
2. Create or identify a named sandbox and connect to `<name>.sbx` (or use `sbx ssh` where current help calls for it).
3. Select the mounted workspace's absolute path in the remote app. Do not assume the app opens it automatically.
4. Prefer SSH local port forwarding for a private development service when appropriate; publish a port only when the workflow requires it.

The TUI is useful while inspecting resource usage or discovering denied network calls. Once that observation is finished, suggest closing it if the user does not need it; the UI can itself consume terminal/host resources. Present this as a practical troubleshooting step, not a documented guarantee of performance improvement.

When setup becomes repetitive, propose the smallest reusable layer:

- checked-in `.sbxenv.yaml` for a repeatable project launch;
- a mixin kit for a portable tool, policy, credential declaration, or runtime setup;
- a sandbox kit for a new agent runtime;
- a template for large, stable packages/toolchains or an expensive prebuilt filesystem.

Offer to author `.sbxenv.yaml` after inspecting the repository. Keep secrets as references, support a local uncommitted override where useful, validate with the installed CLI, and ensure a fresh contributor can start with one documented command.
