---
title: Sharing Docker Images
---
## Learning Objectives 

We’ve spent sometime in gaining a reasonable understanding of running a docker container. Sometimes, we have to go bit further to customise docker images by adding missing software tools or any package dependencies you need for your  analysis. How can you re-use your custom image after sometime later or even share it with other collaborators. This brings us to the point of sharing your image with others!

Upon completion of this session, you will learn: 

- How to share your Docker image w/o DockerHub.

### Sharing Docker Images without DockerHub (=tarball approach)

Sharing a docker image means making your local image available for other people to use.

Docker has *save* command option where you can save your image into a tar file. Let's look at our fastqc container into which you have installed vim editor and you wish to share the new image now with your friend.

Find the fastqc container id and use the following *docker commit* command:

```bash
docker commit <container id> fastqc-vim:test
```
> note: It is a good idea to stop the container before saving it 

and then save the image using `docker save` command

```
docker save d55c1457185e -o  fastqc-vim.test.tar

```
You can check the existance of tar file in your directory after *docker save* command has been executed successful. You can send this tar file to your colllaborators/friends.

One can load the image from the tar file uisng *docker load * command. 

```bash
docker load < fastqc-vim.test.tar
```
Once image is loaded you can now use as if it is downloaded from dockerhub.

### Sharing your image with DockerHub (=registry approach)

Sharing an image *via* docker registry such as  DockerHub (the most popular image registry, hosting hundreds of thousands of images) is an efficient way of sharing and managing your images. Once image is in a docker registry, anyone can pull it from there. 

However, this involves setting-up an account in Docker registry. Here are few steps you can do to set-up your account:

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
