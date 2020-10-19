---
title: Hello-World Docker Tutorial
---


## Learning Objectives
As you have Docker client installed on your Oracle VirtualBox, let's have some fun by running docker containers. In this session, you will be able to: 
- See Docker image in action
- Learn where to find pre-made docker images
- Understand the mechanism behind running a docker container

## Tutorial with Hello-world Container

Let’s start a simple pre-made hello-world container from [Dockerhub](https://hub.docker.com) which is a docker registry for sharing images. Try running following command on your linux terminal:

```bash
docker run hello-world
```
when docker is a command is run  on commandline interface (CLI), Docker client contacts Docker daemon to check if there is an image name named, "hello-world". If the image is not found locally, the daemon pulls "hello-world" image from DockerHub (default for docker container and for other registries one has to explicitly mention docker registry name) and  creates a new container from that image which runs the executable that produces the output you are currently reading. The Docker daemon streamed that output to the Docker client, which sent it to your terminal. hmm, lot of stuff happened behind the scenes !!!

**Note:** Depending on how you've installed docker on your system, you might see a `permission denied` error after running the above command.

1. What is the  terminal output from the above command?

```bash
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
78445dd45222: Pull complete
Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

2. What will happen if you run above command again? Did you observe any change in terminal output now?

Docker image has already been downloaded locally and therefore docker can execute the container straight away.

4. What is the tag used in the hello-world example?

By default, image is pulled with latest tag. It is possible to pull an image with specified tag and is a good practice to use specific tag name for reproducible registry. Tag is more like as version control mechanism for docker images.

Congratulations, you have run a “Hello World” in Docker!

## Bonus exercise

1. Run a container from the image named "alpine" from DockerHub and execute a command inside the container so that the output from the container is "hello-world".

2. Name few other docker registries besides DockerHub. How would you run the same *hello-world example* from a different docker registry?
Many other docker registries (=storage and distribution system for named Docker images) do exist besides DockerHub. Few examples include:
- Google Container Registry  
- REDHAT Quay Container Registry
- Amazon Elastic Container Registry
- Azure Container Registry

Default registry for docker client is DockerHub. For other registries, one has to write full name of registry/repository specific to an image depending on the registry.
