---
title: Serving Containerised Web Application - Rstudio Demonstration
---
R and RStudio are heavily used in data analysis. RStudio is a useful graphical interface to the software R. Main advantages of using Rstudio as docker container include:
  -  Reproduce your analyses,
  -  Collaborate and share code with others
  
Learning Objectives:
- Launching RStudio inside of a Docker container
- Performing some data science activties with R


## How to run

Run using the default password from the Dockerfile build script:
```
docker run -d -p 0.0.0.0:8080:8787 -i -t rocker/rstudio
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
        -p 0.0.0.0:8080:8787 -i -t  rocker/rstudio
```

## How to access rstudio server

To access the app, point your web browser at
    http://localhost:8080/

Dockerfile for RStudio Server

## Perform PCA analysis

Principal component analysis (PCA) is a method of extracting important variables (in form of components) from a large set of variables available in a data set. It extracts low dimensional set of features from a high dimensional data set with a motive to capture as much information as possible.

## How to perform PCA

we will use prcomp function from the stats package. 

```bash
library(ggplot2)
data(iris)
log.ir <- log(iris[, 1:4])

ir.pca <- prcomp(log.ir,
                 center = TRUE,
                 scale. = TRUE) 
# plot method
plot(ir.pca, type = "l")
```
## Visualising PCA

```bash
library(ggbiplot)
g <- ggbiplot(ir.pca, obs.scale = 1, var.scale = 1, 
              groups = ir.species, ellipse = TRUE, 
              circle = TRUE)
g <- g + scale_color_discrete(name = '')
g <- g + theme(legend.direction = 'horizontal', 
               legend.position = 'top')
print(g)

```

