=============
BaseComponent
=============

Abstract base class for Components. These objects serve as building blocks of a :class:`BaseMixture`. A Component encapsulates its parameterisation and log probability estimation of samples. A Component will ideally map onto real stellar associations.


.. class:: BaseComponent

    An abstract class that describes a single, time-dependent component of a mixture model.

    A Component Class is responsible for calculating its loss given a set of data points and responsibilities.

    .. method:: __init__()

        Constructor method

    .. method:: estimate_log_prob(X)
        :abstractmethod:

        Estimate the log probability of each sample given the component. If the component is a simple Gaussian, this method would evaluate the Gaussian at the position of the sample. The features and how a component interprets them are unrestricted. Age, chemical composition etc. can all be in the feature, and this method determines how the component treats them.

        :param X: Input data
        :type Array-like(n_samples, n_features):

        :return: log probabilities of each sample
        :rtype: Array-like(n_samples)

