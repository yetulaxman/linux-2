## Accessing and running singularity containers ##

## https://bioinformaticsworkbook.org/dataAnalysis/RNA-Seq/RNA-SeqIntro/RNAseq-without-a-genome.html#gsc.tab=0
```
#!/bin/bash
#SBATCH --time=00:10:00
#SBATCH --partition=test
#SBATCH --account=project_2001659
export TMPDIR=/scratch/project_2001659/$USER
#export SINGULARITY_CACHEDIR=/scratch/project_2001659/$USER  # store all cache in this directory including image file; by default singularity stores at $HOME/.singularity//cache/shub or oci-tmp

# singularity commands

#singularity run shub://nuitrcs/hello-world #This will run the default script commands written while building image; This is kind of ENTRYPOINT commands in docker container
#singularity build ubuntu-18.10.simg docker://ubuntu:18.10  # download layers from Docker Hub and assemble them into Singularity container
#singularity pull  --name /scratch/project_2001659/$USER/rstudio.simg docker://rocker/tidyverse:latest

singularity exec --bind $PWD trinityrnaseq.img \
 Trinity --seqType fq \
 --max_memory 120G \
 --CPU 16 \
 --normalize_by_read_set \
 --output TrinityOut \
 --left left_1.gz \
 --right right_2.gz \
 --trimmomatic

```
