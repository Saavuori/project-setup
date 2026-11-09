# Self-hosted app platform on Oracle Cloud

A single Oracle Cloud virtual machine that hosts several independent web apps behind one TLS-terminating proxy. This glossary fixes the words the [setup guide](docs/guide.md) uses, so a reader building the same system can follow it without guessing.

## Language

### Where things run

**Host**:
The one Oracle Cloud virtual machine that everything runs on.
_Avoid_: Box, server, instance, VM (when used as a name)

**Proxy Network**:
The single shared container network that connects the Edge Proxy to the public-facing container of every App.
_Avoid_: Web network, bridge, frontend network

### What runs there

**App**:
One independently deployable web application on the Host, packaged as its own container stack with its own directory and its own public hostname.
_Avoid_: Site, service, project, stack (when meaning the whole app)

**Edge Proxy**:
The single shared reverse proxy that owns public ports 80/443, terminates TLS and routes each public hostname to the right App. It is shared infrastructure, not part of any App.
_Avoid_: Caddy (that's the product, not the role), front proxy, load balancer

**App Data Store**:
A database or cache that belongs to exactly one App and lives inside that App's stack; it becomes a Shared Service only when a second App needs the same data.
_Avoid_: Sidecar DB, local DB

**Shared Service**:
A stateful component, such as a database, that lives outside any App and is reachable only on the Host's internal networks, never published to the internet.
_Avoid_: Backend, infra service, DB stack

### How changes arrive

**Updater**:
The scheduled job on the Host that notices a newer image for an App in the registry and redeploys that App. Deployments are pulled by the Host, never pushed to it.
_Avoid_: Deployer, watcher, CD agent
