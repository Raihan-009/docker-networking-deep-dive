# docker-networking-deep-dive

## `Networking Options`

Docker provides three different networking options to facilitate communication between containers and the external world.

- `Bridge`: Default, isolated internal network for containers.

- `None`: Complete network isolation for containers.

- `Host`: Containers share the host's network stack.