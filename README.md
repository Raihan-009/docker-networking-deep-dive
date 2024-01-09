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

![nginx](https://lab-bucket.s3.brilliant.com.bd/labthumbnail/83937f2e-425f-4166-b062-71dcf9ae260c.png)

Here we go! But what's happening?

![nginx-80](https://lab-bucket.s3.brilliant.com.bd/labthumbnail/e5945aeb-8a67-4efc-82bc-ec5e9a9e2357.png)

The `--network host` option in Docker, the services inside the container are directly exposed on the host's network, and there is no separate port mapping. And now we wont be able to run mutliple containers on the same host, on the same port.

---