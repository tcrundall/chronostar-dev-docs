==================
SpaceTimeComponent
==================

.. class:: SpaceTimeComponent(params)

    A component that fits to the inferred origin of the samples.

    The implementation of this class will closely follow that of Paper 1. However, the component object does not consider uncertainties.

    :param params: A tuple of all parameters needed to define the component. Can be None as parameters can be set through calling :func:`set_parameters` or :func:`maximize` .
    :type params: tuple

    .. method:: __init__()

        Constructor method

    .. method:: configure(cls, minimize_method='brent', reg_covar=1e-6, trace_orbit_func=trace_epicyclic_orbit, morph_cov_func=remove_posvel_correlations, **kwargs)

        Class method. Set class level configuration parameters. This must be called (even with just empty argument list) before objects can be instantiated. Defaults stored in method signature.

        :param minimize_method: method used by scipy.optimize.minimize_scalar for optimizing the :func:`_loss`. Default 'brent'.
        :type minimize_method: str
        :param reg_covar: A regularization constant added to the diagonal elements of covariance matrices to ensure invertability. Default 1e-6.
        :type reg_covar: float
        :param trace_orbit_func: A function that traces an orbit from a starting point in feature space through time by a certain age. Default trace_epicyclic_orbit.
        :type trace_orbit_func: Callable of form (start: [n\_dim], age: float, \*args) -> end: [n\_dim]
        :param morph_cov_func: A function that applies birth-site assumptions to the feature-space covariance matrix. Default remove_posvel_correlations.
        :type morph_cov_func: Callable of form (approx_birth_covariance: [n\_dim, n\_dim]) -> birth_covariance: [n\_dim, n\_dim]

    .. method:: estimate_log_prob(X)

        Estimate the log probability of each sample given the component. If the component is a simple Gaussian, this method would evaluate the Gaussian at the position of the sample. The features and how a component interprets them are unrestricted. Age, chemical composition etc. can all be in the feature, and this method determines how the component treats them.

        :param X: Input data
        :type Array-like(n_samples, n_features):

        :return: log probabilities of each sample
        :rtype: Array-like(n_samples)

    .. method:: maximize(X, log_resp)

        Find the best parameters given data `X` and log responsibilities (or membership probabilities).

        :param X: Input data
        :type X: array-like of shape (n_samples, n_features)
        :param log_resp: Log of posterior probabilities of the point of each sample in `X`
        :type log_resp: array-like of shape (n_samples)

    .. method:: n_params()

        Property. Return the number of parameters used to describe component.
    
    .. note::

        Currently hard coded for default morph_cov_func remove_posvel_correlations. Maybe there's a way to infer how many parameters survive morph_cov_func, or have the function explicitly return n_cov_params...
    
    .. method:: split()

        Split the component along the primary axis of its covariance matrix, generating two new components

        :return: Two components, representing `self` split in feature space along its primary axis.
        :rtype: tuple(BaseComponent, BaseComponent)

    .. note::

        Implemented in parent class :class:BaseComponent

    .. method:: get_parameters()

        :return: A tuple of all parameters needed to define the component in feature-space (i.e. current-day), mean, covariance, age
        :rtype: tuple(Array-like of shape (6), Array-like of shape (6,6), float)

    .. method:: set_parameters(params)
        :abstractmethod:

        :param params: A tuple of all parameters needed to define the component in feature-space (i.e. current-day), mean, covariance, age
        :type params: tuple(Array-like of shape (6), Array-like of shape (6,6), float)

