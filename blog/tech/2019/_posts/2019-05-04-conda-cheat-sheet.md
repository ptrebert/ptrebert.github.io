---
layout: post
title: Conda Cheat Sheet
tags: python conda reproducibility
---

Sources:
- [docs.conda.io](https://docs.conda.io)
- [bioconda.github.io](https://bioconda.github.io/)

# Conda's purpose
> Package, dependency and environment management for any language

Create software environments for development and execution in a reproducible way.

For the smallest footprint, install `Miniconda`,
which is just the Conda package manager and Python:

[docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

# How to...

## Define Conda environments
Create a YAML file, e.g., `my-dev-env.yml` as follows:

```yaml
name: my-dev-env
channels:
  - default
  - conda-forge
  - bioconda
  - r
dependencies:
  - Python=3.6.*
  - r-base=3.5.1
  - htslib>=1.8
```
(`default` channel listed to be explicit, can be omitted)

## Create Conda environments

`conda env create -f <FILENAME>`

`conda env create -f my-dev-env.yml`

## (De-)activate Conda environment

`conda activate <ENV-NAME>`

`conda activate my-dev-env`

`conda deactivate my-dev-env`

## Search for packages

`conda search <PKG-NAME>` searches whatever channels are 
configured (at least, that is the `default` channel)

`conda search --channel <CHANNEL-NAME> <PKG-NAME>` searches the package
also in the specified channel

## Add channels

`conda config --add channels <CHANNEL-NAME>`

## Update a Conda environment
Same as: install new packages into environment.
Update the YAML file, e.g.,

```yaml
name: my-dev-env
channels:
  - default
  - conda-forge
  - bioconda
  - r
dependencies:
  - Python=3.6.*
  - r-base=3.5.1
  - htslib>=1.8
  - bioconductor-genomicranges=1.32.7
```

Update the environment: `conda env update -f my-dev-env.yml`

## Remove an environment
`conda env remove --name <ENV-NAME>`

## Remove package from environment
Uninstall: `conda remove --name <ENV-NAME> <PKG-NAME>`

## Install packages via pip
Specify install command in the YAML file as follows:

```yaml
name: my-dev-env
channels:
  - default
  - conda-forge
  - bioconda
  - r
dependencies:
  - Python=3.6.*
  - r-base=3.5.1
  - htslib>=1.8
  - bioconductor-genomicranges=1.32.7
  - pip:
      - intervaltree  # regular package from PyPi
      - git+https://bitbucket.org/whatshap/whatshap  # package from git repo
```

For the git repo case, this installs the master branch. To install a specific branch, change as follows:

```yaml
name: my-dev-env
channels:
  - default
  - conda-forge
  - bioconda
  - r
dependencies:
  - Python=3.6.*
  - r-base=3.5.1
  - htslib>=1.8
  - bioconductor-genomicranges=1.32.7
  - pip:
      - intervaltree
      - git+https://bitbucket.org/whatshap/whatshap@dev-feat-haplotag-region
```