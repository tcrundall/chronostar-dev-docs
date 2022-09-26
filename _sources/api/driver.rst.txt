Driver
======
Manage entire process of fitting a mixture to data. This includes parsing the config file, initialising provided modules/classes, handle any mpi pool setup, set up logging, and perform fit until convergence is signalled.

The interface is defined as follows:

.. class:: Driver(self, config_file, component_class, mixture_class, icpool_class, introducer_class, uncertaintyfinder_class)

    The top level driver that manages the overall fit to data, accounting for
    high-level parameters, such as number of components. It also (ideally) will 
    handle the management of MPIPool processes.

    :param config_file: A yaml file that contains the configuration parameters for `Driver` and all the other classes.
    :type config_file: str
    :param component_class: A Class that either inherits from BaseComponent or matches the interface. This Class encapsulates the parametrisation of a single component.
    :type component_class: subclass(BaseComponent)
    :param mixture_class: A Class that either inherits from BaseMixture or matches the interface exactly. This Class encapsulates the parameterisation of the entire mixture as well as it's fitting method.
    :type mixture_class: subclass(ChronBaseMixture)
    :param icpool_class: A Class that either inherits from BaseICPool or matches the interface exactly. This Class manages the generation of the `mixture_class`'s initial conditions.
    :type icpool_class: subclass(BaseICPool)
    :param introducer_class: A Class that either inherits from BaseIntroducer or matches the interface exactly. This Class handles the initialisation of the next set of mixtures. For example, if we have just fit a collection of mixtures with N components, `introducer_class` will generate a set of initial conditions for `N+1` components.
    :type introducer_class: subclass(BaseIntroducer)
    :param uncertaintyfinder_class: A Class that either inherits from BaseUncertaintyFinder or matches the interface exactly. Once the final best fit is found, this Class explores the parameter space and returns uncertanties for each of the best fit parameters.
    :type uncertaintyfinder_class: sublcass(BaseUncertaintyFinder)

    .. method:: __init__()

        Constructor method

    .. method:: run(data)

        Run the entire fit.

        :param data: The data the mixture will fit to. In order to align with sklearn, I propose an array with shape ``(nsamples, nfeatures)``.

        :return: Best mixture with best fit parameters and uncertainties, and membership probabilities.
        :rtype: tuple(mixture_class, list, array-like of shape (n_samples, n_features))

.. note::
    There is only one place where ``sklearn.mixture.BaseMixture`` expects input data (``X``) to have a ``shape`` attribute, so this could be worked around if desired, however I propose to simplify our problem so that it matches ``sklearn`` design exactly, and then we can branch out from there.
