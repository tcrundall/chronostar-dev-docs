================
SimpleIntroducer
================

.. class:: SimpleIntroducer(BaseIntroducer)

    A simple implementation that introduces an extra component to a previous fit.

    See :ref:`Introducer Guide` for an example implementation and usage.

    .. method:: __init__(component_class)

        Constructor method

    .. method:: next_gen(prev_models)

        Using one or more previous models, produce the next generation of initial conditions.

        If `prev_models` is `None`, then generate some default start(?).

        :param prev_models: One or more previous models from which new initial conditions can be constructed
        :type prev_fits: subclass(BaseMixture) or list[subclass(BaseMixture)]
        :return: A collection of initial conditions
        :rtype: Array-like (nconds, ncomps, npars)
