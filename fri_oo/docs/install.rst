.. ############################################################################
.. install.rst
.. ===========
.. Author : Leo Serena [leo.serena@epfl.ch]
.. ############################################################################

Installation
============

To install the package, first clone the repository into a folder::

    git clone https://github.com/LeoSerena/FRI_oo.git

Then go in the directory::

    cd fri_oo

The installation file are in the dist folder.

Now install the package using pip::

    pip install .

Remarks
-------

The needed dependencies should automatically install, namely: *numpy*, *scipy*, *matplotlib*, *joblib* and *pandas*.
To manually install them using anaconda::

    $ conda activate [env]
    $ conda install numpy, scipy, matplotlib, joblib, pandas
