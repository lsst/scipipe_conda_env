Conda Environment for Science Pipelines
=======================================

This repository contains the definition of the conda environment used by the science pipelines.

It has been moved out from lsstsw package in December 2018 (git clone --bare and git push --mirror).

Procedure
---------

In order to regerate the pinned files from the bleed ones, following procedure has to be followed:

1. ensure your conda is up to date to version *4.6.8*
1. make a copy of the bleed txt file to a yaml file: `copy conda3_bleed-osx-64.txt conda3_bleed-osx-64.yml`
1. edit the yml file and add at the beggining: `name: bleed`
1. create the conda environment from bleed: `conda env create --file conda3_bleed-osx-64.yml`
1. activate the _bleed_ environment: `source activate bleed`
1. check that the list of packages in the environment are as expected
1. export the environment and override the previous pinned file: `conda env export > conda3_packages-osx-64.txt`
1. edit the *conda3_packages-osx-64.txt* file and remove the first line (name) and the last line (location)

