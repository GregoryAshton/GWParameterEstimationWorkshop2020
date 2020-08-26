# Getting started with bilby_pipe

This tutorial will go through using `bilby_pipe`, a tool for running analyses
at scale. `bilby_pipe` is not as flexible as `bilby`, but intended to do common
tasks.

## Installation

* `bilby_pipe` is intended for use on either HTCondor or slurm clusters. You
can also use it on a local machine (running jobs serially). If you haven't
already, follow the [installation steps](https://github.com/GregoryAshton/GWParameterEstimationWorkshop2020/blob/master/pages/installation.md) to make sure you have `bilby_pipe`
installed.
* To check it is installed, run
```bash
$ bilby_pipe --version
bilby_pipe=1.0.0 (CLEAN) ....
```

## Dependencies required for remote data access
* `bilby_pipe` uses the `gwpy`, `NDS2`, and `LDASTools-frameCPP` libraries to
  access LIGO-Virgo-KAGRA data. To get full support, these need to be installed
via conda
```bash
$ conda install -c conda-forge python-ldas-tools-framecpp python-nds2-client
```
If you don't install these, remote data access (i.e. not on a LIGO Data Grid
cluster) will be difficult.

## Basics

`bilby_pipe` operates through an `ini` file (also known as a configuration file). The `ini` file specifies **what** to run and **how**. To see all the available options for the `ini` file, you can run

```bash
$ bilby_pipe --help
```

You can also view this output [in the docs](https://lscsoft.docs.ligo.org/bilby_pipe/master/user-interface.html#bilby-pipe-help).

| :rocket: Task: run the `--help` command and check it produces output |
| --- |
