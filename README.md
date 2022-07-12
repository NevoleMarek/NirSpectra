# nirspectra

TODO: Short description of the project.

## Motivation and goals

TODO

## Table of contents

1. [Repository structure](README.md#repository-structure)
2. [Getting started](README.md#getting-started)
3. [Data sources](README.md#data-sources)
4. [Experiments description](README.md#experiments-description)
5. [Results](README.md#results)


## Project related documents

TODO: Fill links (Google Drive or Confluence pages)

- [Project folder on Google drive](https://drive.google.com/{TODO})
- [Project definition](https://drive.google.com/{TODO})
- [Data analysis report](https://drive.google.com/{TODO})
- [Experiment evaluation report](https://drive.google.com/{TODO})
- [Conceptual design](https://drive.google.com/{TODO})

## Repository structure

The structure of the repository is as follows:

```
.
├── bin
├── data
├── docs
├── notebooks
├── results
├── nirspectra
└── .github/workflows
```

- ```bin``` - A directory containing command-line scripts.
- ```data``` - A directory containing data. Data are not uploaded into the repository.
- ```docs``` - A directory containing additional documentation files.
- ```notebooks``` - A directory containing jupyter notebooks with experiments and reports. For details see [README](notebooks/README.md).
- ```results``` - A directory containing where a data scientist should store results. Results are usually stored in Google drive.
- ```nirspectra``` - The python package that contains all necessary functions such as accessing data, manipulations or model training. More details can be found in a [dedicated README](nirspectra/README.adoc).
- ```.github/workflows``` - A directory with Github CI workflows definitions


## Getting started

Open a new terminal window in your [personal JupyterLab workspace](https://datamole.atlassian.net/wiki/spaces/DS/pages/442663031/Tutorial) and run the following commands:

    git clone {github-repo-ssh-url}
    cd nirspectra
    bin/docker-run-interactive poetry install
    jupyter-register-kernel-poetry nirspectra_some_suffix nirspectra


After reloading the JupyterLab web UI, you can work on the experiments.

## Data sources

TODO: Where to find the data, what format are they in, what columns are there, who to ask for permissions to access the data, etc.
Please add a link to the Data sources artefact as well.

## Experiments description

TODO: Briefly describe steps of the experiment.
Please add a link to the Experiment conceptual design diagram artefact as well.

## Results

TODO: Either describe the results here or add a link to a report on Google Drive / Github / Confluence.
