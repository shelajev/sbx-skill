# Project configuration

Use this reference for choosing among project environment files, kits, and templates.

## Choose the right layer

- `.sbxenv.yaml`: checked-in declaration of how a project launches—agent, workspace, kits, environment values, secret provisioning references, registries, ports, and resources. Best for one-command contributor onboarding.
- Mixin kit: portable runtime capability or team policy layer applied to agents—tools, files, instructions, network rules, and credential declarations.
- Sandbox kit: a new agent definition and runtime.
- Template: reusable image with heavy system packages, language toolchains, or large dependencies baked in for faster creation.

Keep heavy stable dependencies in a template and thin, variable configuration in a kit/environment file. Do not introduce all three layers for a simple project.

## Environment files

This feature is experimental. Inspect `sbx env --help` and the current [environment file docs](https://docs.docker.com/ai/sandboxes/configuration/environment-files/) before authoring because the schema is version-sensitive.

Workflow:

1. Discover existing `.sbxenv*.yaml`, kits, environment examples, and secret conventions.
2. Keep the shared file portable. Put machine-specific paths or optional local overrides in supported host-variable references or an uncommitted override file.
3. Never commit secret values. Be especially careful with secret or registry values sourced by `command`: those commands execute on the host as the user before the sandbox exists. Review them as trusted host code.
4. Validate through the installed CLI before launching.
5. Prefer `sbx env run` for normal attach/create behavior, `env exec` for automation, and `env rm` for resources owned by that environment, subject to current CLI semantics.
6. Document only prerequisites the CLI cannot discover, such as a required host secret name or organization access.

## Templates

Use a Dockerfile extending the correct official sandbox template variant. The runtime agent passed to sbx must match that variant. Fully qualify registry image names when sbx requires it. Keep the resulting image non-secret and ensure the `agent` user/runtime requirements from the current template documentation remain intact.

Before loading, pushing, or using a template, explain registry and network implications. Template cache and reset behavior may vary; inspect current `sbx template --help` and [template docs](https://docs.docker.com/ai/sandboxes/customize/templates/).

## Completion check

A good project setup lets a new contributor, after installing and signing in to sbx, launch predictably with one documented command. Confirm:

- the selected agent starts;
- workspace mode is intentional;
- required tools are present without repeated slow setup;
- network policy permits only known dependencies;
- credentials stay host-side;
- ports/resources are explicit where needed;
- teardown affects only the declared environment.
