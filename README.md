# Conda Environment for Science Pipelines

This repository contains the definition of the Conda environment used by the LSST Science Pipelines.


## Contents

There are two core types of files in the `etc` directory.

- `bleed` files, in the pattern of `conda3_bleed-<platform>-64.yml`, indicating the names of packages on which the 
  Science Pipelines directly depend, or
- `lock` files, in the pattern of `conda-<platform>-64.lock`, indicating a specific versioned set of those packages, 
  and packages upon which they depend, which can be directly instantiated as a conda environment;

Where `<platform>` is one of:

- `linux`, indicating that this file has been tested on CentOS (our reference platform), and, by extension, is appropriate for use on a Linux systems;
- `osx`, indicating that this file has been tested on macOS.

### Lock files

The `lock` files describe an explicit export of conda dependencies, generated with `conda list --explicit`.
Creating an environment based on `lock` files will bypass the conda solver, which has several advantages:

1. Installing the environment is faster as the solver does not run.
1. The environment is always installable, even if conda-forge packages are marked as broken, or hidden through a 
   patched repository metadata file (`repodata.json`), which the solver would normally try to resolve.

## Update Procedures

Note that these procedures should be carried out on the platform being targeted.
That is, generating a `linux` package list should be carried out on a Linux system.

### Generating a complete new environment from the “bleed” files

This procedure will generate a completely new `<packages>` which satisfies the dependencies specified in the `<bleed>` file.
This will likely cause significant changes to the environment, and should normally be preceded by an RFC.

1. Ensure your conda is up-to-date
1. Create the conda environment from bleed: `conda env update --name pinned_<date> --file conda3_bleed-<platform>-64.yml`
   * Use YYYYMMDD format for `<date>`
   * Use `osx` or `linux` for `<platform>`
1. Activate the environment: `source activate pinned_<date>`
1. Check that the list of packages in the environment are as expected
1. Export the environment, over-writing the previous `lock` file:
   `conda list --explicit > conda-<platform>-64.lock`
1. Update the `requirements.txt` file in the [linting repository](https://github.com/lsst/linting) to match the version numbers specified in the new conda environment.

### Adding a new package to the environment

To add a new package without modifying the existing environment,
follow this procedure. Please note that the existing environment must
be solvable, otherwise `conda install` may fail. If it is not
solvable, follow the previous procedure for completing a new
environment from the bleed file.

1. Add the name of the new package to install to the `bleed` file. Try
   to choose an appropriate section, if possible.
1. Instantiate the environment based on a `lock` file: `conda create --name pinned_<date> --file conda-<platform>-64.lock`
   * Use YYYYMMDD format for `<date>`
   * Use `osx` or `linux` for `<platform>`
1. Activate the environment: `source activate pinned_<date>`
1. Install the package: `conda install --no-update-deps <packagename>`
1. Export the environment, over-writing the previous `lock` file: `conda list --explicit > conda-<platform>-64.lock`
1. Update the `requirements.txt` file in the [linting repository](https://github.com/lsst/linting) to match the version numbers specified in the new conda environment.

## Conda Channels
We use the [`conda-forge` channel](https://conda-forge.org/) as our primary distribution source for packages.

## Historical note

June 2020: We previously used environment export (`packages` files) and allowed the use of pip
packages in the `bleed` file. We moved to using `lock` files and no longer
support pip packages in the `bleed` files to ensure packages are
installable in the long term.

April 2020: We switched to using conda-forge as the primary channel
for packages.

This material was moved from the [lsstsw](https://github.com/lsst/lsstsw) package in December 2018.
