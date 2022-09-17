Uncertainty Finder
==================

Takes a best fit model and finds the uncertainty of the best fitting parameters. This can be expensive, as it is only performed at the very end of the entire run.

inputs

- config params
- data
- best fitting component set
- [mpi pool]

injected dependencies

- Component Class
- Background Class
- Loss function

key public methods

- explore_parameter_space

outputs

- best fit with uncertainties

