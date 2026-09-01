# Policy and governance

Use this reference for network and filesystem access, organization governance, MCP access policy, proxies, ports, and connectivity. Read the relevant current [access-control documentation](https://docs.docker.com/ai/sandboxes/governance/access-controls/) and inspect installed policy help before changing rules.

## Do not collapse the policy models

| Surface | Rule model and precedence | Evaluation time |
| --- | --- | --- |
| Network | Local allow/deny rules exist. Under organization governance, only organization allows grant access; local and kit allows cannot broaden them, while local/kit denies can restrict further. | Every outbound request. |
| Filesystem | Organization rules allow host paths for read or write. Local filesystem deny rules do not exist; `sbx policy deny` is network-only. | When a workspace is mounted at sandbox creation. A later policy change does not revoke an existing mount. |
| MCP gateway | Organization policy is Cedar. When MCP enforcement applies, registration and governed requests are default-deny without a matching `permit`; `forbid` overrides `permit`. Approval annotations add a use-time confirmation gate. | Registration policy at `sbx mcp add`; use-time policy on governed gateway requests. Registration policy changes do not remove saved or loaded servers. |

MCP policy governs only Docker's gateway. A server configured directly in an agent bypasses gateway policy, although its remote connection is still subject to network policy. Govern both paths when the requirement covers both. See [mcp.md](mcp.md) for gateway operations.

## Narrow network workflow

1. Preserve the exact failed request and inspect current rules, governance status, and policy logs using installed syntax.
2. Check the specific hostname and port. Account for redirects, API/resource hosts, authentication endpoints, registries, and CDNs without guessing a broad allowlist.
3. Identify the destination's owner and purpose. Ask when trust or required scope is unclear.
4. Add the narrowest rule at the narrowest useful scope. Quote wildcard resources and confirm wildcard semantics before using them.
5. Retry the original operation and re-inspect policy evidence. Do not escalate to unrestricted egress because the first rule was incomplete.

Host-managed proxies enforce egress and can inject supported credentials. Do not overwrite sbx-managed proxy variables or place tokens in environment files, kits, URLs, or sandbox files. Authenticated traffic requires network permission plus credential availability and injection authorization; only third-party schema-v2 kit credentials necessarily require explicit bindings. See [credentials.md](credentials.md).

## Filesystem and MCP safeguards

- For filesystem denials, inspect the resolved host path, read versus write need, wildcard semantics, effective organization policies, and team scope. Recreate the sandbox only after explaining that existing state and mounts will be replaced.
- For MCP, distinguish registration from use-time access and distinguish registered servers from built-in gateway tools. Use `forbid` for operations that must never run; `@requireApproval` is an in-protocol human confirmation guard, not administrator approval or separation of duties.
- Treat tool annotations such as `readOnly` as server-supplied advisory metadata. Unannotated tools are not proof of safety.
- Local stdio MCP servers are host-executed. An organization can forbid the `local-stdio` registration type; network sandboxing alone does not isolate those host processes.

Policy resets delete local rules and interrupt the daemon. Organization policy changes may take time to propagate. Explain scope and impact and require explicit intent before reset. Record why each added destination, path, server, or permission is required so the resulting policy remains reviewable.
