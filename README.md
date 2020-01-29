# `fgc_crispr_pipeline_UAT`

## Summary

Tools for conducting User Acceptance Testing (UAT) for the CRUK deployment of the FGC CRISPR analysis pipeline.

## How to setup an AWS Linux VM to run UAT analyses

### Data
Once a Linux VM is set up and working (minimum 100 GB storage), it will be necessary to sync the contents of the UAT S3 pipeline output data bucket into a local dircetory (the AWS CLI will already be installed):

* Set up AWS credentials (key, secret key, eu-west-1 [region]): `aws configure`
* Sync the v1 pipeline output data from S3 to a local path: `aws sync s3://fgc-pipeline-uat-output-v1 .`
* Sync the v2 pipeline output data: `aws sync s3://fgc-pipeline-uat-output-v2-20200127 .`

### R analysis
The analysis is contained in an R markdown document (`.Rmd`) in the `analysis` sub-directory. When run using `Rscript`, this will produce an `html` output containing the UAT results. To produce this, you must install `pandoc` via `conda`:

* `wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`
* `source Miniconda3-latest-Linux-x86_64.sh`
* `conda install pandoc`
* `conda install r-base`
* `conda install r-tidyverse`
* `conda install r-rmarkdown`

## Usage

To save `UAT-results.html` to the current working directory (note input and output data paths provided to `render` via `params` argument):

`Rscript -e 'Sys.setenv(RSTUDIO_PANDOC="/home/ec2-user/miniconda2/bin/pandoc"); library(rmarkdown); rmarkdown::render("/path/to/fgc_crispr_pipeline_UAT/analysis/UAT-analysis.Rmd", output_file="UAT-results.html", params = list(output_v1 = "/path/to/output_v1", output_v2 = "/path/to/output_v2"))`

