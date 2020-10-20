---
title: Serving Containerised Web Application - Rstudio Demonstration
---
R and RStudio are heavily used in data analysis. RStudio is a useful graphical interface to the software R. This exercise demonstrates using R wihtin RStudio via a web interface running in a docker container.

Main advantages using Rstudio as docker container:
  -  Do data science interactively within the RStudio IDE,
  -  Reproduce your analyses,
  -  Collaborate and share code with others, and
  -  Communicate your results with others.

Learning Objectives:
- Launching RStudio inside of a Docker container
- Performing some data science activties with R


## How to run

Run using the default password from the Dockerfile build script:
```
sudo docker run -d -p 0.0.0.0:8080:8787 -i -t rocker/rstudio
```

PROTIP: You will probably want to  something more secure than an account named guest with the password guest, so you will probably want pass in the
guest user password when you instance the container.

```
docker run -d -p 0.0.0.0:8080:8787 -e USERPASS=secretpassword  -i -t rocker/rstudio
```

You probably want the user's home directory to persist, so if the container restarts
the users' work is not blown away. To do this, map a home directory like this:

```
docker run -d -e USERPASS=secretpassword  \
        -v /external/directory/for/user:/home/guest \
        -p 0.0.0.0:8787:8787 -i -t  rocker/rstudio
```

## How to access

To access the app, point your web browser at
    http://your.hostname.here:8787/

Dockerfile for RStudio Server

## Perform PCA analysis

