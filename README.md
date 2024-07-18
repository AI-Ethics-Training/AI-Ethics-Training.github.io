MLOS DevContainer MLOS Linux MLOS Windows Code Coverage Status

MLOS is a project to enable autotuning for systems.

Contents
MLOS
Contents
Overview
Organization
Contributing
Getting Started
conda activation
Usage Examples
mlos-core
mlos-bench
mlos-viz
Installation
See Also
Examples
Overview
MLOS currently focuses on an offline tuning approach, though we intend to add online tuning in the future.

To accomplish this, the general flow involves

Running a workload (i.e., benchmark) against a system (e.g., a database, web server, or key-value store).
Retrieving the results of that benchmark, and perhaps some other metrics from the system.
Feed that data to an optimizer (e.g., using Bayesian Optimization or other techniques).
Obtain a new suggested config to try from the optimizer.
Apply that configuration to the target system.
Repeat until either the exploration budget is consumed or the configurations' performance appear to have converged.
optimization loop

Source: LlamaTune: VLDB 2022

For a brief overview of some of the features and capabilities of MLOS, please see the following video:

demo video

Organization
To do this this repo provides two Python modules, which can be used independently or in combination:

mlos-bench provides a framework to help automate running benchmarks as described above.

mlos-viz provides some simple APIs to help automate visualizing the results of benchmark experiments and their trials.

It provides a simple plot(experiment_data) API, where experiment_data is obtained from the mlos_bench.storage module.

mlos-core provides an abstraction around existing optimization frameworks (e.g., FLAML, SMAC, etc.)

It is intended to provide a simple, easy to consume (e.g. via pip), with low dependencies abstraction to

describe a space of context, parameters, their ranges, constraints, etc. and result objectives
an "optimizer" service abstraction (e.g. register() and suggest()) so we can easily swap out different implementations methods of searching (e.g. random, BO, LLM, etc.)
provide some helpers for automating optimization experiment runner loops and data collection
For these design requirements we intend to reuse as much from existing OSS libraries as possible and layer policies and optimizations specifically geared towards autotuning systems over top.

By providing wrappers we aim to also allow more easily experimenting with replacing underlying optimizer components as new techniques become available or seem to be a better match for certain systems.

Contributing
See CONTRIBUTING.md for details on development environment and contributing.

Getting Started
The development environment for MLOS uses conda and devcontainers to ease dependency management, but not all these libraries are required for deployment.

For instructions on setting up the development environment please try one of the following options:

see CONTRIBUTING.md for details on setting up a local development environment
launch this repository (or your fork) in a codespace, or
have a look at one of the autotuning example repositories like sqlite-autotuning to kick the tires in a codespace in your browser immediately :)
conda activation
Create the mlos Conda environment.

conda env create -f conda-envs/mlos.yml
See the conda-envs/ directory for additional conda environment files, including those used for Windows (e.g. mlos-windows.yml).

or

# This will also ensure the environment is update to date using "conda env update -f conda-envs/mlos.yml"
make conda-env
Note: the latter expects a *nix environment.

Initialize the shell environment.

conda activate mlos
Usage Examples
mlos-core
For an example of using the mlos_core optimizer APIs run the BayesianOptimization.ipynb notebook.

mlos-bench
For an example of using the mlos_bench tool to run an experiment, see the mlos_bench Quickstart README.

Here's a quick summary:

./scripts/generate-azure-credentials-config > global_config_azure.jsonc

# run a simple experiment
mlos_bench --config ./mlos_bench/mlos_bench/config/cli/azure-redis-1shot.jsonc
See Also:

mlos_bench/README.md for more details on this example.
mlos_bench/config for additional configuration details.
sqlite-autotuning for a complete external example of using MLOS to tune sqlite.
mlos-viz
For a simple example of using the mlos_viz module to visualize the results of an experiment, see the sqlite-autotuning repository, especially the mlos_demo_sqlite_teachers.ipynb notebook.

Installation
The MLOS modules are published to pypi when new releases are tagged:

mlos-core
mlos-bench
mlos-viz
To install the latest release, simply run:

# this will install just the optimizer component with SMAC support:
pip install -U mlos-core[smac]

# this will install just the optimizer component with flaml support:
pip install -U "mlos-core[flaml]"

# this will install just the optimizer component with smac and flaml support:
pip install -U "mlos-core[smac,flaml]"

# this will install both the flaml optimizer and the experiment runner with azure support:
pip install -U "mlos-bench[flaml,azure]"

# this will install both the smac optimizer and the experiment runner with ssh support:
pip install -U "mlos-bench[smac,ssh]"

# this will install the postgres storage backend for mlos-bench
# and mlos-viz for visualizing results:
pip install -U "mlos-bench[postgres]" mlos-viz
Details on using a local version from git are available in CONTRIBUTING.md.

See Also
API and Examples Documentation: https://microsoft.github.io/MLOS
Source Code Repository: https://github.com/microsoft/MLOS
Examples
mlos-autotuning-template repository

sqlite-autotuning

Working example of tuning sqlite with MLOS.

These can be used as starting points for new autotuning projects.
