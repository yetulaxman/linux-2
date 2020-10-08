---
title: Serving containerised web application - Rstudio demonstration
---
R and RStudio are heavily used in data analysis. RStudio is a useful graphical interface to the software R. This exercise demonstrates using R wihtin RStudio via a web interface running in a docker container.

While it is possible to install R and RStudio natively on your own comptuter it may be useful to run an older version, or, as we’ll see in a future tutorial, create your own docker image with specific R options.

With all the experience of the previous section we’ll now follow the process to:


Dockerfile for RStudio Server

Docker image
------------

Pull the image from [Docker Hub](https://hub.docker.com/r/dceoy/rstudio-server/).

```sh
$ docker pull dceoy/rstudio-server
```

Default username and password:

  - username: `rstudio`
  - password: `rstudio`

Usage
-----

Run a server

```sh
$ docker container run --rm -p 8787:8787 -v ${PWD}:/home/rstudio -w /home/rstudio dceoy/rstudio-server
```

Run a server with docker-compose


```
