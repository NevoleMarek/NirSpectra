# Overview

This directory contains the nirspectra package.


First, individual scripts are described. Second the process how to use them (pipeline) is described in section 
[Pipelines](nirspectra/README.adoc#user-content-pipelines). 

## Scripts

### script.py
Script does cool stuff. 

Example command:
```
bin/docker-run poetry run python nirspectra/script.py \
  param1 \
  param2 \
  --param3 42
```

Documentation:

Fill with help of your script. E.g. output of `bin/docker-run poetry run python nirspectra/script.py --help`


## Pipelines

### Training a classifier
For training model data has to be downloaded, preprocessed and then the model can be trained.
Run scripts in the following order:

1. Download data

    `bin/docker-run poetry run python nirspectra/download_data.py`

2. Preprocess data

    `bin/docker-run poetry run python nirspectra/preprocess_data.py`

3. Train model
    
    `bin/docker-run poetry run python nirspectra/train_model.py`
