=============
BaseComponent
=============

Abstract base class for Components. These objects serve as building blocks of a :class:`BaseMixture`. A Component encapsulates its parameterisation and log probability estimation of samples. A Component will ideally map onto real stellar associations.


.. class:: BaseComponent(params)

    An abstract class that describes a single, time-dependent component of a mixture model.

    A Component Class is responsible for calculating its loss given a set of data points and responsibilities.

    :param params: A tuple of all parameters needed to define the component. Can be None as parameters can be set through calling :func:`set_parameters` or :func:`maximize` .
    :type params: tuple

    .. method:: __init__()

        Constructor method

    .. method:: configure(cls, **kwargs)
        :abstractmethod:

        Class method. Set class level configuration parameters. This must be called (even with just empty argument list) before objects can be instantiated. Defaults stored in method signature.

    .. note::

        Maybe configuration defaults should be defined in class, such that :method:configure must only be called if defaults are being modified...


    .. method:: estimate_log_prob(X)
        :abstractmethod:

        Estimate the log probability of each sample given the component. If the component is a simple Gaussian, this method would evaluate the Gaussian at the position of the sample. The features and how a component interprets them are unrestricted. Age, chemical composition etc. can all be in the feature, and this method determines how the component treats them.

        :param X: Input data
        :type Array-like(n_samples, n_features):

        :return: log probabilities of each sample
        :rtype: Array-like(n_samples)

    .. method:: maximize(X, log_resp)
        :abstractmethod:

        Find the best parameters given data `X` and log responsibilities (or membership probabilities).

        :param X: Input data
        :type X: array-like of shape (n_samples, n_features)
        :param log_resp: Log of posterior probabilities of the point of each sample in `X`
        :type log_resp: array-like of shape (n_samples)

    .. method:: n_params()
        :abstractmethod:
        
        Property. Return the number of parameters used to describe component.
    
    .. method:: split()

        Split the component along the primary axis of its covariance matrix, generating two new components

        :return: Two components, representing `self` split in feature space along its primary axis.
        :rtype: tuple(BaseComponent, BaseComponent)

    .. method:: get_parameters()
        :abstractmethod:

        :return: A tuple of all parameters needed to define the component
        :rtype: tuple

    .. method:: set_parameters(params)
        :abstractmethod:

        :param params: A tuple of all parameters needed to define the component
        :type params: tuple
