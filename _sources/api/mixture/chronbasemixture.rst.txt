================
ChronBaseMixture
================

.. class:: ChronBaseMixture(config_params)

    This class serves to provide a consistent, inheritable interface of Mixture Models used by Chronostar.

    A mixture model of arbitrary components. This model encapsulates the parameters of the model and the methods which perform the fit.

    The key methods that children classes of :class:`ChronBaseMixture` must implement are :func:`set_params`, :func:`fit`.

    .. note::
        This list of methods is incomplete. I will extend the list as I work out the minimal working set of methods. The design will be tailored to the sklearn interface. Any desired implementations that are inconsistent with this will need wrappers, though I anticipate the chance of this will be low, and I'll endeavour to avoid this outcome.

    .. method:: set_params(params)
        :abstractmethod:

        Set the initial parameters of the mixture model.

        :return: None
        :rtype: NoneType

    .. method:: fit()
        :abstractmethod:

        Perform the fit

        :return: None
        :rtype: NoneType