# Project configuration

Use this reference for environment files, sandbox skill sharing, templates, and choosing a reusable layer.

## Environment files

Inspect `sbx env --help` and the current [environment-file documentation](https://docs.docker.com/ai/sandboxes/configuration/environment-files/) before choosing a filename or schema.

- Stable v0.39 directory discovery uses `.sbxenv.yaml` (with `.sbxenv.yml` fallback).
- [v0.42 RC/current main](https://github.com/docker/sbx-releases/releases/tag/v0.42.0-rc3) directory discovery uses `sbxenv.yaml`; rc3 prefers the non-hidden file when both forms exist. Do not assume a hidden stable file will be discovered by newer builds. Pass an explicit path when supported and appropriate.

Offer a checked-in environment file when launch setup is repeated or shared, but do not create one during an answer-only request. Discover existing environment files, kits, and secret conventions first.

Environment files may change host state and, in v0.42 RC/main, can run `lifecycle` commands with the host user's privileges. For an unfamiliar file, use `sbx env plan` as the primary read-only inspection before applying it when that command exists. Review every host command, credential/binding, workspace, kit, port, and affected sandbox; do not suppress confirmation merely for convenience. Never commit secret values.

## Sandbox skills (v0.42 RC/main)

When `sbx skills --help` exists, use it as the source of truth for `add`, `update`, `ls`, `rm`, and `import`. Skill installation happens on the host; sharing makes selected skills visible in a sandbox but does not grant its agent access to the host `sbx` CLI.

Newer environment workflows share imported skills read-only by default and expose `--skills=off|readonly|readwrite`. Prefer `readonly`; use `readwrite` only when the user intends sandbox changes to propagate to the shared store. Do not offer this surface on stable versions whose help lacks it.

## Choose the reusable layer

- Environment file: project-specific launch and resource configuration.
- Mixin or sandbox kit: a portable capability or agent runtime used across projects.
- Template: a large, stable filesystem/toolchain expensive to recreate.

Keep credentials host-side and inspect current template/kit help before mutating registries, sandbox state, or shared artifacts.
