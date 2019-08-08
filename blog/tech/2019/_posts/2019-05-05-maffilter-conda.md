---
layout: post
title: Install MafFilter in Conda environment
tags: conda how-to sequence-analysis alignment software
---

# Note 2019-07
Install did not succeed on another system. Error message was dubious...

# Setup Conda environment

Specify a Conda env as follows:

```yaml
name: maffilter
channels:
  - bioconda
  - conda-forge
dependencies:
  - Python=3.6.*
  - boost=1.67.0
  - cmake=3.14.0
```

Short-hand for path to this environemnt: `ENV-PATH`,
e.g., `$HOME/miniconda/envs/maffilter`

# Install dependencies

Activate the Conda environment, and create a folder as destination to clone several
git repositories, e.g., `ENV-PATH/source_pkg`. Clone the following repositories into
this folder:
- https://github.com/BioPP/bpp-core
- https://github.com/BioPP/bpp-seq
- https://github.com/BioPP/bpp-phyl
- https://github.com/BioPP/bpp-popgen
- https://github.com/BioPP/bpp-seq-omics
- https://github.com/BioPP/bpp-phyl-omics

For each of these packages and in the order as listed above, do the following:

1. `cd` into folder with cloned repository
2. `mkdir build && cd build`
3. `cmake -DCMAKE_INSTALL_PREFIX=ENV-PATH`
4. `make && make install`

Source: http://biopp.univ-montp2.fr/wiki/index.php/Installation

# Set environment variables

Inside the Conda environment (everything described here should happen within the
active Conda environment), set the following environment variables:

1. `export CPATH=$CPATH:ENV-PATH/include`
2. `export LIBRARY_PATH=$LIBRARY_PATH:ENV-PATH/lib`
3. `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:ENV-PATH/lib`

Source: http://biopp.univ-montp2.fr/wiki/index.php/Installation

# Install maffilter

1. Clone maffilter git github.com/jydu/maffilter into `ENV-PATH/source_pkg`
2. `cd maffilter && mkdir build && cd build`
3. `cmake -DCMAKE_INSTALL_PREFIX=ENV-PATH`
4. `make && make install`

Afterwards, the maffilter executable should be located in `ENV-PATH/bin`.