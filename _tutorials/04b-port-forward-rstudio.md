---
title: Port-forwarding in Docker Services
---

## Learning objectives

In this session you will learn how to:
- How to pull and launch Rstudio docker 
- Perform port mapping or port forwarding
- Understand different environment variables associated with the docker

## Port forwarding using rstudio webserver

In Docker, the containers themselves can have applications running on ports. When you run a container, if you want to access the application in the container via a port number, you need to map the port number of the container to the port number of the Docker host.


Pull down the ``rstudio`` Docker image from the Docker Hub.

Start a new container from the ``rstudio`` image that exposes port 80 from the container to port 8087 on your host. You will need to use the ``-p`` flag with the docker container run command.

> NB: Mapping ports between your host machine and your containers can get confusing.
> Here is the syntax you will use:
>
> ```bash
> docker run --rm -p 8080:8787 -e PASSWORD=yourpasswordhere rocker/rstudio
> ```
>
> The trick is to remember that **the host port always goes to the left**,
> and **the container port always goes to the right.**
> Remember it as traffic coming _from_ the host, _to_ the container.

Open a web browser and go to port 8080 on your host. The exact address will depend on how you're running Docker today:

* **Native Linux** - [http://localhost:8080]
* **Cloud server** - Make sure firewall rules are configured to allow traffic on port 8080. Open browser and use the hostname (or IP) for your server.


If you see a webpage saying "Welcome to rstudio!" then you're done!


## Working with your docker container

When running a webserver like rstudio, it's pretty useful that you do not have to have an open session into the server at all times to run it.
We need to make it run in the background, freeing up our terminal for other things. Docker enables this with the `-d` parameter for run.

> ```bash
> docker run --rm -p 8080:8787 -e PASSWORD=yourpasswordhere rocker/rstudio
> ```

**Exercise questions**:

1. Browse Docker Hub for interesting images. What could be useful to you?
2. Can you update a R package of your choice to a running container?

