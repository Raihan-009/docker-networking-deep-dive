# docker-networking-deep-dive

## `Networking Options`

Docker provides three different networking options to facilitate communication between containers and the external world.

- `Bridge`: Default, isolated internal network for containers.

- `None`: Complete network isolation for containers.

- `Host`: Containers share the host's network stack.

## `Host Network`

One way to access the container externally is to associate the container to the host network as it takes out the network isolation between host and container.

Lets run an nginx server using the host network of docker.

```bash
docker run -d --network host --name nginx-container nginx
```

Since Nginx is running on the host's network, we can access it using the host machine's IP address or localhost at the default Nginx port, usually 80. In my case I am using virtual machine, so I will try with public IP address of my VM.

![nginx](https://github.com/Raihan-009/docker-networking-deep-dive/blob/main/diagrams/nginx-container.png?raw=true)

Here we go! But what's happening?

![nginx-80](https://github.com/Raihan-009/docker-networking-deep-dive/blob/main/diagrams/nginx-80.png?raw=true)


The `--network host` option in Docker, the services inside the container are directly exposed on the host's network, and there is no separate port mapping. And now we wont be able to run mutliple containers on the same host, on the same port.


## `None Network`

A container with the `--network none` option in Docker, it means that the container is isolated from the host network, and it doesn't have any network interfaces. Tt also means that the container won't be able to communicate with the external network or other containers.

```bash
docker run -d --network none --name isolated-container nginx
```

The only way to interact with it is by accessing its filesystem or running commands inside the container.

```bash
docker exec -it isolated-container /bin/bash
```

![none-nginx](https://github.com/Raihan-009/docker-networking-deep-dive/blob/main/diagrams/isolated-container.png?raw=true)


## `Bridge Network`

The bridge network is the default networking mode in Docker, serving as a `private internal network` for container communication. Containers on the bridge network are `isolated` from the host machine and each other, with each container assigned its own internal IP address. `Network Address Translation (NAT)` enables containers to access the external network via the host machine. Port mapping allows for exposing container ports to the host or external network. Containers on the same bridge network can communicate using their internal IP addresses.


![docker-bridge](https://github.com/Raihan-009/docker-networking-deep-dive/blob/main/diagrams/docker-bridge.png?raw=true)

To create a custom bridge network inside docker

```bash
docker network create v-net
```

And then, to run containers with the `--network` option to connect them to this custom network

```bash
docker run --network=mynetwork v-net
```
To expose a container's port to the host machine or to the outside world, we need to get help of port forwarding.

## `Port Forwarding`

It exposes a container's port to the host or external network, making services accessible.

```bash
docker run -d --name nginx-container -p 4000:80 nginx
```
- `-d`: Runs the container in detached mode.
- `--name nginx-container`: Assigns a name to the container.
- `-p 4000:80`: Maps port 4000 on the host to port 80 on the container.

![port-forwarding](https://github.com/Raihan-009/docker-networking-deep-dive/blob/main/diagrams/port-forwarding.png?raw=true)