---
title: Sharing Docker Images
---
## Learning objectives
 You will learn how to share your Docker image with the world!

We often times need to share  our dockerized image to another person. In order to share a image,  one has to push an image  to a docker registry like DockerHub  (i.e, it is a kind of home for Docker images in the cloud) so that anyone can pull it from there to another machine.

- One can create an account on the Docker Hub [here](https://hub.docker.com/account/signup/). After verifying your email you are ready to go and upload your first docker image.
- Click on Create Repository.
- Choose a name  and a description for your repository and click Create.

Then, login to that account by running the ``docker login`` command on your laptop.

```
docker login --username=yourhubusername --email=youremail@company.com
```
We're almost ready to push our Flask image up to the Docker Hub. We just need to rename it to our namespace first.

Using the ``docker tag`` command, tag the image you created in the previous section to your namespace. For example, I would run:

```bash
docker tag bb38976d03cf yourhubusername/verse_gapminder:firsttry
```

``firsttry`` is the tag I used in my ``docker build`` commands in the previous section, and ``ourhubusername/verse_gapminder:`` is the full name of the new Docker image I want to push to the Hub.
`yourhubusername` is my username at dockerhub, and also my namespace for all my images.
The `:latest` is a versioning scheme you can append to.

All that's left to do is push up your image:

```
 docker push yourhubusername/verse_gapminder
```

Push your image to the repository you created

docker push yourhubusername/verse_gapminder
Your image is now available for everyone to use.


Go to your profile page on the Docker Hub and you should see your new repository listed:
[https://hub.docker.com/repos/u/<username>](https://hub.docker.com/repos/u/<username>)


**References**:
- For more info on the Docker Hub, and the cli integration,
head over to [https://docs.docker.com/docker-hub/](https://docs.docker.com/docker-hub/) and read the guides there.
-  https://runnable.com/docker/rails/manage-share-docker-images
