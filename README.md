# Docker Sandboxes (`sbx`) skill

An Agent Skill for setting up, operating, and troubleshooting [Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) with the `sbx` CLI.

It makes your AI agent check the current Docker documentation before acting, then gives it practical guidance for network policy, secrets, project environments, SSH integrations, templates, and kits. It also covers frequently missed behavior from the [Docker Sandboxes FAQ](https://docs.docker.com/ai/sandboxes/faq/).

## Install from GitHub

```sh
npx skills add shelajev/sbx-skill
```

The installer asks which supported agents and scope to use. To install non-interactively for every detected agent:

```sh
npx skills add shelajev/sbx-skill --all
```

## Use it

Ask your agent naturally, for example:

> Set up an sbx sandbox for this project and create a repeatable environment file for my installed sbx version.

Or invoke the skill explicitly as `$sbx` in agents that support explicit skill names. The skill expects the Docker Sandboxes CLI to be installed on the host; follow Docker's current [installation guide](https://docs.docker.com/ai/sandboxes/install/).

Useful Docker resources:

- [Docker Sandboxes documentation](https://docs.docker.com/ai/sandboxes/)
- [FAQ](https://docs.docker.com/ai/sandboxes/faq/)
- [Release notes](https://docs.docker.com/ai/sandboxes/release-notes/)
- [`sbx-releases`: stable, RC, and nightly releases](https://github.com/docker/sbx-releases/releases)

## License

Apache License 2.0. See [LICENSE](LICENSE).
