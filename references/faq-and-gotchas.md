# Operational gotchas

Do not cache ordinary FAQ answers here. Read the current [FAQ](https://docs.docker.com/ai/sandboxes/faq/) or relevant feature page and reconcile it with the installed channel.

## Credentials are not one lifecycle

- Service secrets are global by default; sandbox-scoped service secrets synchronize immediately to an existing sandbox.
- Global service-secret refresh behavior is version-dependent. Stable v0.39 documentation says a global secret applies at creation and requires recreation after a change. v0.42 RC/current main resupplies global service secrets when a sandbox starts or reattaches. Check the installed version and current [credentials documentation](https://docs.docker.com/ai/sandboxes/configuration/credentials/) before prescribing restart versus recreation.
- Registry credentials have separate scopes. `--all-sandboxes` registry credentials are inputs for new sandboxes and are not picked up by existing ones; use a sandbox-scoped registry credential for an existing sandbox.

Never collapse these cases into “global credentials are creation-time.” After a credential change, verify access without printing the value.

## Other gotchas

- Kit application and removal semantics vary by version. Inspect `sbx kit --help` before changing an existing sandbox; identify state that recreation would lose.
- Stopping retains sandbox state; removal/reset does not. Account for work, agent history, images, volumes, and interactive setup before recreation.
- A saved template captures its filesystem. Remove manually stored tokens before saving and prefer host-side proxy-managed credentials.
- Install user-level tools such as `rustup`, `nvm`, or `pyenv` as the sandbox's non-root agent user, not under `/root`.
- SSH sessions do not imply host environment forwarding. After `sbx setup ssh`, connect with ordinary `ssh <sandbox>.sbx` or an SSH-capable application.
