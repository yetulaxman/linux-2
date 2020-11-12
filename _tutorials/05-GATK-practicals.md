---
title: GATK Tutorials
---

### Description
*GATK*: GATK4 toolkit offers a wide variety of tools with a primary focus on variant discovery and genotyping. The content on this page is borrowed from GATK webpages/courses. To get familiar with GATK tools, you can read the following:

- Read overview of GATK4 [here](https://software.broadinstitute.org/gatk/gatk4)
- Quick start guide [here](https://software.broadinstitute.org/gatk/documentation/quickstart)
- Presentations materials and recordings [here](https://software.broadinstitute.org/gatk/documentation/presentations)
- Best Practices workflows [here](https://software.broadinstitute.org/gatk/best-practices/)

### Download GATK container from Dockerhub

```docker pull broadinstitute/gatk:latest``` <br>

  or with some specific-version information  <br>
```docker pull broadinstitute/gatk:4.0.11.0```

### Start up the GATK container

```docker run -it broadinstitute/gatk:latest```

### Run a GATK command in the container

```./gatk --list```

### The general format for GATK commands is
gatk ToolName [tool args]

### exit from the container

```ctrl+p then ctrl+q```

### Download example data
You can download toy example dataset from CSC's allas objects storage:

```bash
wget https://a3s.fi/Softwares/data.zip
```

### Start running gatk container and mount the location of the data bundle inside the docker

``` docker run -v /path/data:/gatk/data -it broadinstitute/gatk:latest ```


### Get usage information of a tool from GATK

```gatk HaplotypeCaller --help```

### Run HaplotypeCaller

``` gatk HaplotypeCaller -R /gatk/data/ref/ref.fasta -I data/bams/mother.bam \ ``` <br>
``` -O /gatk/data/sandbox/variants.vcf ```

> Note: Add JVM options to the command if you run into memory issues

> ``` gatk --java-options "-Xmx4G" HaplotypeCaller \ ``` <br>
> ``` -R /gatk/data/ref/ref.fasta -I /gatk/data/bams/mother.bam \ ``` <br>
> ```-O /gatk/data/sandbox/variants.vcf ```


### Run GVCF workflow tools using HaplotypeCaller, GenomicsDBImport and then GenotypeGVCFs to perform joint calling on multiple input samples.

### Run HaplotypeCaller on three input bams (mother, father, son)

``` gatk HaplotypeCaller -R /gatk/data/ref/ref.fasta -I /gatk/data/bams/mother.bam -O /gatk/data/sandbox/mother.g.vcf -ERC GVCF ```

``` gatk HaplotypeCaller -R /gatk/data/ref/ref.fasta -I /gatk/data/bams/father.bam -O /gatk/data/sandbox/father.g.vcf -ERC GVCF ```

```gatk HaplotypeCaller -R /gatk/data/ref/ref.fasta -I /gatk/data/bams/son.bam -O /gatk/data/sandbox/son.g.vcf -ERC GVCF ```

### Run GenomicsDBImport on three GVCFs to consolidate

``` gatk GenomicsDBImport -V /gatk/data/sandbox/mother.g.vcf \ ``` <br>
``` -V /gatk/data/sandbox/father.g.vcf \ ``` <br>
``` -V /gatk/data/sandbox/son.g.vcf --genomicsdb-workspace-path \ ``` <br>
```//data/sandbox/trio.gdb_workspace --intervals 20 ```

### Alternatively, use CombinedGVCFs command as an alternative to GenomicsDBImport

```gatk CombineGVCFs -R /gatk/data/ref/ref.fasta \  ``` <br>
```-V /gatk/data/sandbox/father.g.vcf \  ``` <br>
```-V /gatk/data/sandbox/mother.g.vcf  -V /gatk/data/sandbox/son.g.vcf \ ``` <br>
``` -O /gatk/data/sandbox/combine_trio_variants.vcf ```

### Run GenotypeGVCFs on the GDB workspace to produce final multisample VCF

``` gatk GenotypeGVCFs -R /gatk/data/ref/ref.fasta \   ``` <br>
``` -V gendb://data/sandbox/trio.gdb_workspace \  ``` <br>
```-G StandardAnnotation -O /gatk/data/sandbox/trio_variants.vcf ``` <br>


### delete all containers

```sudo docker rm `sudo docker ps --no-trunc -aq` ```

```docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs docker rm```

### Delete stopped containers

```docker rm -v $(docker ps -a -q -f status=exited)```

### From within the container’s command prompt, detach and return to the host’s prompt.

```ctrl+p then ctrl+q```

### Getting Docker Container's IP Address from host machine:

```docker inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)```

### Useful to know
> ***docker logs*** - gets logs from container <br>
> ***docker events*** - gets events from container <br>
> ***docker port*** - shows public facing port of container <br>
> ***docker top*** - shows running processes in container <br>
> ***docker stats*** - shows containers' resource usage statistics <br>
> ***docker diff*** - shows changed files in the container's FS

### add user to docker group  and type ID /check UID
 ```sudo usermod -aG docker $user```
