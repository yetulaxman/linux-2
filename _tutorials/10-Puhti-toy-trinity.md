---
title: Running Singularity Container using Trinity Example 
---

## Accessing and running singularity containers ##

```
#!/bin/bash
#SBATCH --time=02:00:00
#SBATCH --partition=small
#SBATCH --account=project_xxx
#SBATCH --cpus-per-task=5
export TMPDIR=$PWD
singularity exec --bind $PWD:/scratch/project_xxx/Trinity trinityrnaseq.simg \
 Trinity --seqType fq \
 --max_memory 1G \
 --CPU 5 \
 --output $PWD/TrinityOut \
 --left $PWD/reads.left.fq.gz \
 --right $PWD/reads.right.fq.gz \
                                   
```
