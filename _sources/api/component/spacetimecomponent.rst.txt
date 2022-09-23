==================
SpaceTimeComponent
==================

.. class:: SpaceTimeComponent

    A component that fits to the inferred origin of the samples.

    The implementation of this class will closely follow that of Paper 1. However, the component object does not consider uncertainties.

    .. method:: __init__()

        Constructor method

    .. method:: estimate_log_prob(X)
        :abstractmethod:

        Estimate the log probability of each sample given the component. If the component is a simple Gaussian, this method would evaluate the Gaussian at the position of the sample. The features and how a component interprets them are unrestricted. Age, chemical composition etc. can all be in the feature, and this method determines how the component treats them.

        :param X: Input data
        :type Array-like(n_samples, n_features):

        :return: log probabilities of each sample
        :rtype: Array-like(n_samples)

