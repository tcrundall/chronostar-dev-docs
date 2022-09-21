=============
BaseComponent
=============

Abstract base class for Components

The abstract base class for initial conditions pool objects.
These objects manage a pool of initial conditions which :class:`Model` uses to initialise a fit.


.. class:: BaseComponent

    An abstract class that describes a single, time-dependent component of a mixture model.

    A Component Class is responsible for calculating its loss given a set of data points and responsibilities.

    .. method:: __init__()

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


