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

## The ini file

An ini file consists of `key=value` pairs, a complete guide can be found [in the docs](https://lscsoft.docs.ligo.org/bilby_pipe/master/ini_file.html), here we will go through an example for a simulated signal.

### Ini file example
```
# The accounting tag, onnly needed on LDG clusters.
# See https://ldas-gridmon.ligo.caltech.edu/accounting/condor_groups/determine_condor_account_group.html
# for help with determining what tag to use
accounting = FILL_THIS_IN

# A label to help us remember what the job was for
label = bbh_injection

# The directory to store results in
outdir = outdir_bbh_injection

# Which detectors to use, option: H1, L1, V1
detectors = [H1, L1]

# The duration of data to analyse in seconds
duration = 4

# The sampler
sampler = dynesty

# The options to pass to the sampler
sampler-kwargs = {'nlive': 1000}

# The prior file to use
prior-file = bbh_simple_example.prior

# We want to inject a signal (in the case, drawn randomly from the prior)
injection = True

# We want to use Gaussian noise (default is to simulate it from O4-design sensitivity curves) 
gaussian-noise = True

# We'll do just one simulation
n-simulation = 1

# We'll run one "parallel" job. This runs n-parallel *identical* jobs and then combines the results together into a single combined run
n-parallel = 1
```
| :rocket: Task: save the contents of the ini file above in a file `bbh_simple_example.ini` |
| --- |

### Prior file
Note, in the ini file, we specified `prior-file = bbh_simple_example.prior`, this is pointing to a file (you can specify and absolute or relative path as needed).
Here are the contents of that file, as you can see, this is a simple prior in only the distance and inclination
```
mass_1 = 50.0
mass_2 = 45.0
a_1 = 0.0
a_2 = 0.0
tilt_1 = 0.0
tilt_2 = 0.0
phi_12 = 0.0
phi_jl = 0.0
luminosity_distance =  bilby.gw.prior.UniformComovingVolume(name='luminosity_distance', minimum=1e2, maximum=5e3, unit='Mpc')
dec = -0.2
ra = 1.4
theta_jn = Sine(name='theta_jn')
psi = 0.0
phase = 0.0
geocent_time = 0.0
```

| :rocket: Task: save the contents of the ini file above in a file `bbh_simple_example.prior`. Note, this **must** match the filename in the ini file |
| --- |
