==================
SpaceTimeComponent
==================

.. class:: SpaceTimeComponent

    A component that fits to the inferred origin of the samples.

    The implementation of this class will closely follow that of Paper 1. However, the component object does not consider uncertainties.

    .. method:: __init__()

        Constructor method

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

    .. method:: _project_orbit(start_pos, time)

        Project the orbit from starting phase-space position `start_pos` by `time` Myr.

        :param start_pos: The starting phase-space position in RHS, LSR-centered, XYZUVW
        :type start_pos: array-like of shape (6)
        :param time: The amount of time in Myr to project orbit. Positive values are forward projection. Negative values are backwards.
        :type time: float

        :return: Final position of orbit
        :rtype: array-like of shape (6)