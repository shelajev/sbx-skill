# Network policy

Use this reference for blocked requests, policy changes, governance, proxies, ports, and authenticated egress. Inspect current network-policy docs and `sbx policy --help` before changing rules.

## Invariants

- Deny wins over allow. With organization governance active, only organization allows grant access; local and kit denies can restrict further but their allows cannot broaden access.
- Host-managed proxies enforce sandbox egress and inject supported credentials. Do not overwrite sbx-managed proxy environment variables or put tokens in environment files, kits, URLs, or sandbox files.
- Prefer sandbox scope for one-project access and kit policy only when access belongs to every user of that capability.

## Narrow workflow

1. Preserve the exact failed request and inspect current rules, governance status, and policy logs using installed syntax.
2. Check the specific hostname and port before mutation. Account for redirects, API/resource hosts, authentication endpoints, registries, and CDNs without guessing a broad allowlist.
3. Identify the destination's owner and purpose. Ask when trust or required scope is unclear.
4. Add the narrowest rule at the narrowest useful scope. Quote wildcard resources and confirm current wildcard semantics before using them.
5. Repeat the original operation and re-inspect policy evidence. Do not escalate to unrestricted egress merely because the first rule was incomplete.

Authenticated traffic needs both network permission and a credential binding. Keep the value in the host-side store and verify access without exposing it.

## Diagnostic distinctions

- If a rule is inactive, inspect organization governance rather than repeatedly adding local allows.
- If a sandbox shell works but a nested container fails, inspect the proxy path and which proxy settings reached the container.
- For corporate networks, configure the upstream proxy on the host using current documentation; do not replace sandbox proxy variables.
- SSH, UDP, ICMP, published ports, and cloud networking have version/channel-specific semantics. Check relevant help and docs rather than generalizing from ordinary HTTPS egress.

Record why each added destination is required so the resulting policy remains reviewable.
