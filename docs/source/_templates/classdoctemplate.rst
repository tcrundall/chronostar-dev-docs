==========
@ClassName
==========

The abstract base class for initial conditions pool objects.
These objects manage a pool of initial conditions which :class:`Model` uses to initialise a fit.


.. class:: @ClassName

    A pool (or priority queue?) of initial conditions that can be used to initialise model fits.

    Calling :method:`pool` returns an iterator that can be iterated over, yielding a set of initial conditions. By registering the result of each fit with the :method:`register_result`, ICPool can use this information to generate the next set of initial conditions

    :param construct_param1: Describe parameter used in constructor
    :type construct_param1: int

    .. method:: __init__(construct_param1)

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

