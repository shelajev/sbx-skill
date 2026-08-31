# Docker Sandboxes (`sbx`) skill

An Agent Skill for setting up, operating, and troubleshooting [Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) with the `sbx` CLI.

It makes your AI agent check the current Docker documentation before acting, then gives it practical guidance for network policy, secrets, project environments, SSH integrations, templates, and kits. It also covers frequently missed behavior from the [Docker Sandboxes FAQ](https://docs.docker.com/ai/sandboxes/faq/).

## Install from GitHub

```sh
npx skills add shelajev/sbx-skill --all
```

This installs the skill for the supported AI coding agents detected on your machine. Remove `--all` if you want the installer to ask which agents and scope to use.

## Install from Tessl

Once the public registry package is available:

```sh
tessl install olegselajev/sbx --global --yes
```

Omit `--global` to install it only in the current project.

## Use it

Ask your agent naturally, for example:

> Set up an sbx sandbox for this project and create a repeatable `.sbxenv.yaml`.

Or invoke the skill explicitly as `$sbx` in agents that support explicit skill names. The skill expects the Docker Sandboxes CLI to be installed on the host; follow Docker's current [installation guide](https://docs.docker.com/ai/sandboxes/install/).

Useful Docker resources:

- [Docker Sandboxes documentation](https://docs.docker.com/ai/sandboxes/)
- [FAQ](https://docs.docker.com/ai/sandboxes/faq/)
- [Release notes](https://docs.docker.com/ai/sandboxes/release-notes/)
- [`sbx-releases`: stable, RC, and nightly releases](https://github.com/docker/sbx-releases/releases)
- [GitHub repository](https://github.com/docker/sbx)

## License

Apache License 2.0. See [LICENSE](LICENSE).
