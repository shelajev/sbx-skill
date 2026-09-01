# Cloud operations

Use this route only when the user requests cloud Sandboxes and installed help exposes `--cloud` or equivalent current syntax. Cloud is not part of stable v0.39; consult the installed channel's help and matching [`sbx-releases`](https://github.com/docker/sbx-releases/releases) notes.

Do not apply local-daemon assumptions to cloud operations. Before acting, inspect help for the exact operation and establish:

- whether the sandbox is local or cloud and which account/project owns it;
- create/run versus attach/reuse behavior;
- move semantics between local and cloud;
- TTL and deletion consequences;
- persistent volume/workspace behavior;
- cloud SSH/connectivity and public port exposure;
- network, credentials, kit, and template semantics for that channel.

Cloud operations can create billable or remotely persistent resources and may publish services. Explain scope, lifetime, exposure, and teardown before creation, move, or publication. Verify resulting location and resource state with the channel's read-only listing/inspection commands.
