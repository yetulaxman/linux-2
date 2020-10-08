### Deepvariant pipeline as developed by GoogleAI ###
Run [deepvariant method](https://github.com/google/deepvariant)   to perform variant calling on WGS and WES data sets in Puhti supercomputing environment using deepvariant singularity container


## How to get started
One needs the DeepVariant programs ((https://github.com/google/deepvariant) and some test data. One can download data from Pouta object storage  (or from google bucket)  Storage.

### Prepare deepvariant singulairty image from docker image
Deepvariant (this example is with version 0.8) uses docker to run the binaries instead of copying binaries to local machines first.

One needs get deepvariant docker image, models and test data in order to run the pipeline. Additionally, other prerequisites for running deepvariant method includes 1) obtaining A reference genome in [FASTA](https://en.wikipedia.org/wiki/FASTA_format) format and its corresponding index file (.fai). 2) An aligned reads file in [BAM](http://genome.sph.umich.edu/wiki/BAM) format and its corresponding index file (.bai).

### Download deepvariant docker image

```
sudo docker pull gcr.io/deepvariant-docker/deepvariant:0.8.0
```

#### _Convert the image to singularity image on your own ( or download singularity image and test data below and chnage the file paths appropriately )_ ####

```
wget https://object.pouta.csc.fi/pilot_projects/Deepvariant_singulairty.zip

or

sudo docker tag gcr.io/deepvariant-docker/deepvariant:0.8.0 localhost:5000/deepvariant:latest
sudo docker run -d -p 5000:5000 --restart=always --name registry registry:2
sudo docker push localhost:5000/deepvariant:latest
SINGULARITY_NOHTTPS=1 singularity build deepvariant.simg docker://localhost:5000/deepvariant:latest

```

### Prepare slurm scripts to run on Puhti (e.g., deepvariant_puhti.sh)

```
#!/bin/bash
#SBATCH --time=00:05:00
#SBATCH --partition=test
#SBATCH --account=project_xxx

export TMPDIR=$PWD

singularity -s exec -B /users/$USER/Puhti:/users/$USER/Puhti  \
deepvariant.simg \
/opt/deepvariant/bin/run_deepvariant \
--model_type=WGS   --ref=/users/$USER/Puhti/testdata/ucsc.hg19.chr20.unittest.fasta \
--reads=/users/$USER/Puhti/testdata/NA12878_S1.chr20.10_10p1mb.bam \
--regions "chr20:10,000,000-10,010,000"   --output_vcf=output.vcf.gz  \
--output_gvcf=output.g.vcf.gz
```

### Submit the job using sbatch command

```
sbatch  deepvariant_puhti.sh
```

Please **note** that one can use gpu version of deepvariant with the following sbatch script

```
#!/bin/bash
#SBATCH --time=00:05:00
#SBATCH --partition=gputest
#SBATCH --gres=gpu:v100:1
#SBATCH --account=project_xxx

export TMPDIR=$PWD

singularity -s exec --nv -B /users/$USER/Puhti_gpu:/users/$USER/Puhti_gpu \
deepvariant_gpu.simg   /opt/deepvariant/bin/run_deepvariant \
--model_type=WGS   --ref=/users/$USER/Puhti_gpu/testdata/ucsc.hg19.chr20.unittest.fasta \
--reads=/users/$USER/Puhti_gpu/testdata/NA12878_S1.chr20.10_10p1mb.bam  \
--regions "chr20:10,000,000-10,010,000"   --output_vcf=output.vcf.gz  \
--output_gvcf=output.g.vcf.gz
```
