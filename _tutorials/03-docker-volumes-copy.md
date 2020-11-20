---
title: Docker Volumes
---

# Learning Objectives
In scientific appplications, the usage of docker volumes is essential to persist data when working containers. In this session, you will be able to learn:
- How docker enables data persistence
- Different volumes types and how they are created

## Description

Docker images are stored as read-only layers. When we start a container from a image, Docker takes the read-only image and adds a read-write layer on top (Union File System). If a file in the  running container is modified, the file is copied out of the underlying read-only layer and into the top-most read-write layer where the changes are applied. As containers are ephemeral, any changes made inside the container are lost permanently once container is removed.

 In order to save (persist) data and also to share data between containers, Docker came up with the concept of volumes. Quite simply, volumes are directories (or files) that are outside of the default Union File System and exist as normal directories and files on the host filesystem.

We are essentially going to look at how to manage data within your Docker containers. You have two different ways of mounting persistent data from your container:

- `Bind mounts` (=Mounting a Host Directory as a Data volume)
- `Docker volumes`

> **Note**: Other type of mount called, **tmpfs mount** which is stored in the host system’s memory only and are never written to the host filesystem. When the container stops, the tmpfs mount is removed, and files written there won’t be persisted. This may be your option If you do't want the data to persist either on the host machine or within the container.  This may be for security reasons or to protect the performance of the container when your application needs to write a large volume of non-persistent state data.

**A bind mounts** : When you use a bind mount option, a file or directory on the host directory is mounted into a container.  While  it is easy and connects directly to the host filesystem, non-docker processes on the Docker host  can modify them at any time. One needs to specify it at runtime with no indication on what mounts a given container has, and that you need to deal with backup, migration etc. in a tool outside the Docker ecosystem.


## Bind mounts example

Now that your are familiar with basic docker commands, let us try some real world docker container. For now don't worry about the context of this container.

Pull the latest Docker image for Trinity like so:

```
docker pull trinityrnaseq/trinityrnaseq

````

Download trinity dump from allas object storage.

and mount the file inside the trinity container.

```
wget https://a3s.fi/trinity/Luke_test_trinity.zip

unzip Luke_test_trinity.zip

or

scp yetukuri@puhti.csc.fi:/scratch/project_2003682/Trinity/*.gz .

```

Run Trinity like so (eg. as shown where with a very small test data set):

```
docker run trinityrnaseq/trinityrnaseq Trinity \
      --seqType fq \
      --left `pwd`/reads.left.fq.gz \
      --right `pwd`/reads.right.fq.gz
      --max_memory 1G --CPU 4 --output `pwd`/trinity_out_dir

```
issue:
> Error, cannot locate file: /home/ubuntu/General/training/Lukr_test/reads.left.fq.gz at /usr/local/bin/trinityrnaseq/Trinity line 2774.

We need to create a mapping between the host system, and the container with the `-v` command:

``` bash
docker run --rm -v `pwd`:`pwd` trinityrnaseq/trinityrnaseq Trinity \
     --seqType fq \
     --left `pwd`/reads_1.fq.gz \
     --right `pwd`/reads_2.fq.gz
     --max_memory 1G --CPU 4 --output `pwd`/trinity_out_dir

```

It should work fine and all resuts will be written to "trinity_out_dir" inside the current directory.  if you are root user this is fine, you can view the results.

```
-rw-r--r-- 1 root root   200578 Jun 12 08:29 Trinity.fasta
-rw-r--r-- 1 root root     3125 Jun 12 08:29 Trinity.fasta.gene_trans_map
-rw-r--r-- 1 root root      791 Jun 12 08:29 Trinity.timing
```

what if you are not a root user?

That will map whatever files are in the `pwd` folder on the host to `pwd` in the container.

```

sudo docker run -v `pwd`:`pwd` trinityrnaseq/trinityrnaseq ls -l `pwd`

```

how do we check if the 'pwd' folder is indeed created inside the container.

```

* Run a `docker container ls` inside the container to check if the files are run.

how do we make host volume read-only?

> The `:ro` attribute is making the host volume read-only, making sure the container can not edit the files on the host.


how to avoid permission issues With Docker-Created Files?
(https://vsupalov.com/docker-shared-permissions/)
The user of the container (root in the worst case) is completely different than the one on the host.As the container ran with the “root” user by default, we won’t be able to use those files from the host. One way to fix them temporarily, is to take ownership of them again and again and again:

chown -R hostuser:hostuser shared

It is tedious and have to do everytime you do it

Set the Docker user when running your container:


  ```bash
  docker run --rm -v `pwd`:`pwd`\
   --user "$(id -u):$(id -g)" \
  trinityrnaseq/trinityrnaseq Trinity \
       --seqType fq \
       --left `pwd`/reads_1.fq.gz \
       --right `pwd`/reads_2.fq.gz
       --max_memory 1G --CPU 4 --output `pwd`/trinity_out_dir

  ```

–user “$(id -u):$(id -g)“’ ->  tell the container to run with the current user id and group id which are obtained dynamically through bash command substitution by running the “id -u” and “id -g” and passing on their values.


```
total 41664
-rw-r--r-- 1 ubuntu ubuntu   199157 Jun 12 08:40 Trinity.fasta
-rw-r--r-- 1 ubuntu ubuntu     3264 Jun 12 08:40 Trinity.fasta.gene_trans_map
-rw-r--r-- 1 ubuntu ubuntu      791 Jun 12 08:40 Trinity.timing
````
now you have the right permissions to check files and go for the further analysis


**A docker Volume** is where you can use a named or unnamed volume to store the external data. You would normally use a volume driver for this, but you can get a host mounted path using the default local volume driver. when you use a volume, a new directory is created within Docker’s storage directory on the host machine, and Docker manages that directory’s contents.

In the next section, you will get to try both of them.

## Docker Volume example

Volumes are entities inside docker, and can be created in three different ways.

* By explicitly creating it with the `docker volume create <volume_name>` command
* By creating a named volume at container creation time with `docker container run -d -v DataVolume:/opt/app/data nginx`
* By creating an anonymous volume at container creation time with `docker container run -d -v /opt/app/data nginx`

First off, lets try to make a data volume called `data`:

```bash
docker volume create data
```

Docker creates the volume and outputs the name of the volume created.

If you run `docker volume ls` you will have a list of all the volumes created and their driver:

```outputs
DRIVER              VOLUME NAME
local               data
```

Unlike the bindmount, you do not specify where the data is stored on the host.

In the volume API, like for almost all other of Docker’s APIs, there is an `inspect` command giving you low level details. Let’s use it against the html volume.

```bash
docker volume inspect data
[
    {
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/data/_data",
        "Name": "data",
        "Options": {},
        "Scope": "local"
    }
]
```

You can see that the `data` volumes is mounted at `/var/lib/docker/volumes/data/_data` on the host.

> **Note** we will not go through the different drivers. For more info look at Dockers own [example](https://docs.docker.com/engine/admin/volumes/volumes/#use-a-volume-driver).

You can now use this data volume in all containers. Try to mount it to an nginx server with the `docker container run --rm --name www -d -p 8080:80 -v data:/usr/share/nginx/html nginx` command.

> **Note:** If the volume refer to is empty and we provide the path to a directory that contains data in the base image, that data will be copied into the volume.

Try now to look at the data stored in `/var/lib/docker/volumes/data/_data` on the host:

```bash
sudo ls /var/lib/docker/volumes/data/_data/
50x.html  index.html
```

Those two files comes from the Nginx image and is the standard files the webserver has.

### Attaching multiple containers to a volume

Multiple containers can attach to the same volume with data. Docker doesn't handle any file locking, so applications must account for the file locking themselves.

Let's try to go in and make a new html page for nginx to serve. We do this by making a new ubuntu container that has the `data` volume attached to `/tmp`, and thereafter create a new html file with the `echo` command:

```bash
docker container run -ti --rm -v data:/tmp ubuntu bash
root@9c36fcfcc048:# echo "<html><h1>hello world</h1></html>" > /tmp/hello.html
root@9c36fcfcc048:# ls /tmp
hello.html  50x.html  index.html
```

Head over to your newly created webpage at: `http://<IP>:8080/hello.html`

## cleanup

Exit out of your ubuntu server and execute a `docker container stop www` to stop the nginx container.

Run a `docker container ls` to make sure that no other containers are running.

```bash
docker container ls
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                                                          NAMES
```

The data volume is still present, and will be there until you remove it with a `docker volume rm data` or make a general cleanup of all the unused volumes by running `docker volume prune`.

## Tips and tricks

As you have seen, the `-v` flag can both create a bindmount or name a volume depending on the syntax. If the first argument begins with a / or ~/ you're creating a bindmount. Remove that, and you're naming the volume. For example:

* `-v /path:/path/in/container` mounts the host directory, `/path` at the `/path/in/container`
* `-v path:/path/in/container` creates a volume named path with no relationship to the host.

### Sharing data

If you want to share volumes or bindmounts between two containers, then use the `--volumes-from` option for the second container. The parameter maps the mapped volumes from the source container to the container being launched.

# Conclusion
Volumes are the Docker way of persisting data beyond the lifetime of a container. In this exercise, we saw how to create and destroy volumes; how to mount volumes when running a container; how to find their locations on the host (under /var/lib/docker/volumes) and in the container using docker volume inspect and docker container inspect; and how they interact with the container’s union file system.
