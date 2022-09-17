Initial Conditions Generator
============================

Manages the generation of each generation of initial conditions, based on the performance of the previous generation.
For example, a Gaussian Mixture Model is a linear combination of `N` components. The ICG would use the performance of all attempts made with `N` components, and using some user definable approach, would generate the next set of `N + k` component models.

In order to be extremely general, the ICG is an actual *generator* wrapped around an ojbect that does the actual generation... these notes aren't very clear, but I promise you the end result will be extremely cool, flexible, and very straightforward to parallelise.

inputs:

- config params

injected dependencies:

- Component Class
- Component Inserter

The usage will look something like this::

  icg = InitialConditionsGenerator(SphereComponent, LazyInserter)

  for ident, init_conds in icg.initial_conditions():
    m = Model(init_conds) 
    m.fit(data)
    icg.register_result(ident, m.params, my.bic(data))

  # loop will end when icg is no longer able to generate reasonable initial conditions
  best_model = icg.best_model()

See? So elegant. Behind the scenes, the ICG is bookkeeping which models performed the best, and therefore which models are the best candidates for further exploration. The ICG can be as clever or as simple as the user desires. The method of how a new component is introduced is also left completely free.

An ICG implementation could look a little like this::
  
  class InitialConditionsGenerator():

    def __init__(self, ComponentClass, InsertionApproach):
      self.component_class = ComponentClass
      self.insertion_approach = InsertionApproach

    def initial_conditions(self):

      best_model = None
      prev_best_score = None
      best_score = -np.inf
      while best_model is None or best_score > prev_best_score:
        prev_best_score = best_score
        self.pars_registry = {}
        self.score_registry = {}

        # Loop over the next generation of initial conditions
        for ix, init_conditions in enumerate(self.insertion_approach.next_gen(best_model)):
          yield ix, init_conditions

        # Once all initial conditions are provided, look for best one in registry
        best_ix = max(self.score_registry, key=self.score_registry.get)
        best_model = self.pars_registry[best_ix]
        best_score = self.score_registry[best_ix]

        # Using best fit, repeat until score ceases to improve

    def register_result(self, ident, parameters, score):
      self.pars_registry[ident] = parameters
      self.score_registry[ident] = score

Ok, maybe not so simple now... but still a promising approach, I think.

      

      
      



    



