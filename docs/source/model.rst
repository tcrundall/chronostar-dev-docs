Model
=====

Ideally extends the `BaseMixture class from sklearn <https://github.com/scikit-learn/scikit-learn/blob/36958fb240fbe435673a9e3c52e769f01f36bec0/sklearn/mixture/_base.py#L483>`_.

.. note::
   The BaseMixture class expects the input data to be a single array of shape ``(nsamples, nfeatures)``. This therefore doesn't allow for the pre-construction of star covariance matrices. The typical usage also doesn't expect uncertainties in the data, however this is customizable.

inputs

- config params
- data
- [mpi pool]

injected dependencies

- Component Class
- Background Class
- Loss function

key public methods:

- fit
- bic

key private methods:

- is_converged
- e_step
- m_step

outputs

- best fit of given model


