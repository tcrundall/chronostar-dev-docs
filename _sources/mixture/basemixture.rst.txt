================
ChronBaseMixture
================

Our base mixture inherits from sklearn.mixture.basemixture. :class:`ChronBaseMixture` is a linear combination of :class:`BaseComponent` and :class:`BaseBackground`.


.. class:: ChronBaseMixture(sklearn.mixture.basemixture)

    A mixture model of arbitrary components. This model encapsulates the parameters of the model and the methods which perform the fit.

    :param Component: Class of Component which describe the data points. Typically time dependent, 6D gaussians.
    :type Component: subclass(BaseComponent)
    :param Background: Class of Background which describe the "background" data points.
    :type Background: subclass(BaseBackground)


    .. method:: __init__(Component, Background)

        Constructor method


    .. attribute:: attribute1

        Some attribute

        :type: attribute type

    .. method:: some_abstract_method()
        :abstractmethod:

        Describe method

        :return: Describe return object
        :rtype: generator

    .. method:: some_method(param1)

        Describe some_method

        :param param1: describe param1
        :type param1: int




