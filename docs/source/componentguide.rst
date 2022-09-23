===============
Component Guide
===============

Ideally extends the 
`BaseMixture class from sklearn <https://github.com/scikit-learn/scikit-learn/blob/36958fb240fbe435673a9e3c52e769f01f36bec0/sklearn/mixture/_base.py>`_.

I predict there will only need to be one implementation of this class, as the feature and fitting customisation will be done elsewhere (i.e. in :class:`Component` or :class:`BaseICPool`).

If we ignore data uncertainties, the age is the only free parameter in the maximisation step. To explain this further, consider fixing the age at 0. In this case we could use `sklearn.mixture.GaussianMixture` out of the box. There is no parameter exploration needed, because the maximisation step calculates the mean and covariance from the "assigned members" (a.k.a. samples of responsibility(?)).

`AgedGaussianMixture` introduces a free age parameter for each of its components. The key remaining **dependent** parameters of a component are :math:`\mu_0` and :math:`\Sigma_0`. :math:`\mu_0` is constrained completely by the age and data, since we just project :math:`\mu` back in time by :math:`t`. :math:`\Sigma_0` is less apparent, but with some assumptions can be equally constrained. We get :math:`\Sigma` from the assigned members. We project this back in time by :math:`t` using a Jacobian. We now have a :math:`~\Sigma_0`. The final step is to impose the core assumptions of a Component - that there is no positional-velocity correlation - and impose assumptions specific to the chosen component class. E.g. for the :class:`SphereComponent` we assume spherical in position space and spherical in velocity space. Therefore in this case we can derive :math:`\Sigma_0` by finding the total space-volume of :math:`~\Sigma_0` and inferring the required radius of an equal volume sphere in position space. With similar treatment we can derive the radius in velocity space.

.. note::
   The above paragraph probably belongs in the Component documentation.

.. note::
   The BaseMixture class expects the input data to be a single array of shape 
   ``(nsamples, nfeatures)``. This therefore doesn't allow for the 
   pre-construction of star covariance matrices. The typical usage also 
   doesn't expect uncertainties in the data, however this is customizable.



.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :glob:

   api/component/*


Example Component Usage
-----------------------

A simple usage will look something like this::

    # ...
    # initial conditions is a list of Component objects with pars set sensibly
    inital_conditions = get_some_valid_initial_conditions(ComponentClass)

    # config params is a dictionary
    config_params = read_relevant_config_params(config_file)

    # data is array-like of shape (n_samples, n_features)
    data = read_in_preprocessed_data

    # Note: the Component objects know how to interpret the data

    # Initialse model, and fit to data
    m = ComponentMixture(**config_params)
    m.set_params(initial_conditions)
    m.fit(data)


Suggested Component implementation
----------------------------------

The bulk of the implementation is within sklearn's BaseMixture. The key abstract methods that ComponentMixture must implement are :func:`ComponentMixture.set_params` and :func:`ComponentMixture_estimate_log_prob`.

These could be done as follows::
  
  def set_params(self, components):
    self.components = components

  def _estimate_log_prob(self, X):
    n_samples = X.shape[0]
    n_components = len(self.components)

    log_probs = np.zeros((n_samples, n_components))
    for k, component in enumerate(self.components):
      log_probs[:, k] = component.estimate_log_prob(X)
    
    return log_probs

