# Credentials

Use this reference for service and custom secrets, dynamic host sources, credential bindings, registry authentication, OAuth, and SSH-agent forwarding. Inspect the installed `sbx secret --help` and current [credentials documentation](https://docs.docker.com/ai/sandboxes/configuration/credentials/) before changing credential state.

## Separate availability from injection authorization

Authenticated traffic needs network permission, an available credential, and authorization to inject it. These are separate checks; a credential binding is not universally required.

- Built-in credentials are authorized through the provenance of embedded kits.
- Third-party `schemaVersion: "2"` kits require an explicit binding for each credential they use, including credentials inherited from a built-in agent.
- A binding records approved mechanisms and domains, not the secret value. Declining or omitting it withholds injection.
- Kit credential declarations and network policy still constrain where an authorized credential can be sent.

Diagnose a failure as network reachability, credential availability, or injection authorization before changing anything. Verify authentication without printing the credential or resolver output.

## Choose the correct facility

- Service secret: a host-stored value for a built-in or kit-declared service. Prefer interactive entry or standard input over command-line literals.
- Dynamic service or custom secret: a host-side vault reference or command. The daemon resolves it on the host, so review the reference, executable, arguments, refresh behavior, and possible sensitive error output. Never embed the secret in the command text.
- Custom secret: domain/environment-variable-based injection when the service model does not fit. Treat it as experimental when the installed documentation says so, and scope its hosts narrowly.
- Credential binding: an approval record for third-party schema-v2 kit injection. Review requested mechanisms and every domain before approving.
- Registry credential: separate authentication for pulling images, templates, and kits. Prefer sandbox scope over all-sandboxes scope.
- OAuth: a host-side flow when supported. Proxy passthrough options can place real tokens inside a sandbox and weaken isolation; explain that tradeoff before enabling them. MCP OAuth has a separate lifecycle described in [mcp.md](mcp.md).
- SSH agent: forwards signing capability, not private-key bytes. A sandboxed process can still ask the agent to sign, and outbound SSH remains governed by network policy. Use only when the workflow requires it.

## Lifecycle distinctions

- In v0.39, a sandbox-scoped service secret can take effect on an existing sandbox, while a changed global service secret requires recreation. v0.42.0-rc3 resupplies global service secrets when a sandbox starts or reattaches. Confirm the installed version before prescribing restart or recreation.
- All-sandboxes registry credentials are inputs to new sandboxes; existing sandboxes do not acquire them. Use a sandbox-scoped registry credential for an existing sandbox.
- Removing a sandbox, removing an environment, or resetting `sbx` can affect differently scoped secrets, bindings, registrations, and host credential-store entries. Inspect the exact removal help and explain what persists before acting.

Use the dated [compatibility snapshot](compatibility.md) only for known version differences; use current docs and installed help for newer versions.
