.. chronostar-dev documentation master file, created by
   sphinx-quickstart on Sat Sep 17 16:22:57 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to chronostar-dev's documentation!
==========================================
**Chronostar** is a Python library for discovering and/or characterising
unbound stellar associations (aka "moving groups") and their members using 
features such as kinematics, age markers, chemical composition. The code 
can be found on `github <https://github.com/mikeireland/chronostar/>`_.

These docs will sketch out the structure of the next generation of Chronostar
code.

The goal of Chronostar is to be a flexible framework for fitting Gaussian
Mixture Models to astronomical data. This will be achieved by utilising
"injected dependencies". The :ref:`Driver <driver guide>` is composed of a collection
of 4 classes (:doc:`ICPool <icpoolguide>`, :doc:`Introducer <introducerguide>`, :doc:`Mixture <mixtureguide>` and :doc:`Component <componentguide>`) for which
Chronostar provides many implementations. Anyone wishing to modify 
aspects of Chronostar (e.g. input data, fitting method, models of 
components) simply needs to provide the :ref:`driver <Driver Guide>` with their own
class that matches the required interface, whether that be by writing
the class from scratch, or by using inheritance to extend pre-existing
classes.

It turns out fitting Gaussian Mixture Models is quite complex, so a lot of
the design will be shamelessly *inspired* by 
`scikit-learn <https://scikit-learn.org/stable/auto_examples/mixture/plot_gmm_selection.html>`_.

Select members of the chronostar development team may view and modify a
`graphical outline of the chronostar pipeline
<https://docs.google.com/drawings/d/17wZQVIBfof91lPKZWbGgBINZ8BsEbOVlWe1dcx11Foc/edit?usp=sharing>`_.

..
   Check out the :doc:`usage` section for further information, including how to :ref:`install <installation>` the project.

.. note::

   This project is under active development

.. toctree::
   :maxdepth: 1
   :caption: Contents:
   :glob:

   generaloverview
   driverguide
   mixtureguide
   componentguide
   icpoolguide
   introducerguide

..
   .. toctree::
      :maxdepth: 2
      :caption: Lost docs:
      :glob:

      uncertaintyfinder


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
