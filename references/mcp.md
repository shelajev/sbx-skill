# MCP gateway

Use this reference for `sbx mcp`: server registration, static or dynamic loading, OAuth, local stdio servers, gateway tools, SSRF-sensitive URLs, and lifecycle. Inspect `sbx mcp --help`, the relevant subcommand help, and the current [MCP gateway documentation](https://docs.docker.com/ai/sandboxes/mcp-gateway/) before mutation.

Docker Sandboxes' MCP gateway is separate from direct agent MCP configuration and from Docker Desktop's MCP Toolkit. Registrations and OAuth state are managed on the host; supported agents inside sandboxes see the gateway endpoint.

## Registration and execution boundary

- `sbx mcp add` creates a reusable host registration; it does not by itself attach the server to a sandbox.
- A remote endpoint executes remotely. A local metadata/registry entry resolving to an OCI stdio package and an explicit `--command` both execute on the host, outside sandbox isolation.
- Treat every local command, package runner, manifest, image, working directory, host mount, and passed credential as host-trusted code. Prefer a pinned and reviewed artifact over a floating package invocation.
- Resolve the owner and final identity of registration URLs and redirects. Treat private, loopback, and cloud-metadata targets as an SSRF/host-network boundary. Some versions reject these targets and others register them with a warning; permission to continue is not evidence that the target is safe.

Inspect an existing registration before changing or removing it. Registration is host-global and may be used by several sandboxes or environment files.

## Choose loading deliberately

- Static mode preloads named registrations at sandbox creation and withholds dynamic discovery/attachment tools from the agent. Prefer it when the required servers are known.
- Dynamic mode exposes gateway tools that let the agent discover and attach registered servers. Use it only when runtime discovery is intended, and account for organization policy on built-in gateway tools.
- `sbx mcp load` attaches an already registered server to a running sandbox and can persist across restarts. Confirm sandbox and server names before loading.

Registration, attachment, authorization, and use are separate states. Verify each with read-only listing/inspection/status commands supported by the installed version.

## OAuth and governance

OAuth credentials remain in the host credential store. Review the server identity, requested scopes, callback/auth metadata, and client registration before authorizing. Store confidential client secrets through the supported secret facility rather than flags or registration files. Removing an OAuth grant, a server registration, or its client identity/secret are separate operations.

MCP organization policy uses Cedar and has separate registration-time and use-time decisions. It can govern registered-server tools, resources, prompts, and built-in gateway tools, and can require per-request confirmation. A direct MCP connection configured inside an agent is outside gateway policy and must also be constrained with network policy when relevant. Read [policy-and-governance.md](policy-and-governance.md) before changing MCP policy.

Use the [compatibility snapshot](compatibility.md) when explaining whether a particular installed channel includes this subsystem or a flag, then confirm against installed help.
