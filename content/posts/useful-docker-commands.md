---
title: "Useful Docker Commands"
date: 2019-11-08T17:44:28+03:00
draft: false
toc: false
images:
tags: 
  - docker
---

### Docker commands you should know

#### Automatically start containers - restart policies

- Restart policies to control whether your containers start automatically when they exit, or when Docker restart

- Restart policies ensure that linked containers are started in the correct order.

- Docker recommends that you use restart policies, and avoid using process managers to start containers.

To configure the restart policy for a container, use the --restart flag when using the docker run command. The value of the `--restart`
flag can be:

- `--restart no` : (default) Do not automatically restart the container.

- `--restart on-failure` :  Restart the container if it exits due to an error, which manifests as a non-zero exit code.

- `--restart always` : Always restart the container if it stops. If it is manually stopped, it is restarted only when Docker daemon restarts or the container itself is manually restarted.

- `--restart unless-stopped` : Similar to `always`, except that when the container is stopped (manually or otherwise), it is not restarted even after Docker daemon restarts.

#### `CMD` directive ( Dockerfile)

It sets the default command for the image that runs if no other command is specified.

#### `EXPOSE` directive

The `EXPOSE` directive documents the ports that should be published when running a container from the image.

#### `ENV` directive

It sets environment variables that are made available in subsequent build steps and to containers at the runtime.

#### `ADD` versus `COPY` directive

- The ADD directive can pull a file from an external URL.
- The ADD directive can extract an archive into the image.

#### Flattening an existing multi-layered image into a single layer

We can run a container from the image, export it, and then import it as a new image.

- `docker service ps nginx` list all of the replicas that are part of a service `nginx`

#### view detailed metadata about a container

```bash
docker container inspect
```

find metadata about any container.

```bash
docker inspect
```

query metadata about any Docker object.

#### Command to view logs for all of the tasks in a service called `my-service`

```bash
docker service logs my-service
```

This command will retrieve logs for all of the tasks in the service.

This command will evenly spread out tasks based upon the values of the availability_zone label.

```bash
docker service create --placement-pref spread=node.labels.availability_zone nginx
```

What flag should we use to specify a custom volume driver when creating a volume alongside a service that has `docker service create` ?

> --mount volume-driver=<driver>

Which storage driver is the default for current versions of CentOS/RHEL and Ubuntu?

> overlay2

Which of the following commands may result in the creation of a new named volume?

```bash
docker run -v my-vol:/tmp nginx
```

This command will create a new volume called `my-vol` if one does not already exist under that name.

Which of the following commands will allow us to mount the `/my/path` directory on the host to a container?

```bash
docker run -v /my/path:/tmp nginx
```

```bash
docker run --mount source=/my/path,destination=/tmp nginx
```

Which of the following statements about the `overlay` network driver is accurate?

> The `overlay` network driver dynamically creates networking components on the node when a relevant task gets scheduled on that node.

Which of the following tasks can we perform to set a custom DNS server for a container?

> We can use the `--dns` flag with `docker run`.

Which of the following commands will create a new bridge network?

> `docker network create my-network` Since the bridge is the default, a new bridge network will generate even when `--driver` is not specified.

Which of the following statements about Docker Content Trust (DCT) is accurate?

> When Docker Content Trust is enabled, unsigned images will not be allowed to run on the system.

Which command allows us to create an encrypted overlay network?

```bash
docker network create --opt encrypted --driver overlay my-net
```

Which of the following is a secure method for allowing a Docker client to authenticate with a registry that uses a self-signed certificate?

> We add the self-signed certificate as a trusted registry certificate under `/etc/docker/certs.d/`

N/B
> The **Docker daemon** must run as root, so it is essential to ensure that it's being protected and has limited access to it.

We can set `dns` in `/etc/docker/daemon.json` ; valid method that we can use for setting the default DNS server for all containers on a host

When creating a container, how would we specify that the container should be attached to an existing network called `my-network`?

> We can use `docker run --network my-network nginx`

When creating an `overlay` network, what flag can we use to allow containers to attach to the network after it is created?

> `--attachable`

View container logs

> docker logs <container>

Which component of the Docker Container Networking Model (CNM) is responsible for allocating IP addresses within Docker networks?

> The *IP Address Management* (IPAM) Driver is responsible

The `none` driver
> The *none* driver implements sandboxes.

Which of the following commands will publish a service's port, but only on nodes that are running a task for that service?

```bash
docker service create -p mode=host,published=8082,target=80 nginx
```

How would we create a volume called `new-volume` without running a container?

> We would run `docker volume create new-volume`

Which of the following commands would we use to locate the data for a volume on the host?

> docker volume inspect <volume>

Which storage driver is the default for CentOS 7 and earlier?

> `devicemapper`

This command will return the container metadata, including the location of its data on the host.

```bash
docker container inspect <container>
```

What volume driver allows you to create and access external storage that can be shared across a Docker Swarm cluster using SSH?

> `vieux/sshfs` This is a custom driver that uses SSH to access remote storage from any node in the cluster.

What command would we use to list the services that are part of a stack called `web-store`?

```bash
docker stack services web-store
```

Which of the following commands will allow us to add a label to a Docker Swarm node?

```bash
docker node update --label-add <label> <node name>
```

Which of the following commands would successfully change the number of replicas to 5 in a service called my-service?

```bash
docker service update --replicas 5 my-service
```

```bash
docker service scale my-service=5
```

Daniel has some nodes with labels that specify the availability zone of each node. He wants to run a service that can run tasks on any node and that do not have the label availability_zone=east. Which command should he use?

```docker service create --constraint node.labels.availability_zone!=east nginx```

> `docker swarm unlock-key` command

Which of the following best describes the procedure for backing up the Universal Control Plane (UCP) metadata?

> Run a container from the `ucp` image with the `backup` command

What Linux feature does Docker use in order to limit memory usage for containers?

> Control groups (`cgroups`)

How would we back up Docker Swarm?

> Back up the contents of `/var/lib/docker/swarm` on a Swarm manager.

How would we create a new swarm cluster?

> Run `docker swarm init`

What does the **HEALTHCHECK** directive do?

> It sets a command that will be used by the Docker daemon to determine whether the container is healthy.

