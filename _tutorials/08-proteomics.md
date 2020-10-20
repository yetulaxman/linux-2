---
title: Proteomics Web Application
---

# Proteome Network analysis using containerised webapplication #

## learning objectives
- Start a web browser  application as container
- Get a feel of tutorial
- Appreciate the ease of use of web-container.


## Basic usage of docker
- [Check dockerhub](https://hub.docker.com/r/jdrudolph/photon)
- Pull from dockerhub
- docker pull jdrudolph/photon
- Launch application


First open the Docker Quickstart Terminal. After initialization (can take some time), denote the IP address of docker (under the whale image). Now you can run PHOTON by entering:

`bash docker run -d -p 5000:5000 jdrudolph/photon `

Now you can access PHOTON from your browser under the IP address of docker followed by colon and 5000. For example 192.168.0.100:5000. Alternatively you can lookup the IP address using docker-machine ip default.

## Docker for Windows / Linux

Open a Powershell (terminal in linux) and run:

`bash docker run -d -p 5000:5000 jdrudolph/photon `

Now you can access PHOTON from your [browser](http://localhost:5000).



