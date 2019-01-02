.. ############################################################################
.. install.rst
.. ===========
.. Author : Leo Serena [leo.serena@epfl.ch]
.. ############################################################################

Installation
============

To install the package, first clone the repository into a folder::

    git clone https://c4science.ch/source/FRIsOO.git

Then go in the directory::

    cd fri_oo

now install the package using pip::

    pip install .

Remarks
-------

The needed dependencies should automatically install themselfs, namely: *numpy*, *scipy*, *matplotlib*, *joblib* and *pandas*.
To manually install them using anaconda::

    $ conda activate [env]
    $ conda install numpy, scipy, matplotlib, joblib, pandas
