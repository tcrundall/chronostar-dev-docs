==================
SpaceComponent
==================

.. class:: SpaceComponent

    A component that fits to the current day samples (stars).

    This component should characterise the background.

    .. method:: __init__()

        Constructor method

    .. method:: estimate_log_prob(X)

        Estimate the log probability of each sample given the component.

        :param X: Input data
        :type Array-like(n_samples, n_features):

        :return: log probabilities of each sample
        :rtype: Array-like(n_samples)

    .. method:: maximize(X, log_resp)

        Find the best parameters given data `X` and log responsibilities (or membership probabilities).
        Since there are no data uncertainties and no age parameter, this method can be implemented with two numpy functions, e.g. the weighted versions of `np.mean` and `np.covariance`.

        :param X: Input data
        :type X: array-like of shape (n_samples, n_features)
        :param log_resp: Log of posterior probabilities of the point of each sample in `X`
        :type log_resp: array-like of shape (n_samples)