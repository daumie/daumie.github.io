---
title: "Useful Docker Commands"
date: 2019-11-08T17:44:28+03:00
draft: true
toc: false
images:
tags: 
  - untagged
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

