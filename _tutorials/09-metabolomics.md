---
title: Containerised metabolomics application
---
## Learning objectives
- Learn how to use docker environment for processing Metabolomics dockers
- how to check tool help
- Try couple of processing steps


# Tutorial help Here
https://github.com/computational-metabolomics/dimspy/blob/master/examples/run.sh

In general when you look for related tools, you may find several relavent tools for your needs.
It may take sometime to get used to the tools.
Read the documentation of dimspy and get the overall idea of tools.

```

dimspy --help

```
what are the different processing steps that dimspy can perform on metabolomics data?

Once you have got some idea. You can now download some tutorial data here
```
wget https://github.com/computational-metabolomics/dimspy-galaxy/raw/master/tools/dimspy/test-data/MTBLS79_mzml_triplicates.zip
```


you can now unzip the mzxml files using the following commands

```
dimspy unzip \
--input ../tests/data/MTBLS79_subset/MTBLS79_mzml_triplicates.zip \
--output results/mzml
```

check how many files are there?

Now you can  run the step process-scans  using the following command:

```
dimspy process-scans \
--input results/mzml \
--output results/peaklists.hdf5 \
--filelist ../tests/data/MTBLS79_subset/filelist_mzml_triplicates.txt \
--function-noise median \
--snr-threshold 3.0 \
--ppm 2.0 \
--min_scans 1 \
--min-fraction 0.5 \
--block-size 5000 \
--ncpus 2

```


Report the output files you have got from this step?



##############################################################
source: https://hub.docker.com/r/workflow4metabolomics/galaxy-workflow4metabolomics/dockerfile
https://github.com/yufree/xcmsrocker
https://pypi.org/project/metaMS/
https://hub.docker.com/r/biocontainers/xcms/
https://github.com/pietrofranceschi/metabolomics_course
https://hub.docker.com/r/pietrofranceschi/metabolomics_course/tags

## learning objectives
- How to name a biocontainer
- How to add some options
- Running container in different ports
- Building a container from github

## Video lecture about this content


## instructions for starting container

- Install Docker
- Pull the Rocker image docker pull yufree/xcmsrocker:latest
- Use docker run -e PASSWORD=xcmsrocker -p 8787:8787 yufree/xcmsrockerto start the image
- Open the browser and visit http://localhost:8787or http://[your-ip-address]:8787 to power on RStudio server
- Default user name is rstudioand password is xcmsrocker
- Enjoy your data analysis!

## actual analysis inside the biocontainer
