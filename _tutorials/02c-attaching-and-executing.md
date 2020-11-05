---
title: Restarting and attaching to containers
---
## Learning Objectives
In this session you will be able to learn:
- How to connect to a running container
- How to install a package inside container

## Executing commands on running containers

If you want to go into a container to execute some ad-hoc commands inside a running container you can use the following docker command:

```bash 
docker exec <options> <container> <command>
```
`exec`  command starts another process inside the container. This could be a shell, or a script of some sort.

> NOTE:
> When you attach to an already started container, you can stop it by `Ctrl+d` or detach yourself by `Ctrl+p Ctrl+q`

First, start up our fastqc container and let it be running in the background:

```bash
docker run -d  biocontainers/fastqc:v0.11.9_cv6 sleep inf
661c6dd59d78b97f8142d67eff6b1d58fbbd42247900241e08f46abdbad19f06
```

You can now check list of containers running in your machine and identify the container ID corresonding to fastqc container

Step into a new container by executing a bash inside the container:

```bash
docker exec -it <container id> bash 

# In this case container id is starting with "661c6dd59...." 
docker exec -it <661c6dd59> bash
```
You are now inside of a running container. 

## Installing content inside the container.

As a good design principle, containers only have the bare minimum installed. dastqc container does not have vim edior, let's install vim editor here. Go inside the container and type the following command:

```bash
apt-get update && apt-get install vim
```
Then detach from the container with `Ctrl+p Ctrl+q`.  

```bash
docker container exec -it <container_id> bash
vi
```
This way you can add some modification to existing container. Please note that containers are not meant to be updated like we did and are more meant to run and disappear after their job is executed.

## Conclusion
Understand Docker Run vs Docker Exec commands. Docker run is the command you useful in creating a brand new container from an image, whereas docker exec lets you run commands on an already running container. You have also seen how to break out of a container either by terminating the process by `Ctrl+d`, or by detaching from the process by `Ctrl+p Ctrl+q`.
