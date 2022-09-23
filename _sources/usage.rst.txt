Usage
=====

.. _installation:

Chronostar Installation
-----------------------

To use Chronostar, first clone from the `github repo <https://github.com/mikeireland/chronostar/>`_.

.. code-block:: console

   $ git clone https://github.com/mikeireland/chronostar/

Then do more things

Performing a fit
----------------

This is some placeholder documentation.... To perform a fit to data, you can use the
``chronostar.fitfitter.fitfit()`` function:

.. py:function:: chronostar.fitfitter.fitfit(data=None)

   Gets the best fit to the data.

   :param data: Optional input "data".
   :type data: astropy.table.Table or None
   :return: The best fit
   :rtype: list[Component]

