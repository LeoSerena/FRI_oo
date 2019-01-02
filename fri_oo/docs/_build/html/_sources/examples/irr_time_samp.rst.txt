.. ##################################################################################
.. irr_time_samp.rst
.. =================
.. Author: Leo Serena [leo.serena@epfl.ch]
.. ##################################################################################

Irregular Time Sampling Reconstruction
======================================

Irregular Time Sample Theory
****************************

As a reconstruction algorithm, we want to reconstruct a periodic signal represented by the sum of stream of diracs

.. math::
    \sum_{k' \in Z} \sum_{k = 1}^{K} \alpha_{k} \delta (t - t_k - k' \tau)

Where the :math:`t_k` and the :math:`a_k` are the unknown. The measurements are sampled as

.. math::
    y_l = \sum_{k=1}^K \varphi (t_{l}' - t_k), l \in 1,...,L

and :math:`\varphi(t) = \frac{sin(\pi B t)}{B \tau sin(\pi t / \tau)}` is the dirichlet kernel where B is the bandwidth.

The Fourier transform of the signal x(t) is (since it is a repeating signal)

.. math::
    \hat{x}_m = \frac{1}{\tau} \sum_{k=1}^K a_k e^{-j \frac{2 \pi m}{\tau} t_k}

The measurements are thus related to :math:`\hat{x}_m` by a linear equation G

.. math::
    y_l = \sum_{ | m | < \lfloor \frac{\tau B}{2} \rfloor} \hat{x}_m e^{-j \frac{2 \pi m}{\tau} t_l'}

.. math::
    G = 
    \begin{bmatrix}
    e^{-j \frac{2 \pi (-M)}{\tau} t_1'} & e^{-j \frac{2 \pi (-M+1)}{\tau} t_1'} & ... & e^{-j \frac{2 \pi M}{\tau} t_1'} \\
    e^{-j \frac{2 \pi (-M)}{\tau} t_2'} & e^{-j \frac{2 \pi (-M+1)}{\tau} t_2'} & ... & e^{-j \frac{2 \pi M}{\tau} t_2'} \\
    ... & ... & ... & ... \\
    e^{-j \frac{2 \pi (-M)}{\tau} t_L'} & e^{-j \frac{2 \pi (-M+1)}{\tau} t_L'} & ... & e^{-j \frac{2 \pi M}{\tau} t_L'}\\
    \end{bmatrix}

for :math:`M = \lfloor \frac{\tau B}{2} \rfloor`

By knowing this and using the annihilating filter, the problem is then a constrained minimization

.. math::
    min_{b,c \in C} || a - Gb ||^2 \\
    subject\ to\ b * c = 0

where
    * **a** is the column vector of the measurements
    * **b** is the column vector of the sampled signal
    * **c** is the annihilationg filter
    * G is the linear mapping as explained above

In the noisy case, the reconstruction can't be perfect anymore and has to be consistent with the noise level :math:`\epsilon^2`.

.. math::
    find\ b,c \in C \\
    subject\ to\ b * c = 0, \\
    || a - Gb ||^2 < \epsilon^2 \\

This is achievable because b is a FRI signal.

For more information about the algorithm, see the `original paper`_ .

.. _original paper: https://infoscience.epfl.ch/record/222498/files/article.pdf?version=5

Irregular Time Sample Applications
**********************************

Let's see how to use this algorithm for time domain samples.
First, install the package, see :ref:`Installation` page.

Noiseless Example
-----------------

Reconstruction setup::

    from fri_oo.irr_time_samp import IrrTimeSamp as ITS

    K = 5                                   # number of diracs
    tau = 1.                                # period of the signal
    M = K                                   # we don't need to oversample since it is a noiseless example
    L = 2 * M + 1                           # number of measurements
    B = (2. * M + 1)                        # bandwidth
    its_noiseless = ITS(K, tau, B, L)
    its_noiseless.setup()                   # in this example, we use the built-in signal generator
    its_noiseless.add_noise()               # we don't add any noise

Reconstruction::

    its_noiseless.reconstruction()          # this will apply the reconstruction with default parameters

the reconstructed diracs can then be displayed using panda by using *show_results*::

    its_noiselss.show_results()

+-------------+------------------+-------------+-------------------+
| original tk | reconstructed tk | original ak | reconstructed ak  | 
+=============+==================+=============+===================+
|  0.121755   |     0.121755     |   1.197991  |      1.197991     |
+-------------+------------------+-------------+-------------------+
|  0.136733   |     0.136733     |  -0.591423  |     -0.591423     |
+-------------+------------------+-------------+-------------------+
|  0.189521   |     0.189521     |  -1.027550  |     -1.027550     |
+-------------+------------------+-------------+-------------------+
|  0.509655   |     0.509655     |   1.330009  |      1.330009     |
+-------------+------------------+-------------+-------------------+
|  0.853141   |     0.853141     |   0.554392  |      0.554392     |
+-------------+------------------+-------------+-------------------+

to recover the :math:`a_k` and :math:`t_k` reconstructed::

    ak = its_noiseless.ak_recon
    tk = its_noiseless.tk_recon

to plot the result (here we also have the original signal), we first save them::

    its_noiseless.save_results("its_noiseless")  # saved to the result file
    its_noiseless.plot()

.. image:: ../_static/images/ITS_noiseless_diracs.jpg

.. image:: ../_static/images/ITS_noiseless_cont.jpg

To see how to manage results: :ref:`Data Management`

Noisy Example
-------------

::

    K = 5
    tau = 1.
    M = K * 8                                   # we need to oversample the signal since we add noise
    L = 2 * M + 1
    B = (2. * M + 1)
    P = 5                                       # signal to noise ratio in dB

    ITS_noisy = ITS(K, tau, B, L)
    ITS_noisy.setup()
    ITS_noisy.add_noise(P = P)
    ITS_noisy.reconstruction(max_init = 50)     # here we do only half of the default initialisation
    ITS_noisy.save_results("ITS_noisy_example")
    ITS_noisy.show_results()

+-------------+------------------+-------------+-------------------+
| original tk | reconstructed tk | original ak | reconstructed ak  |
+=============+==================+=============+===================+
|   0.076222  |     0.075616     |  1.163884   |      1.258038     |
+-------------+------------------+-------------+-------------------+
|   0.237510  |     0.236998     |  -0.768787  |     -1.073441     |
+-------------+------------------+-------------+-------------------+
|   0.575985  |     0.514066     |  1.490636   |      0.646190     |
+-------------+------------------+-------------+-------------------+
|   0.691389  |     0.575437     |  0.882757   |      1.466266     |
+-------------+------------------+-------------+-------------------+
|   0.955820  |     0.690202     |  0.733382   |      0.957095     |
+-------------+------------------+-------------+-------------------+

::

    ITS_noisy.plot()

.. image:: ../_static/images/ITS_noisy_diracs.jpg

.. image:: ../_static/images/ITS_noisy_cont.jpg

Parameterized Reconstruction
----------------------------

In this example we will reconstruct the signal with given measurements and linear mapping G::

    K = 5
    tau = 1.
    M = K
    L = 2 * M + 1
    B = (2. * M) + 1

    ITS_param = ITS(K, tau, B, L)
    ITS_param.setup(G = G, a = a)       
    # we give the setup function the corresponding 
        # a (L x 2 measurements) 
        # G (L x M linear mapping)
    ITS_param.add_noise()
    ITS_param.reconstruction()

    ITS_param.save_results("ITS_param")
    ITS_param.plot()

.. image:: ../_static/images/ITS_param_diracs.jpg

.. image:: ../_static/images/ITS_param_cont.jpg