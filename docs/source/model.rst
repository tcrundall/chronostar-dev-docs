AgedGaussianMixture
===================

Ideally extends the 
`BaseMixture class from sklearn <https://github.com/scikit-learn/scikit-learn/blob/36958fb240fbe435673a9e3c52e769f01f36bec0/sklearn/mixture/_base.py>`_.

To achieve first the **minimal working example**, I propose disregarding data uncertainties completely. RV's are pretty good (compared to the occupied velocity volumn of associations), and datasets with no RV's are too large for Chronostar to successfully fit in it's current implementation anyway. By disregarding data uncertainties, the parameter space to be explored for each (traditional) component is restricted to one dimension only, the age.

If we ignore data uncertainties, the age is the only free parameter in the maximisation step. To explain this further, consider fixing the age at 0. In this case we could use `sklearn.mixture.GaussianMixture` out of the box. There is no parameter exploration needed, because the maximisation step calculates the mean and covariance from the "assigned members" (a.k.a. samples of responsibility(?)).

`AgedGaussianMixture` introduces a free age parameter for each of its components. The key remaining **dependent** parameters of a component are :math:`\mu_0` and :math:`\Sigma_0`. :math:`\mu_0` is constrained completely by the age and data, since we just project :math:`\mu` back in time by :math:`t`. :math:`\Sigma_0` is less apparent, but with some assumptions can be equally constrained. We get :math:`\Sigma` from the assigned members. We project this back in time by :math:`t` using a Jacobian. We now have a :math:`~\Sigma_0`. The final step is to impose the core assumptions of a Component - that there is no positional-velocity correlation - and impose assumptions specific to the chosen component class. E.g. for the :class:`SphereComponent` we assume spherical in position space and spherical in velocity space. Therefore in this case we can derive :math:`\Sigma_0` by finding the total space-volume of :math:`~\Sigma_0` and inferring the required radius of an equal volume sphere in position space. With similar treatment we can derive the radius in velocity space.

.. note::
   The above paragraph probably belongs in the Component documentation.

.. note::
   The BaseMixture class expects the input data to be a single array of shape 
   ``(nsamples, nfeatures)``. This therefore doesn't allow for the 
   pre-construction of star covariance matrices. The typical usage also 
   doesn't expect uncertainties in the data, however this is customizable.

inputs

- config params
- data
- [mpi pool]

injected dependencies

- Component Class
- Background Class
- Loss function

key public methods:

- fit
- bic

key private methods:

- is_converged
- e_step
- m_step

outputs

- best fit of given model

.. class:: SpaceTimeMixture(n_components=1, *, covariance_type='full', tol=0.001, reg_covar=1e-06, max_iter=100, n_init=1, init_params='kmeans', weights_init=None, means_init=None, precisions_init=None, random_state=None, warm_start=False, verbose=0, verbose_interval=10)

   This class both models and fits a Gaussian Mixture Model with time
   dependence. Since the EM algorithm is mighty complicated, we elect 
   to follow the design laid out by :class:`sklearn.mixture.BaseMixture`.
   
   BaseMixture is an abstract class, and so we must implement the missing
   methods.

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


   .. method:: placeholderMethod(self, uuids=None)

      This documentation here is merely placeholding for syntax.

      Returns a list of :class:`bluepy.blte.Service` objects representing
      the services offered by the device. This will perform Bluetooth service
      discovery if this has not already been done; otherwise it will return a
      cached list of services immediately..

      :param uuids: A list of string service UUIDs to be discovered,
         defaults to None
      :type uuids: list, optional
      :return: A list of the discovered :class:`bluepy.blte.Service` objects,
         which match the provided ``uuids``
      :rtype: list On Python 3.x, this returns a dictionary view object,
         not a list


