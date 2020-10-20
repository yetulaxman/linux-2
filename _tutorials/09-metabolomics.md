---
title: Containerised Metabolomics Application
---
## Learning objectives
On completion of this session, you will learn:
- How to check an unknown tool for help
- To use a docker container for processing Metabolomics data
- Couple of processing steps in metabolomics


# Tutorial with dimspy 

In general when you look for related tools, you may find several relavent tools for your needs. It may take sometime to get used to the tools. Read the documentation of dimspy and get the overall idea of tools.

```

dimspy --help

```
what are the different processing steps that dimspy can perform on metabolomics data?

Once you have got some idea about different processing steps.

You can now download some tutorial data  from here:

```
wget https://github.com/computational-metabolomics/dimspy-galaxy/raw/master/tools/dimspy/test-data/MTBLS79_mzml_triplicates.zip
```

you can now unzip the mzxml files using the following command from dimspy software:

```
dimspy unzip \
--input ../tests/data/MTBLS79_subset/MTBLS79_mzml_triplicates.zip \
--output results/mzml
```

Check how many files are there?

Now you can  run the *process-scans* step using the following command:

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


*Exercise if time permits* : 

1. Perorm a metabolomics data analysis using XCMS software using docker container here: https://hub.docker.com/r/pietrofranceschi/metabolomics_course
