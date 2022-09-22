==============
BaseBackground
==============

Abstract base class for Backgrounds. The objects (along with :class:`BaseComponent` objects) serve as building blocks for a :class:`BaseMixture`. These objects encapsulate their parametrisation and log probabilty calculation of samples. Ideally :class:`BaseBackground` components describe the "field" stars.


.. class:: BaseBackground

    An abstract class that describes a single, time-independent component of a mixture model.

    A Background Class is responsible for calculating its loss given a set of data points and responsibilities.

    .. method:: __init__()

        Constructor method

    .. method:: estimate_log_prob(X)
        :abstractmethod:

        Estimate the log probability of each sample given the background component. If the background component is a simple Gaussian, this method would evaluate the Gaussian at the position of the sample. The features and how a component interprets them are unrestricted. Age, chemical composition etc. can all be in the feature, and this method determines how the component treats them.

        :param X: Input data
        :type Array-like(n_samples, n_features):

        :return: log probabilities of each sample
        :rtype: Array-like(n_samples)


