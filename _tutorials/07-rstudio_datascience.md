---
title: Serving Containerised Web Application - Rstudio Demonstration
---
 RStudio is a graphical interface to the software R and is heavily used in statistical data analysis. Main advantages of using Rstudio as a docker container include:
  -  Reproducibility of analysis results
  -  Collaboration with colleagues
  
Learning Objectives:
- Launching RStudio as a Docker container
- Performing a simple data analysis task using R


## How to run

Run using the default password from the Dockerfile build script:
```
docker run -d -p 0.0.0.0:8080:8787 -i -t rocker/rstudio
```

In case you want to set a more secure password for rstudio, you can pass an environment variable for password as below:

```
docker run -d -p 0.0.0.0:8080:8787 -e USERPASS=secretpassword  -i -t rocker/rstudio
```

In case you want to persist data after analysis, you can mount data directory  with `-v` flag as below:

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

we will use prcomp function from `stats` package. 

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

