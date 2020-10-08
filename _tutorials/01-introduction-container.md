---
title: Essential Docker Termonology
---


### Learning Objective
In this session, you will understand the basic terminology to survive in this course. More docker jargon will be introduced as we go with course.


### 1. Understand the following basic terminology in Docker world.

In this course, you will come across a lot of Docker-specific jargon which might be confusing to the beginners. You can go through glossary of docker jargon [here](https://docs.docker.com/glossary/). Below is the essential terminology of docker ecosystem and we often use in this course:

- *Images* :A Docker image is a read-only file system, comprised of multiple layers, that is used to execute code in a Docker container. A image is created usually from a Dockerfile, which comprises a set of instructions for a complete and executable version of an application. It is a Docker container at rest, i.e., not running.

 <span style="color:green">TIP: It is analogous to class object in programming language text</span>

- *Containers* - Running instances of Docker images &mdash; containers run the actual applications. A container includes an application and all of its dependencies. It shares the kernel with other containers, and runs as an isolated process in user space on the host OS. You created a container using `docker container run` based on an image that was downloaded. A list of running containers can be seen using the `docker container ls` command.

   It is analogous to an instance of docker image; The Docker Image in running state.

- *Docker daemon* - The background service running on the host that manages building, running and distributing Docker containers.

- *Docker client* - The command line tool that allows the user to interact with the Docker daemon.

- *Docker Hub* - A [registry](https://hub.docker.com/explore/) of Docker images. You can think of the registry as a directory of all available Docker images. You'll be using this later in this tutorial.

- *Docker Registry* - A remote repository where Docker Images can be stored and made public. Constitutes the point of contact between application developers and their users and allows the distribution of Docker containers.
