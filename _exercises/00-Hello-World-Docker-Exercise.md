---
title: Hello-World Docker Tutorial
---

## Exercises

### 1. Run a container from a image named "alpine" from DockerHub and execute a command inside that container so that output from the container is "hello-world"

```bash
docker run alpine echo "hello-world"
```

### 2. How would you run the same *hello-world example* from a different docker registry? Name few third-party docker registries besides DockerHub.


Default registry for docker client is DockerHub. For other registries, one has to write fully qualified name of docker images and it would look something like this: *host name/repository/imagename:tag*.

A generic fully qualified name for *hello-world* image would be as below:

```bash
docker run docker.io/library/hello-world  # from dockerHub
```

Many other docker registries (=storage and distribution system for named Docker images) do exist besides DockerHub. Few examples include:
- Google Container Registry  
- REDHAT Quay Container Registry
- Amazon Elastic Container Registry
- Azure Container Registry

