---
title: "How to Docker Machine"
date: 2019-08-22T15:12:53+03:00
draft: false
toc: false
images:
tags: 
  - docker
  - containers
  - aws
---

### What is Docker?

Docker is the command-line tool that uses containerization to manage multiple images and containers and volumes and such -- a container is basically a lightweight virtual machine.

### Where does docker machine come in?

When using a containerized application, it’s important to be able to easily deploy them in the cloud. Docker-Machine is a tool that lets you install Docker Engine on Virtual Hosts.

Since Docker isn't running on your actual host OS, docker-machine needs to deal with IP addresses and ports and volumes and such. And its settings are saved in environment variables, which means you have to run commands like this every time you open a new shell:

```bash
eval $(docker-machine env default)
docker-machine ip default
```

### What are some of the benefits of using docker machine?

For instance, you can easily have a development, staging and production environment accessible from your own machine and update them accordingly.

### How does docker work?

Docker has a client server-architecture, in which the Client sends the command to the Docker Host, which runs the Docker Daemon. Both the Client and the Docker Host can be in the same machine, or the Client can communicate with any of the Docker Hosts running anywhere, as long as it can reach and access the Docker Daemon.

The Docker Client and the Docker Daemon communicate over REST APIs, even on the same system. One tool that can help you manage Docker Daemons running on different systems from our local workstation is Docker Machine.

Docker-machine sets up Docker hosts on any supported system via [provider drivers](https://docs.docker.com/machine/drivers/).

### Deploying Containers to a remote host using docker-machine. ( we'll be using AWS in this example)

[Spin up an EC2 instance](https://searchaws.techtarget.com/tip/Manually-spin-up-an-EC2-server-instance-in-7-steps) ( Bare metal with virtualization enabled )

  > When using an already existing instance, you can spin up a container in the machine with:

  ```bash
  docker-machine create --driver generic \
    --generic-ip-address "<publicIP_of_instance>" \
    --generic-ssh-user "ubuntu" \
    --generic-ssh-key ~/.ssh/id_rsa \
    hello-world
  ```

 Note the following details: You can refer to the official [aws driver documentation](https://docs.docker.com/machine/drivers/aws/)

  > You can spin up an instance at the same time creating docker containers inside the instance.

+ AWS_ACCESS_KEY_ID
+ AWS_SECRET_ACCESS_KEY
+ AWS_AMI
+ AWS_DEFAULT_REGION
+ AWS_ZONE
+ AWS_SSH_USER # default user for the instance ( ubuntu )
+ AWS_ROOT_SIZE
+ AWS_SECURITY_GROUP
+ AWS_SSH_KEYPATH
+ AWS_VPC_ID
+ AWS_SUBNET_ID
+ AWS_KEYPAIR_NAME
+ AWS_INSTANCE_TYPE
+ AWS_VOLUME_TYPE
+ MACHINE_NAME

```bash
docker-machine create --driver amazonec2 \
    --amazonec2-access-key AKI****** \
    --amazonec2-secret-key Jei****** \
    --amazonec2-open-port 2376 \
    --amazonec2-region us-east-1 \
    --amazonec2-zone f \
    --amazonec2-ami ami-07d0cf3af28718ef8 \
    --amazonec2-instance-type m5a.xlarge \
    --amazonec2-keypair-name instance-keypair \
    --amazonec2-ssh-keypath ~/.sshid_rsa \
    --amazonec2-root-size 16 \
    --amazonec2-security-group <insert_security_group_here> \
    --amazonec2-ssh-user ubuntu \
    --amazonec2-vpc-id vpc-********* \
    --amazonec2-subnet-id subnet-********** \
    --amazonec2-use-ebs-optimized-instance \
    --amazonec2-use-private-address \
    --amazonec2-volume-type gp2 \
    ${MACHINE_NAME}
```

### Essential docker-machine commands

You can confirm the number of docker-machines you have with

```bash
docker-machine ls
```

To change local docker environment variables to the remote machine ones

```bash
eval $(docker-machine env remote-machine)
```

To validate which docker-machine you point to:

```bash
docker-machine active
```

To ssh into the remote machine. Once the machine is created, it’s really easy to ssh into it because the SSH certificates are generated on the machine and kept locally.

```bash
docker-machine ssh remote-machine
```

To copy files to/from the machine

```bash
docker-machine scp ~/localfile.txt remote-machine:~/
```

To go back to using your local instance

```bash
eval $(docker-machine env -u)
```

### What if you want to share the machines?

Install [machine-share](https://github.com/bhurlow/machine-share)

```bash
npm install -g machine-share
```

To export machines

```bash
$ machine-export <machine-name>
>> exported to <machine-name>.zip
```

To import machines

```bash
machine-import <machine-name>.zip
>> imported
```
