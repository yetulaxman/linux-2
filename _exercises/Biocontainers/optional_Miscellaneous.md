# Description

Environment variable exercise

## Run instructions

Build the image as follows

    docker build -t testimage .

Run it and set the environment variable `myhost`

    docker run -e myhost=host1 testimage

You should get this as the output.

    host1

Run it without setting the env variable and you should get this as output

    docker run testimage

    testhost

#############################################
# Description
What size image does this Dockerfile create?
What size of archive does it create if you export it to a tarball?
What else is in the image that was built?

Notice that it uses the scratch base image and adds a small text file to it. To find out the size of the image you have to build the image and then use inspect to get the size.

## Run instructions

    docker build -t smallimage .
    docker inspect -f '{{ .Size }}' smallimage
    2
    docker save -o a.tar smallimage
    ls -l a.tar
    mkdir tmp
    cd tmp
    tar xvf ../a.tar
##############################################
# Description
What command will most quickly tell you what is contained in an Alpine Linux /etc/hosts file?

## Run instructions

A quick way to do this is to run the container and include a command to output the file that you are interested in like this.

    docker run --rm alpine cat /etc/hosts

If the container is already running, another way is to use the `docker cp` command and copy it to your local host.

    docker container cp /etc/hosts  containername:/tmp
###########################################
# Description
How can you quickly and succinctly determine the image id and created date for an Alpine image?

You can use the `inspect` command to access the image metadata and the `--format` argument in conjunction with it to extract only specific fields like this.

## Run instructions

    docker inspect alpine:latest --format='{{.Id}} {{ .Created}}'
################################################
# Description
Use inspect to get metadata for a resource.

## Run instructions

    docker network create testthing
    docker volume create testthing
    docker run --name=testthing alpine date
    docker image tag alpine testthing

Now try this

    docker inspect testthing

Did you get the output that you expected?

Potentially not.

This is because several resource types can be given names and nothing prevents the same name from being given to different resources (but not the same resource type).

Note that you may need to use inspect with a type argument to indicate whether you want a the data for a container, image or other resource.

Which resource did you get the output for?

Try the following to be more specific and compare the outputs.

    docker inspect --type=container testthing
    docker inspect --type=image testthing
    docker inspect --type=container testthing

Getting an unexpected resource type in the output can confuse automated processing so it is best to always be specific when using inspect with a resource name.
####################################################
# Description
Run the following containers and note some containers failed and exited with an error status.
Use inspect to show the exit status of only the failed containers.

Did you know that you can include conditionals in format statements?

## Run instructions

    docker run ubuntu date
    docker run ubuntu date1
    docker run ubuntu date2
    docker run ubuntu date


## Answer
The following will output the name and exit status of eaach container that failed and has not been removed.
Note that the if portion gets the state of all containers no longer running with a non-zero exit code.
The if condidtion is contained in the {{}} and the fields to be output follows.

    docker inspect -f '{{if ne 0 .State.ExitCode}}{{.Name}} {{.State.ExitCode}}{{ end}}' $(docker ps -aq)

    /blissful_thompson 127
    /boring_colden 127

    /condescending_mcclintock 127
######################################################
# Description
Docker images can be interesting to look around in.
You can save images to a tar archive and then extract the layers and metadata files from them.

## Run instructions

    docker pull alpine
    docker save -o alpine.tar alpine
    mkdir tmp
    cd tmp

Extract the archive

    tar xvf ../alpine.tar

Look at the layers and metadata files that are in the image.
Note that you might be able to find other archives in the layers, extract those and see what you find.
####################################################
# Description
Docker containers can be interesting to look around in.
You can save containers to a tar archive and then extract the layers and metadata files from them.
Note that exported containers look a little different from images that have been saved to an archive.

## Run instructions

Run an alpine container

    docker run -it --name=alpinetest -e tmpvar=test alpine sh

Note that you can see the environment variable that you set in the run command.

    echo $tmpvar

See the other environment variables with `env`

Export the running container to an archive

    docker export -o alpine.tar alpinetest

Now extract the contents of the archive and look around.

    mkdir tmp
    cd tmp
    tar xvf ../alpine.tar

Notice that you do not see the metadata files that are present in a image that has been saved to an archive.
###################################################
# Description

We want to only have ps output show the command used and image name being run but this is not working.
Can you fix it?

    docker ps --format '{{.Command .Image}}'

# Answer

    docker ps --format '{{.Command}} {{.Image}}'

#####################################################
pull different docker images

docker pull pegi3s/clustalomega

docker pull pegi3s/fastqc

docker pull pegi3s/sratoolkit

#####################################################
5.2 Bypass Entry point

The solution to the problem is to override the ENTRYPOINT instruction by providing an alternate option on the docker run command itself. This is accomplished with the --entrypoint option.

To simply explore the pegi3s/clustalomega container without sharing a directory we can therefore use the following command:

docker run -it --rm  --entrypoint "/bin/bash"  pegi3s/clustalomega

######################################################
