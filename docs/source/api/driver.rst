Driver
======
Manage entire process of fitting a model to data. This includes parsing the config file, initialising provided modules/classes, handle any mpi pool setup, set up logging, and perform fit until convergence is signalled.

The interface is defined as follows:

.. class:: Driver

    The top level driver that manages the overall fit to data, accounting for
    high-level parameters, such as number of components. It also (ideally) will 
    handle the management of MPIPool processes.

    .. method:: __init__(self, config_file, component_class, model_class, icg_class, inserter_class, uncertaintyfinder_class)

        Initialise a Driver objects

        :param config_file: A yaml file that contains the configuration parameters for `Driver` and all the other classes.
        :type config_file: str
        :param component_class: A Class that either inherits from BaseComponent or matches the interface. This Class encapsulates the parametrisation of a single component.
        :type component_class: subclass(BaseComponent)
        :param model_class: A Class that either inherits from BaseModel or matches the interface exactly. This Class encapsulates the parameterisation of the entire model as well as it's fitting method.
        :type model_class: subclass(sklearn.mixture.BaseMixture)
        :param icg_class: A Class that either inherits from BaseICG or matches the interface exactly. This Class manages the generation of the `model_class`'s initial conditions.
        :type icg_class: subclass(BaseICG)
        :param inserter_class: A Class that either inherits from BaseInserter or matches the interface exactly. This Class handles the initialisation of the next set of models. For example, if we have just fit a collection of models with N components, `inserter_class` will generate a set of initial conditions for `N+1` components.
        :type inserter_class: subclass(BaseInserter)
        :param uncertaintyfinder_class: A Class that either inherits from BaseUncertaintyFinder or matches the interface exactly. Once the final best fit is found, this Class explores the parameter space and returns uncertanties for each of the best fit parameters.
        :type uncertaintyfinder_class: sublcass(BaseUncertaintyFinder)

    .. attribute:: example_attr 

        An example attribute

        :type: array-like of shape (n_components,)

    .. method:: run(self, data)

        Run the entire fit.

        :param data: The data the model will fit to. In order to align with sklearn, I propose an array with shape ``(nsamples, nfeatures)``.

        :return: Best model with best fit parameters and uncertainties, and membership probabilities.
        :rtype: tuple(model_class, list, array-like of shape (n_samples, n_features))

.. note::
    There is only one place where ``sklearn.mixture.BaseMixture`` expects input data (``X``) to have a ``shape`` attribute, so this could be worked around if desired, however I propose to simplify our problem so that it matches ``sklearn`` design exactly, and then we can branch out from there.
