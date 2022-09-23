Driver Guide
============
Manage entire process of fitting a model to data. This includes parsing the config file, initialising provided modules/classes, handle any mpi pool setup, set up logging, and perform fit until convergence is signalled.

The interface is defined in :class:`Driver`.

The main loop of :class:`Driver` will get initial conditions from :class:`BaseICPool`, initialise a :class:`ComponentMixture` object, run the :func:`ComponentMixture.fit` method, report the score of the fit to :class:`BaseICPool` and track the best fit seen so far. The main loop continues until :class:`BaseICPool` ceases to yield initial conditions. The final fit is the best fit seen so far.

.. toctree::
   :maxdepth: 2
   :caption: API:
   :glob:

   api/driver

Example Driver usage
--------------------

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
This is an example of `dependency injection <https://en.wikipedia.org/wiki/Dependency_injection>`_ and has the consequence that Driver
doesn't need to know any specifics about the SphereComponent, the Driver only
expects the SphereComponent to adhere to a prescribed interface. This interface can be defined either as a "protocol" or by requiring all ``Component`` classes to inherit from the abstract ``BaseComponent``.

.. note::
    Dependency injection seems to be more about injecting objects and not classes. So this section will need to be updated as I explore the implementation further.

This provides maximal flexibility and extensibility, as anyone can write their
own component class or model class, and substitute this class into their own
script.

Proposed Driver implementation
-------------------------------
A rough implementation of :class:`Driver` could look like this::

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



