---
title: Sharing Docker Images
---
## Learning Objectives (Work in progress)

We’ve spent sometime in getting a reasonable understanding in running handful of docker containers. We often have to customise docker images by adding missing software tools or any package dependencies you need for your  analysis. How can you re-use the same image after sometime or even share it with other collaborators. This brings us to the point of sharing your image with others!

Upon completion of this session, you will learn: 

- how to share your Docker image with others.

### Sharing Docker Images without DockerHub
Sharing means taking the images we’ve built on your local machine and making them available for other people to use
We often times need to share  our dockerized image to another person


docker has save command where you can save into tar file. We can list the directory after save.

```bash
docker save busybox > busybox.tar

```

Load the image from the tar file. once you load the image you can interact with it and run the container as an official image.

```bash
docker load < busybox.tar
```

### Sharing your image with dockerhub 

Sharing an image via docker registries such as  DockerHub  (i.e, it is a kind of home for Docker images in the cloud) is most effiecient way to share and manage your images. DockerHub is the most popular image registry, hosting hundreds of thousands of images, which are downloaded billions of times every month. Once image is in docker registry, anyone can pull it from there. 

However this involves having an account in Docker registry. Here are few steps you can follow:

- One can create an account on the DockerHub [here](https://hub.docker.com/account/signup/). After verifying your email you are ready to go and upload your first docker image.
- Click on Create Repository.
- Choose a name  and a description for your repository and click Create.

Then, login to that account by running the ``docker login`` command on your virtual machine

```
docker login --username=your-user-name --email=youremail@company.com
```
We're almost ready to push our our docker image up to the DockerHub. We just need to rename it to our namespace first.

Using the ``docker tag`` command, tag the image you created in the previous section to your namespace. For example, I would run:

```bash
docker tag <CID> your-user-name/image-name[:tag]
```

``tag`` is the tag I used in my `docker build` commands in the previous section, and `your-user-name/image-name` is the full name of the new Docker image I want to push to the Hub. `your-user-name` is my username at dockerhub, and also my namespace for all my images. The `:latest` is a versioning scheme you can append to. all images should be tagged with an appropriate prefix before being pushed.

All that's left to do is push up your image:

```
 docker push your-user-name/image-name
```
A push does not happen automatically on rebuilds; docker push should always be executed explicitly.
Push your image to the repository you created. Your image is now available for everyone to use. Go to your profile page on the DockerHub and you should see your new repository listed:[https://hub.docker.com/repos/u/<username>](https://hub.docker.com/repos/u/<username>)
Removing an image from the remote repository is not trivial

**References**:
- For more info on the Docker Hub, and the cli integration,
head over to [https://docs.docker.com/docker-hub/](https://docs.docker.com/docker-hub/) and read the guides there.
-  https://runnable.com/docker/rails/manage-share-docker-images
