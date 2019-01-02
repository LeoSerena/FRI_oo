.. ##################################################################################
.. DataManagement.rst
.. =================
.. Author: Leo Serena [leo.serena@epfl.ch]
.. ##################################################################################

Data Management
===============

This file explains how to manage saved data from the **results** file.

First, we need to import the plotter module::

    from fri_oo import plotter

retrieve_data()
---------------

Returns the list of the names of the saved reconstructions in the **results** file::

    plotter.retrive_data()

['ITS_example', 'IFS_example', 'FRI_Curve_example']

fetch_data(name: str)
---------------------

Returns the dictionary of the requested reconstruction::

    plotter.fetch_data(name)

plot_data(name: str)
---------------------

Plots the requested data, for example::

    plotter.plot_data('ITS_example') # or plotter.plot_data(plotter.retrieve_data()[0])

.. image:: ../_static/images/ITS_noisy_diracs.jpg

.. image:: ../_static/images/ITS_noisy_cont.jpg

delete_data(name: str)
----------------------

Deletes the given reconstruction from the results file::

    plotter.retrive_data()

['ITS_example', 'IFS_example', 'FRI_Curve_example']

::

    plotter.delete_data('ITS_example')

    plotter.retrieve_data()

    plotter.retrive_data()

['IFS_example', 'FRI_Curve_example']