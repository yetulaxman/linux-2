---
title: Sharing Docker Images
---
## Learning Objectives (Work in progress)

We’ve spent sometime in gaining a reasonable understanding of running a docker container. Sometimes, we have to go bit further to customise docker images by adding missing software tools or any package dependencies you need for your  analysis. How can you re-use you custome image after sometime or even share it with other collaborators. This brings us to the point of sharing your image with others!

Upon completion of this session, you will learn: 

- How to share your Docker image w/o DockerHub.

### Sharing Docker Images without DockerHub

Sharing a docker image means taking the image you’ve built on your local machine and making them available for other people to use.

Docker has *save* command option where you can save your image into a tar file as shown below:

```bash
docker save busybox > busybox.tar

```
You can check tar file in your dierctory after save command is successful. You can send this tar file to your colllaborators.


One can load the image from the tar file. Once your image is loaded, you can interact with it and run the container as an official image.

```bash
docker load < busybox.tar
```

### Sharing your image with DockerHub 

Sharing an image *via* docker registry such as  DockerHub (the most popular image registry, hosting hundreds of thousands of images) is an efficient way of sharing and managing your images. Once image is in a docker registry, anyone can pull it from there. 

However, this involves having an account set-up in Docker registry. Here are few steps you can follow:

- One can create an account on the DockerHub [here](https://hub.docker.com/account/signup/). After verifying your email you are ready to go and upload your first docker image.
- Click on Create Repository.
- Choose a name  and a description for your repository and click Create.

Then, login to that account by running the ``docker login`` command on your virtual machine

```
docker login --username=your-user-name --email=youremail@company.com
```
We're almost ready to push our our docker image up to the DockerHub. We just need to rename it to our namespace first.

Using the `docker tag` command, tag the image you created in the previous section to your namespace. For example, you can run:

```bash
docker tag <CID> your-user-name/image-name[:tag]
```

`tag` is the tag used in `docker build` commands and `your-user-name/image-name` is the full name of the new Docker image we want to push to the Hub. `your-user-name` is your username at dockerhub, and also my namespace for all my images. All images should be tagged with an appropriate prefix before pushing an image.

 Let's push your image as below:

```bash
 docker push your-user-name/image-name
```

Please note that a push does not happen automatically on rebuilds; docker push should always be executed explicitly. Once the Push  to the repository is successfull, your image is now available for everyone to use. Go to your profile page on the DockerHub and you should see your new repository listed:[https://hub.docker.com/repos/u/<username>](https://hub.docker.com/repos/u/<username>)
Removing an image from the remote repository is not trivial

**Useful link**:
- More information about DockerHub, and CLI integration can be found [here](https://docs.docker.com/docker-hub/)
