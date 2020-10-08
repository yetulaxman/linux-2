
---
## Learning objectives
One of the most useful and often used docker commands  is "docker run ..." . We have briefly touched it in the "hello-world" example. Here, let's go beyond hello-world and explore more options in running containers.
You will learn
- Launching docker image with more options
- Running containers interactively
- Running docker containers in the background
- Listing images and containers

## Launching docker image with more options

In the previous hello-world example, docker run command  not only implicitly fetched its image from DockerHub and but also launched the container on your host machine. Let's play here with a bioinformatics container called, [fastqc](https://hub.docker.com/r/biocontainers/fastqc) instead.

To get started, let's pull *fastqc* image in the following way:

```bash
docker pull biocontainers/fastqc:v0.11.9_cv6
```
*Note*: if you don't provide tag (i.e., v0.11.9_cv6), docker client looks for the tag "latest" which may or may not present in DockerHub. While image is being downloaded, look for all available tags for *fastqc* image of *biocontainers* repository.

Here, the `pull` command fetches the *fastqc* image from the **Docker registry** and saves it in your system. It will not run (=create any container from image) image yet.


Great! Now let's run a Docker **container** based on this image. To do that you are going to use the `docker run` command.

```bash

docker run biocontainers/fastqc:v0.11.9_cv6 echo "hello from fastqc"

docker run biocontainers/fastqc:v0.11.9_cv6  ls -l /

```
When you run `docker container run ...`, with a command (`echo`), please note that command was actually run inside the container and the container is exited.


## Docker run interactively

In all the cases mentioned above, docker ran the the commands inside the container and exited once the command is executed.   These non-interactive shells will exit after running any scripted commands, unless they are run in an interactive terminal.

Running images interactively is a useful for testing or development, for example if you want to test some installation instructions inside software. So let's try the following command:

```bash

docker container run -it biocontainers/fastqc:v0.11.9_cv6 /bin/sh

```
Here you ran the  `docker run` command with special flags `-it` so that it attaches you to an interactive tty in the container. You can run as many commands in the container as you want! Take some time to run your favorite commands. Remember, you can write `exit` when you want to quit.

here is an example on how one can get help on fastqc usage:

```bash
docker container run -it biocontainers/fastqc:v0.11.9_cv6 fastqc --help
```

*Note*: the flags `-it` are short for `-i -t` which again are the short forms of `--interactive` (Keep STDIN open) and  `--tty` (allocate a terminal).

## Run docker containers in the background

Often you will want to run an image in the background. Try running *fastqc* in the background mode using flag .-d` (--detach)

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

List running all containers using the `ps -a` command as below:

```bash

docker ps -a  

```
In the above steps, we saw how to get the short container ID of all our containers using `docker ps -a`

Try adding the `--no-trunc`   flag to see the entire container ID:

```bash
 docker container ps -a --no-trunc
```
This long ID is the same as the string that is returned after starting a container with
`docker run ...`

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

We explored few docker commands in this short tour of docker commands. The `docker  run ...` command is the more often used command in our daily life. It makes sense to spend some time getting comfortable with it. To find out more about `run`, use `docker container run --help` to see a list of all flags it supports.
