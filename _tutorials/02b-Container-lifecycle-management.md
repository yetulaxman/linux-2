---
title: Management of Docker Container Life-cycle
---
## Learning Objectives
One of the most useful and often used docker commands is `docker run ...` . We have briefly seen it in `hello-world` example. Here, let's go beyond simple `hello-world` example and explore few more related docker commands.

In this episode, you will learn: 
- Some of usefull Docker commands to manage images/containers
- How to run containers interactively
- Running docker containers in the background
- How to list images and containers in host system

## Launching docker image with more options

In our previous `hello-world` example, `docker run` command not only implicitly fetched its image from DockerHub and but also launched a container on your host machine. What if you just want to download a docker image but not yet ready for running a contaianer. No worries ! we've `docker pull ...` command for the same purpose. This time, we'll however  use a well-known bioinformatics container, [fastqc](https://hub.docker.com/r/biocontainers/fastqc).

To get started, let's pull *fastqc* image in the following way:

```bash
docker pull biocontainers/fastqc:v0.11.9_cv6
```
*Note*: if you don't provide tag (i.e., v0.11.9_cv6), docker deamon looks for `fastqc` image with tag "latest" which may or may not present in DockerHub. While image is being downloaded, look for all available tags for *fastqc* image of *biocontainers* repository.

Here, the `docker pull ...` command fetches the *fastqc* image from the **Docker registry** and saves it locally in your system. It will not run (=create any container from image) image yet.


Great! Now let's run a Docker **container** based on this image. To do that we are going to use the `docker run ...` command.

```bash

docker run biocontainers/fastqc:v0.11.9_cv6 echo "hello from fastqc"

docker run biocontainers/fastqc:v0.11.9_cv6  ls -l /

```
When you run `docker container run ...`, with a command (`echo`), please note that command was actually run inside the container and the container is exited after running `echo` command.


## Docker run interactively

In previous examples, `docker run ...` command exited once a user-defined (or default) command is executed inside the container. You're probably thinking if there is a way to run more than just one command in a container. So we want command prompt back after running a command. This is possible *via* running docker containers interactively. Running images interactively is useful for testing or development. So let's try the following command:

```bash

docker container run -it biocontainers/fastqc:v0.11.9_cv6 /bin/sh

```
Here you ran the  `docker run ...` command with special flags `-it` so that it attaches you to an interactive tty in the container. You can run as many commands in the container as you want! Take some time to run write a useful command (e.g., get some documentation help from a software). Remember, you can write `exit` when you want to quit.

here is an example on how one can get help on fastqc usage:

```bash
docker container run -it biocontainers/fastqc:v0.11.9_cv6 fastqc --help
```

*Note*: the flags `-it` are short for `-i -t` which again are the short forms of `--interactive` (Keep STDIN open) and  `--tty` (allocate a terminal).

## Run docker containers in the background

The main disadvantage of running a container in the foreground (the default mode for dockers) is that you can not access the command prompt anymore. That means you can't run any other commands while the `fastqc` container is running. In ordder to run a `fastqc` container in the background mode, you can use flag `-d` (--detach).

Let's launch `fastqc`in the bacground (=detached) mode as below:

```bash
docker container run -it -d biocontainers/fastqc:v0.11.9_cv6 /bin/sh
docker ps -a
````
## Listing images and containers

if you start working multiple containers, it is possible that multiple images and containers are hanging around in the host system. It is time to manage them now.

List running containers using the `ps` command as below:

```bash
docker ps #command shows you all containers that are currently running.
```
All containers have an **ID** and a **name**. Both the ID and name is generated every time a new container spins up with a random seed for uniqueness. If you want to assign a specific name to a container then you can use the `--name` option. That can make it easier for you to reference the container going forward.

List running all containers using `ps -a` command as below:

```bash

docker ps -a  

```
In the above steps, we saw how to get the short container ID of all our containers using `docker ps -a`

Try adding the `--no-trunc` flag to see the entire container ID:

```bash
 docker container ps -a --no-trunc
```
This long ID is the same as the string that is returned after starting a container with. `docker run ...`

in order to list only the container IDs using the `-q` flag

```bash
docker ps -a -q
```
What you see  a list of all containers that you ran. Notice that the `STATUS` column shows that these containers exited a some time ago.

You can also filter results with the --filter flag; for example, try filtering by exit code:
```bash
docker ps -a --filter "exited=1"
docker ps -a --filter "exited=0"

```

You can also check whether the fetched image is available on the host system using the following command:

use the `docker image ls` command to see a list of all images on your system.

```bash
docker image ls

```
How many images are stored locally on your workstation?

##  Summary

We explored few docker commands in this short tour of docker commands. The `docker  run ...` command is the more often used command in our daily life. It makes sense to spend some time getting comfortable with it. To find out more about `docker run ...`, use `docker container run --help` to see a list of all flags it supports.
