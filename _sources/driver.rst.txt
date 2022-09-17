Driver
======
Manage entire process of fitting a model to data.

Responsibilities include:

- Parse config file
- Initialise modules
- Handle mpi pool
- set up logging
- halt execution upon convergence signal
- (( fit background?? ))

inputs:

- clean data table
- config file
- opt: mpi pool

injected dependencies:

- Initial Component Set Generator
- Model Fitter
- Component Model
- (( Background Model ))

outputs:

- best fit components (w/ uncertainties)
- membership probabilities
- live log
- opt: progress “snapshots” written to file for restarts
- opt: progressive results written to file

