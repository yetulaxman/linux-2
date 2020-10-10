---
title: Adding Content to Running Containers
---
## Learning objectives
In this example you will learn:
- how to connect to a running container
- how to install a package inside container

## Attaching to your container

If you want to go into a container to execute some command you can use the following docker option:
- ``exec`` Executing another process inside the container. This could be a shell, or a script of some sort.

> NOTE:
> When you attach to an already started container, you can stop it by `Ctrl+d` or detach yourself by `Ctrl+p Ctrl+q`

First, start up an Nginx container:

```bash
docker container run -d -p 8000:80 nginx
661c6dd59d78b97f8142d67eff6b1d58fbbd42247900241e08f46abdbad19f06
```

Try to attach to the container. Exit it, and browse the webpage again to acknowledge it is gone.

Step into a new container by executing a bash inside the container:

```bash
docker container run -d -p 8000:80 nginx
docker exec -it nginx ps -aux # View Running Processes inside container
```
You can now able to launch a website at `localhost:8000`. on your browser.

## Adding content inside the container.

As a good design principle, containers only have the bare minimum installed. Nginx container does not have vim edior, let's install vim editor here. Go inside the container and type the following command:

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
