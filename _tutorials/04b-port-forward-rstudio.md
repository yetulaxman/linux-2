---
title: Port-forwarding in docker services
---

## Learning objectives

In this session you will learn how to perform port mapping or port forwarding

## Port forwarding using rstudio webserver

In Docker, the containers themselves can have applications running on ports. When you run a container, if you want to access the application in the container via a port number, you need to map the port number of the container to the port number of the Docker host.

Running arbitrary Linux commands inside a Docker container is fun, but let's do something more useful.

Pull down the ``rstudio`` Docker image from the Docker Hub.

Start a new container from the ``rstudio`` image that exposes port 80 from the container to port 8087 on your host. You will need to use the ``-p`` flag with the docker container run command.

> NB: Mapping ports between your host machine and your containers can get confusing.
> Here is the syntax you will use:
>
> ```bash
> docker run --rm -p 8787:8787 -e PASSWORD=yourpasswordhere rocker/rstudio
> ```
>
> The trick is to remember that **the host port always goes to the left**,
> and **the container port always goes to the right.**
> Remember it as traffic coming _from_ the host, _to_ the container.

Open a web browser and go to port 8080 on your host. The exact address will depend on how you're running Docker today:

* **Native Linux** - [http://localhost:8787]
* **Cloud server** - Make sure firewall rules are configured to allow traffic on port 8080. Open browser and use the hostname (or IP) for your server.


If you see a webpage saying "Welcome to rstudio!" then you're done!

If you look at the console output from docker, you see nginx producing a line of text for each time a browser hits the webpage:

```bash
sofus@Praq-Sof:~/git/docker-exercises$ docker container run -p 8080:80 nginx
172.17.0.1 - - [31/May/2017:11:52:48 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:53.0) Gecko/20100101 Firefox/53.0" "-
```

Press **control + c** in your terminal window to stop your container.

## Working with your docker container

When running a webserver like nginx, it's pretty useful that you do not have to have an open session into the server at all times to run it.
We need to make it run in the background, freeing up our terminal for other things.
Docker enables this with the `-d` parameter for run.
`docker container run -p 8080:80 -d nginx`

```bash
sofus@Praq-Sof:~/git/docker-exercises$ docker container run -p 8080:80 -d nginx
78c943461b49584ebdf841f36d113567540ae460387bbd7b2f885343e7ad7554
```

Docker prints out the container ID and returns to the terminal.
