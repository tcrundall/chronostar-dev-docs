==============
BaseIntroudcer
==============

.. class:: BaseIntroducer

    The abstract base class for Introducer objects

    An Introducer uses the results of one or more previous fits to generate a series of reasonable initial conditions for future fits.

    A simple example would be: take a previous fit, for each component replace it by two components of identical current day mean, but with an age offset.

    :param component_class: A Class that either inherits from BaseComponent or matches the interface. This Class encapsulates the parametrisation of a single component.
    :type Component: subclass(BaseComponent)

    .. note::
        It is unclear if provision of a component class is necessary. It could be that the logic of insertion strategies could be contained with the component objects which are passes as arguments to :func:`~BaseIntroducer.next_gen` within the `prev_models` argument.

    .. method:: __init__(component_class)

        Constructor method

    .. method:: next_gen(prev_models)
        :abstractmethod:

        Using one or more previous models, produce the next generation of initial conditions.

        If `prev_models` is `None` then return some default initial conditions.

        :param prev_models: One or more previous models from which new initial conditions can be constructed
        :type prev_models: subclass(BaseMixture) or list[subclass(BaseMixture)] or None
        :return: A collection of initial conditions
        :rtype: Array-like (nconds, ncomps, npars)
    
    .. note::
        Maybe an Introducer needs access to the data, especially if we expect the Introducer to initialse the very first run...
