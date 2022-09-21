============
BaseInserter
============

The abstract base class for inserters.

An inserter takes one (or more) previous model fit(s) and determines multiple plausible locations to introduce another :class:`Component`. The inserter communicates all plausible locations by returning a set of multiple initial conditions.


.. class:: BaseInserter

    An Inserter uses the result/s of a/many previous fit/s to generate a series of reasonable initial conditions to try next.

    A simple example would be: take a previous fit, for each component replace it by two components of identical current day mean, but with an age offset.

    :param component_class: A Class that either inherits from BaseComponent or matches the interface. This Class encapsulates the parametrisation of a single component.
    :type Component: subclass(BaseComponent)

    .. method:: __init__(component_class)

        Constructor method

    .. method:: next_gen(prev_fit)
        :abstractmethod:

        Using one or more previous fits, generate the next generation of initial conditions.

        :param prev_fits: One or more previous fits from which new initial conditions can be constructed
        :type prev_fits: subclass(BaseMixture) or list[subclass(BaseMixture)]
        :return: A collection of initial conditions
        :rtype: Array-like (nconds, ncomps, npars)
