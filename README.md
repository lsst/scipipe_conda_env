# Conda Environment for Science Pipelines

This repository contains the definition of the Conda environment used by the LSST Science Pipelines.

## Contents

Files in the `etc` directory are named following the pattern:

```
conda3_<filetype>-<platform>-64.yml
```

Where `<filetype>` is one of:

- `bleed`, indicating the names of packages on which the Science Pipelines directly depend, or
- `packages`, indicating a specific versioned set of those packages, and packages upon which they depend, which can be directly instantiated as a Conda environment.

And `<platform>` is one of:

- `linux`, indicating that this file has been tested on CentOS (our reference platform), and, by extension, is appropriate for use on a Linux systems;
- `osx`, indicating that this file has been tested on macOS.

## Procedures

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
1. Export the environment, over-writing the previous `packages` file: `conda env export > conda3_packages-<ARCH>-64.yml`

### Adding a new package to the environment

1. Add the name of the new package to install to the `bleed` file.
1. Instantiate the environment based on a `package` file: `conda env update --name pinned_<date> --file conda3_packages-<platform>-64.yml`
   * Use YYYYMMDD format for `<date>`
   * Use `osx` or `linux` for `<platform>`
1. Activate the environment: `source activate pinned_<date>`
1. Install the package: `conda install --no-update-deps <packagename>`
1. Export the environment, over-writing the previous `packages` file: `conda env export > conda3_packages-<ARCH>-64.yml`

## Historical note

This material was moved from the [lsstsw](https://github.com/lsst/lsstsw) package in December 2018.
