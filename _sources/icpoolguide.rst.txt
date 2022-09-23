=============================
Initial Conditions Pool Guide
=============================

An Initial Conditions Pool manages a pool of initial conditions which could be
used to initialise a :class:`ComponentMixture` object. An IC Pool yields 
initial conditions to another object (e.g. :class:`Driver`) and expects a fit
to be performed, and the score to be registered with the IC Pool. Using a fit
and score registry, the IC Pool can keep the pool populated with reasonable
initial conditions for future runs.

The precise mechanism for how new initial conditions are generated depends on
the chosen :class:`BaseInserter`.

I've chosen to utilise some fancy python in order to make the IC Pool as general
and flexible as possible. The :func:`BaseICPool.pool` method returns a *generator*
which can be iterated over.

A simple implementation :class:`SimpleICPool` identifies the best model with :math:`k`
components and uses this to generate a set of :math:`k+1`-component initial conditions
(i.e. how original Chronostar did things). :ref:`See below<Example SimpleICPool Usage>`
for example usage and implementation.

However, more advanced approaches are possible with this interface design. For example
a (as of yet not implemented) :class:`FancyICPool` could take the best 3 :math:`k`-component
fits and combine them to produce a :math:`k+3`-component initial condition set.

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :glob:

   api/icpool/*

.. note::
  Need to work out where the very first initial conditions come from. Is `ICPool` responsible, or is an `Introducer` object?

Example SimpleICPool Usage
--------------------------

The usage will look something like this::

  icg = InitialConditionsGenerator(SphereComponent, LazyInserter)

  for ident, init_conds in icg.initial_conditions():
    m = Model(config_params) 
    m._set_parameters(init_conds)
    m.fit(data)
    icg.register_result(ident, m.params, my.bic(data))

  # loop will end when icg is no longer able to generate reasonable initial conditions
  best_model = icg.best_model()

See? So elegant. Behind the scenes, the ICPool object is tracking which models 
performed the best, and therefore which models are the best candidates for 
further exploration. The ICPool can be as clever or as simple as the user 
desires. The method of how a new component is introduced is also left 
completely free.

Suggested SimpleICPool implementation
-------------------------------------

A *serial* ICG implementation could look a little like this::
  
  class InitialConditionsGenerator():

    def __init__(self, ComponentClass, InsertionApproach):
      self.component_class = ComponentClass
      self.insertion_approach = InsertionApproach()

    def initial_conditions(self):

      best_model = None
      prev_best_score = None
      best_score = -np.inf
      while prev_best_score is None or best_score > prev_best_score:
        prev_best_score = best_score
        self.registry = {}

        # Loop over the next generation of initial conditions
        for ix, init_conditions in enumerate(self.insertion_approach.next_gen(best_model)):
          yield ix, init_conditions

        # Once all initial conditions are provided, look for best one in registry
        best_model, best_score = max(self.registry.values() key=lambda x: x[1])

        # Using best fit, repeat until score ceases to improve

    def register_result(self, ident, model, score):
      self.registry[ident] = (model, score)

Ok, maybe not so simple now... but still a promising approach, I think.

