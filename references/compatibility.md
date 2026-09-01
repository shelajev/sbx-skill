# Compatibility snapshot — 2026-09-01

Use this dated reference only to explain known differences among the releases available on 2026-09-01. Always run `sbx version`, inspect installed help, and check newer [stable release notes](https://docs.docker.com/ai/sandboxes/release-notes/) and [`sbx-releases`](https://github.com/docker/sbx-releases/releases) before acting.

- v0.39.0 is the stable release dated 2026-08-19. It includes the stable MCP gateway introduced in v0.38 and experimental declarative environments discovered as `.sbxenv.yaml` with `.sbxenv.yml` fallback. Environment files can use host-executed dynamic secret commands and must remain outside writable workspace mounts.
- v0.42.0-rc3 is a prerelease dated 2026-08-31. It prefers `sbxenv.yaml`, adds fuller environment planning and confirmation, binds the applied environment file read-only into the sandbox, and includes prerelease cloud behavior. Do not present these behaviors as v0.39 behavior.
- The 2026-09-01 nightly contains work beyond v0.42.0-rc3, including cloud and read-only skill-sharing surfaces. Nightly syntax and semantics are directional evidence, not a stable contract.

Use explicit labels such as “v0.39”, “v0.42.0-rc3”, and “2026-09-01 nightly”. Do not call any of them “current” in durable instructions.
