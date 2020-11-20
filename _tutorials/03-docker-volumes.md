---
title: Docker Volumes
---

# Learning Objectives
In scientific appplications, the usage of docker volumes is essential to persist data when working containers. In this session, you will be able to learn:
- How docker enables `data persistence` 
- Different volumes types and how they are created

## Description

Docker images are stored as read-only layers. When we start a container from a image, Docker takes the read-only image and adds a read-write layer on top (Union File System). If a file in the  running container is modified, the file is copied out of the underlying read-only layer and into the top-most read-write layer where the changes are applied. As containers are ephemeral, any changes made inside the container are lost permanently once container is removed.

 In order to save (persist) data and also to share data between containers, Docker came up with the concept of volumes. Quite simply, volumes are directories (or files) that are outside of the default Union File System and exist as normal directories and files on the host filesystem.

We are essentially going to look at how to manage data within your Docker containers. You have two different ways of mounting persistent data from your container:

- `Bind mounts` (=Mounting a Host Directory as a Data volume)
- `Docker volumes`

> **Note**: Other type of mount called, **tmpfs mount** which is stored in the host system’s memory only and are never written to the host filesystem. When the container stops, the tmpfs mount is removed, and files written there won’t be persisted. This may be your option If you do't want the data to persist either on the host machine or within the container.  This may be for security reasons or to protect the performance of the container when your application needs to write a large volume of non-persistent state data.

**Bind mounts** : When you use a bind mount option, a file or directory on the host directory is mounted into a container.  While  it is easy and connects directly to the host filesystem, non-docker processes on the Docker host  can modify them at any time. One needs to specify it during runtime.

## Bind mounts example

Now that your are familiar with basic docker commands, let us try some real world docker container. For now don't worry about the context of this container.

Pull the latest Docker image for Trinity like so:

```
docker pull biocontainers/fastqc:v0.11.9_cv7

````
copy fastq files from Puhti as below:

```
scp csc-username@puhti.csc.fi:/scratch/project_2003682/Trinity/*.gz .

```

Run Trinity like so (eg. as shown where with a very small test data set):

```
docker run biocontainers/fastqc:v0.11.9_cv7 \
      fastqc /data/reads.left.fq.gz
```
issue:
> Skipping '/data/reads.left.fq.gz' which didn't exist, or couldn't be read

We need to create a mapping between the host system, and the container with the `-v` command:

``` bash
docker run --rm -v /home/biouser/Downloads:/data biocontainers/fastqc:v0.11.9_cv7  fastqc /data/reads.left.fq.gz

```

It should work fine and all resuts will be written to "host directory" that was mounted inside the container.  if you are root user this is fine, you can view the results. Sometimes it can be an issue.

```
-rw------- 1 biouser biouser 14905493 Nov 19 16:08 lung3e_1_subset.fq.gz
-rw------- 1 biouser biouser 14955814 Nov 19 16:08 lung3e_2_subset.fq.gz
-rw-rw---- 1 biouser biouser  1252784 Nov 19 14:22 reads2.left.fq.gz
-rw-rw---- 1 biouser biouser  1274471 Nov 19 14:22 reads2.right.fq.gz
-rw-r--r-- 1 root    root      263555 Nov 20 02:07 reads.left_fastqc.html
-rw-r--r-- 1 root    root      345718 Nov 20 02:07 reads.left_fastqc.zip
-rw-rw---- 1 biouser biouser  1251148 Nov 19 14:22 reads.left.fq.gz
-rw-rw---- 1 biouser biouser  1272939 Nov 19 14:22 reads.right.fq.gz
```

what if you are not a root user?

One tedious way is go inside the container and change the permissions of files. 

Alernative option is to set Docker user when running your container:


```bash     
docker run  --user "$(id -u):$(id -g)" --rm -v /home/biouser/Downloads:/data biocontainers/fastqc:v0.11.9_cv7  fastqc /data/reads.left.fq.gz

```

Above –user “$(id -u):$(id -g)“  flag in run command  informs the container to run with current user id and group id which are obtained dynamically through bash command substitution by running the “id -u” and “id -g” and passing on their values.

Notice the changes in the permissions of files written this time by fastqc as below:

```bash
-rw------- 1 biouser biouser 14905493 Nov 19 16:08  lung3e_1_subset.fq.gz
-rw------- 1 biouser biouser 14955814 Nov 19 16:08  lung3e_2_subset.fq.gz
-rw-rw---- 1 biouser biouser  1252784 Nov 19 14:22  reads2.left.fq.gz
-rw-rw---- 1 biouser biouser  1274471 Nov 19 14:22  reads2.right.fq.gz
-rw-r--r-- 1 root    root      263555 Nov 20 02:07  reads.left_fastqc.html
-rw-r--r-- 1 root    root      345718 Nov 20 02:07  reads.left_fastqc.zip
-rw-rw---- 1 biouser biouser  1251148 Nov 19 14:22  reads.left.fq.gz
-rw-r--r-- 1 biouser biouser   259532 Nov 20 02:17  reads.right_fastqc.html
-rw-r--r-- 1 biouser biouser   340578 Nov 20 02:17  reads.right_fastqc.zip
-rw-rw---- 1 biouser biouser  1272939 Nov 19 14:22  reads.right.fq.gz

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

Unlike the `bind mount` we discussed above, you don't specify where the data is stored on the host. It is actually stored in the file systems managed by docker.

There is a `inspect` command to get low levels details of volumes. Let’s use it against the html volume.

```bash
#command
> docker volume inspect data
# output
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

You can now use this data volume in all containers. Try to mount it inside the fastqc container and write some file to data volume as below:

```
docker run --rm -v data:/data_volume biocontainers/fastqc:v0.11.9_cv7  touch /data_volume/text.txt
```

You can see that the file you created is stored in data volume as below:

```
sudo ls -l /var/lib/docker/volumes/data/_data

-rw-r--r-- 1 root root 0 Nov 20 02:32 text.txt

```

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
