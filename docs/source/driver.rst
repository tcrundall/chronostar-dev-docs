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

Example usage
-------------

A rough usage could look like this::

    from chronostar.driver import Driver
    from chronostar.component import SphereComponent
    from chronostar.model import Model
    from chronostar.initialconditions import InitialConditionsGenerator
    from chronostar.componentinserter import LazyInserter
    from chronostar.uncertaintyfinder import UncertaintyFinder

    driver = Driver(
        config_file='some_config_file.yml',
        component_class=SphereComponent,
        model_class=Model,
        icg_class=InitialConditionsGenerator,
        inserter_class=LazyInserter,
        uncertaintyfinder_class=UncertaintyFinder,
    )

    driver.fit(data)

Note that we pass classes (and not objects) to the driver when initialising it.
This is an example of *dependency injection* and has the consequence that Driver
doesn't need to know any specifics about the SphereComponent, the Driver only
expects the SphereComponent to adhere to a prescribed interface. This interface can be defined either as a "protocol" or by requiring all ``Component`` classes to inherit from the abstract ``BaseComponent``

This provides maximal flexibility and extensibility, as anyone can write their
own component class or model class, and substitute this class into their own
script.

Proposed implementation
-----------------------
A rough implementation of `driver` could look like this::

    class Driver:
        def __init__(
            self,
            config_file,
            component_class,
            model_class,
            icg_class,
            inserter_class,
            uncertaintyfinder_class,
        ):
            # Set all attributes
            self.config_file = config_file
            # etc ...

            # Initialise each class from pars in config file by calling static methods
            self.config_params = parse_config_file(self.config_file)

            self.component_class.config(self.config_params['component'])
            self.model_class.config(self.config_params['model'])
            # etc ...

        def run(self, data):
            inserter = self.inserter_class()
            icg = self.icg_class(self.component_class, inserter)

            # This design is perhaps a bit ambitious, but it lends itself well to parallelisation
            initial_conditions_generator = icg.initial_conditions()
            for unique_id, init_conds in initial_conditions_generator:
                m = Model(init_conds) 
                m.fit(data)
                icg.register_result(unique_id, m.params, my.bic(data))

            # loop will end when icg is no longer able to generate reasonable initial conditions
            best_model = icg.best_model()

            uncertaintyfinder = self.uncertaintyfinder_class()
            best_model, uncertainties, memberships = uncertaintyfinder(best_model, data)

            return best_model, uncertainties, memberships



