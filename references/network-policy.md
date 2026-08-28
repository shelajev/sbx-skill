# Network policy

Use this reference for network configuration, blocked requests, authenticated egress, proxies, and policy review.

## Mental model

Sandbox outbound TCP traffic is mediated by host-side proxies and evaluated against policy. Direct external UDP and ICMP remain blocked. HTTP proxy environment variables are managed by sbx; do not overwrite `HTTP_PROXY`, `HTTPS_PROXY`, `NO_PROXY`, or lowercase equivalents in kits.

Rules may come from local policy, a kit, or organization governance. Deny wins over allow. With organization governance active, only organization allow rules grant access; local and kit allow rules are inactive, while their deny rules still restrict.

## Safe workflow

Inspect the installed syntax first:

```sh
sbx policy --help
sbx policy ls --wide
sbx policy ls --include-inactive
```

For new unmanaged hosts, Balanced is the usual preset:

```sh
sbx policy init balanced
```

Do not change an existing preset merely because another would also work. Test a target before adding a rule:

```sh
sbx policy check network api.example.com
sbx policy check network --sandbox <name> api.example.com:443
```

Add the narrowest rule at the narrowest useful scope:

```sh
sbx policy allow network --sandbox <name> api.example.com:443
sbx policy deny network --sandbox <name> telemetry.example.com
```

Then re-run `check` and the failing operation. Use `sbx policy log` to discover redirects, CDN endpoints, package registries, or transparent-proxy traffic; do not guess a large allowlist. Quote wildcard resources so the host shell does not expand them. Understand wildcard semantics from the current [network policy docs](https://docs.docker.com/ai/sandboxes/governance/access-controls/network/) before choosing `*.` versus `**.`.

Global rules affect current and future sandboxes. Prefer `--sandbox` for a one-project need and kit policy for a capability that should travel with a kit. Use current CLI help for removal and inspection syntax.

## Kit policy and credentials

In schema v2 kits, network policy belongs under:

```yaml
permissions:
  network:
    allow:
      - api.example.com
    deny:
      - telemetry.example.com
```

An authenticated destination needs both network permission and a credential declaration/binding. Prefer proxy-managed credential injection so the real secret stays on the host. Never encode a token in `environment.variables`, static files, install commands, or URLs.

## Troubleshooting signals

- Rule appears inactive: inspect the governance status and `--include-inactive`; an organization admin may need to allow the target.
- HTTPS works from the sandbox shell but not a nested container: inspect the proxy path and whether proxy variables reached the container.
- Package install fails: inspect logs for every contacted host, including redirects and artifact/CDN hosts.
- SSH fails: TCP port 22 needs an explicit matching rule; UDP cannot be enabled.
- Corporate network fails: configure the upstream proxy on the host according to the current [upstream proxy docs](https://docs.docker.com/ai/sandboxes/configuration/upstream-proxy/), not by replacing sandbox proxy variables.

Record why each added domain is needed. If the observed host is dynamic or overly broad, ask whether the convenience/security tradeoff is acceptable.
