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
Uninstall package: `conda remove --name <ENV-NAME> <PKG-NAME>`

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

## Export Conda environment specification

Why could this be important? Specifying ("pinning") package versions prevents a change/update
of said package, BUT other packages/libraries not specifically listed in the YAML file can
nevertheless be changed during `conda env update`. In the worst case, this may break something.

As soon as the work performed in the Conda environment reaches a certain level of
maturity (e.g., dependencies no longer change), one can export a more comprehensive
specification of the respective Conda environment via
`conda env export --name ENV-NAME > my-dev-env.yml`. Obviously, this will overwrite
`my-dev-env.yml`.

## Setting environment variables

In the top-level directory of the Conda environment (`/envs/my-dev-env/`),
create the following files and folders:

```text
mkdir -p ./etc/conda/activate.d
mkdir -p ./etc/conda/deactivate.d
touch ./etc/conda/activate.d/env_vars.sh
touch ./etc/conda/deactivate.d/env_vars.sh
```

Edit `./etc/conda/activate.d/env_vars.sh` as follows:

```text
#!/bin/sh

export VAR1=FOO
export VAR2=BAR
```

Edit `./etc/conda/deactivate.d/env_vars.sh` as follows:

```text
#!/bin/sh

unset VAR1
unset VAR2
```

NB: Preferred way for handling stuff such as `LD_LIBRARY_PATH` etc.