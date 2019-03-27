Conda Environment for Science Pipelines
=======================================

This repository contains the definition of the conda environment used by the science pipelines.

It has been moved out from lsstsw package in December 2018 (git clone --bare and git push --mirror).

Procedure
---------

In order to regerate the pinned files from the bleed ones, following procedure has to be followed:

1. ensure your conda is up to date to version *4.6.8*
1. create the conda environment from bleed: `conda env update --name pinned_<date> --file conda3_bleed-<ARCH>-64.yml`
   * Use YYYYMMDD format for _\<date\>_
   * _\<ARCH\>_ is `osx` or `linux` 
1. activate the *pinned_<date>* environment: `source activate pinned_<date>`
1. check that the list of packages in the environment are as expected
1. export the environment and override the previous pinned file: `conda env export > conda3_packages-<ARCH>-64.yml`

