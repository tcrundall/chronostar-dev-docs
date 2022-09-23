================
ComponentMixture
================

.. class:: ComponentMixture(n_components=1, *, covariance_type='full', tol=0.001, reg_covar=1e-06, max_iter=100, n_init=1, init_params='kmeans', weights_init=None, means_init=None, precisions_init=None, random_state=None, warm_start=False, verbose=0, verbose_interval=10)

   This class mixes both the :class:`sklearn.mixture.BaseMixture` and :class:`ChronBaseMixture`.

   This class both models and fits an arbitaray Mixture Model with the 
   Expectation Maximisation (EM)
   algorithm. Since the EM algorithm is mighty complicated, we elect 
   to follow the design laid out by :class:`sklearn.mixture.BaseMixture`.
   
   BaseMixture is an abstract class, and so we must implement the missing
   methods.

   For now I've included all :class:`sklearn.mixture.BaseMixture` parameters, but I expect some are superfluous/incompatible to our needs and will be removed.

   To enable consistent API usage of all :class:`ChronBaseMixture` implementations, the specific configuaration parameters (i.e. those that are passed to __init__) will typically be read from a config file and passed as a dictionary.

   .. note::
      To ensure that nothing is hidden behind the scenes, I have dumped all the :class:`sklearn.mixture.GaussianMixture` parameters here. Certainly not all of them are required, and I'll whittle the list down over time.

   :param n_components: The number of mixture components.
   :type n_components: int, default=1
   :param covariance_type:  String describing the type of covariance parameters to use. Must be one of: 

      *  'full': each component has its own general covariance matrix.
      *  'tied': all components share the same general covariance matrix.
      *  'diag': each component has its own diagonal covariance matrix.
      *  'spherical': each component has its own single variance.
   :type covariance_type: {'full', 'tied', 'diag', 'spherical'}, default='full'
   :param tol: The convergence threshold. EM iterations will stop when the lower bound average gain is below this threshold.
   :type tol: float, default=1e-3
   :param reg_covar: Non-negative regularization added to the diagonal of covariance. Allows to assure that the covariance matrices are all positive.
   :type reg_covar: float, default=1e-6
   :param max_iter: The number of EM iterations to perform.
   :type max_iter: int, default=100
   :param n_init: The number of initializations to perform. The best results are kept.
   :type n_init: int, default=1
   :param init_params:  The method used to initialize the weights, the means and the precisions. String must be one of:

      *  'kmeans' : responsibilities are initialized using kmeans.
      *  'k-means++' : use the k-means++ method to initialize.
      *  'random' : responsibilities are initialized randomly.
      *  'random_from_data' : initial means are randomly selected data points.

   :type init_params: {'kmeans', 'k-means++', 'random', 'random_from_data'}, default='kmeans'
   :param weights_init: The user-provided initial weights. If it is None, weights are initialized using the init_params method.
   :type weights_init: array-like of shape (n_components, ), default=None
   :param means_init: The user-provided initial means, If it is None, means are initialized using the init_params method.
   :type means_init: array-like of shape (n_components, n_features), default=None
   :param precisions_init: The user-provided initial precisions (inverse of the covariance matrices). If it is None, precisions are initialized using the 'init_params' method. The shape depends on 'covariance_type':

      *   (n_components,)                        if 'spherical',
      *   (n_features, n_features)               if 'tied',
      *   (n_components, n_features)             if 'diag',
      *   (n_components, n_features, n_features) if 'full'

   :type precisions_init: array-like, default=None
   :param random_state: Controls the random seed given to the method chosen to initialize the parameters (see init_params). In addition, it controls the generation of random samples from the fitted distribution (see the method sample). Pass an int for reproducible output across multiple function calls. See Glossary.
   :type random_state: int, RandomState instance or None, default=None
   :param warm_start: If 'warm_start' is True, the solution of the last fitting is used as initialization for the next call of fit(). This can speed up convergence when fit is called several times on similar problems. In that case, 'n_init' is ignored and only a single initialization occurs upon the first call. See the Glossary.
   :type warm_start: bool, default=False
   :param verbose: Enable verbose output. If 1 then it prints the current initialization and each iteration step. If greater than 1 then it prints also the log probability and the time needed for each step.
   :type verbose: int, default=0
   :param verbose_interval: Number of iteration done before the next print.
   :type verbose_interval: int, default=10


   .. method:: set_params(components)

      :param components: List of component objects (i.e. as provided by :class:`SimpleIntroducer`)
      :type components: List[subclass(:class:`BaseComponent` and :class:`BaseBackground`)]

      Set the initial parameters of the mixture model.

      :return: None
      :rtype: NoneType


   .. method:: _estimate_log_prob(X)

      Estimate the log probability of each sample for each component. The components stored in :attr:`components` determine how the estimation is calculated.

      :param X: input data
      :type X: Array-like (n_samples, n_features)

      :return: The log probility of each sample for each component
      :rtype: Array-like (n_samples, n_components)

      An example implementation could be::

         log_probs = np.zeros((n_samples, n_components))
         for k, component in enumerate(self.components):
            log_probs[:, k] = component.estimate_log_prob(X)
         
         return log_probs

   .. attribute:: components

      A list of components (and background components) which retain their current best-fit parameters

      :type: List[subclass(BaseComponent) and/or subclass(BaseBackground)]
