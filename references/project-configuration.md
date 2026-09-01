# Project configuration

Use this reference for environment files, sandbox skill sharing, templates, and choosing a reusable layer.

## Environment files

Inspect `sbx env --help` and the current [environment-file documentation](https://docs.docker.com/ai/sandboxes/configuration/environment-files/) before choosing a filename or schema.

- v0.39 directory discovery uses `.sbxenv.yaml` (with `.sbxenv.yml` fallback).
- [v0.42.0-rc3](https://github.com/docker/sbx-releases/releases/tag/v0.42.0-rc3) prefers `sbxenv.yaml` when both hidden and non-hidden forms exist. Do not assume one channel's discovery behavior on another; pass an explicit path when supported and appropriate.

An environment file is host-trusted code, even when it comes from a familiar repository. It can declare a dynamic `secrets.command` that the host executes; newer schemas can add other host lifecycle actions. Keep every applied environment file outside the primary and additional writable workspace mounts so a sandboxed process cannot rewrite future host behavior. Only offer a version-controlled file when the repository layout preserves that boundary. Do not create one during an answer-only request.

Discover existing environment files, workspaces, kits, and secret conventions first. For an unfamiliar file, use `sbx env plan` as the primary read-only inspection when that command exists. Otherwise read the complete resolved configuration before applying it. Review every host command or secret resolver, credential/binding, MCP registration, workspace, kit, port, and affected sandbox; do not suppress confirmation merely for convenience. Never commit secret values.

v0.42.0-rc3 binds the environment file read-only into the sandbox it describes. That reduces post-launch modification risk but does not make an untrusted file safe: `sbx` still evaluates its host-side declarations. v0.39 lacks this mitigation, so the outside-writable-mount requirement is essential.

## Sandbox skills

When `sbx skills --help` exists, use it as the source of truth for `add`, `update`, `ls`, `rm`, and `import`. Skill installation happens on the host; sharing makes selected skills visible in a sandbox but does not grant its agent access to the host `sbx` CLI.

The 2026-09-01 nightly adds read-only skill-sharing behavior beyond v0.42.0-rc3. Prefer read-only sharing when installed help supports it; use read-write only when the user intends sandbox changes to propagate to the shared store. Do not offer flags or semantics whose installed help is absent. See the dated [compatibility snapshot](compatibility.md).

## Choose the reusable layer

- Environment file: project-specific launch and resource configuration.
- Mixin or sandbox kit: a portable capability or agent runtime used across projects.
- Template: a large, stable filesystem/toolchain expensive to recreate.

Keep credentials host-side and inspect current template/kit help before mutating registries, sandbox state, or shared artifacts.
