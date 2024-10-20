=================================  
API
=================================

.. py:function:: GWDALI.GWDALI(Detection_Dict, FreeParams, detectors, approximant='TaylorF2', fmin=1, fmax=1.e4, fsize=3000, dali_method='Fisher_Sampling', sampler_method='nestle', new_priors = None, save_fisher=True, save_cov=True, plot_corner=True, save_samples=True, hide_info=False, index=1, rcond=1.e-4, diff_order=2, step_size=1.e-6, run_sampler=True, npoints=300)

	Return GW samples, Fisher and covariance matrix, parameters uncertainties, parameters recovered and signal to noise ratio (SNR).

	:param Detection_Dict: A dictionary of GW parameters;
	:param FreeParams: list of free parameters among the available ['m1', 'm2', 'RA', 'Dec', 'DL', 'inv_dL', 'ln_dL', 'iota', 'cos_iota', 'psi', 't_coal', 'phi_coal', 'sx1', 'sy1', 'sz1', 'sx2', 'sy2', 'sz2','S1','theta_1','phi_1','S2','theta_2','phi_2']
	:param detectors: list of dictionaries for each detector interferometer (for Einstein Telescope you need to specify its three interferometers configuration). Each detector dictionary needs to have the following keys:

		* ``name``: (str) The detector name for which the *Noise Power Spectral Density* will be chosen. Available detectors: ['aLIGO', 'aVirgo', 'KAGRA', 'ET', 'CE'];
		* ``lon``: (float) The detector longitude (degrees);
		* ``lon``: (float) The detector latitude (degrees);
		* ``rot``: (float) X-arm detector orientation starting from North-South direction (degrees);
		* ``shape``: (float) Opening angle between arms interferometer (degrees);

	:param approximant: GW approximant among the available ['Leading_Order', 'TaylorF2'_py, ...] (or another approximant provided by lal). To use the lal approximants you need to have installed `lal/lalsuite <https://lscsoft.docs.ligo.org/lalsuite/lalsuite/index.html>`_ in your machine.
	:param fmin: initial frequency value to the GW signal be evaluated.
	:param fmax: final frequency value to the GW signal be evaluated.
	:param fsize: number of frequency points.
	:param dali_method: DALI method [``'Fisher_Sampling'``, ``'Doublet'``, ``'Triplet'``, ``'Standard'``] or only ``'Fisher'`` for a simple numerical matrix inversion. The 'Standard' method use the complete GW likelihood (with no approximation).
	:param sampler_method: Method used for DALI (the same ones available in `bilby package <https://lscsoft.docs.ligo.org/bilby/>`_)
	:param new_priors: Redefine your priors
	:param save_fisher: Save the Fisher Matrix in a file named 'Fisher_Matrix_<index>.txt' where ``index`` is the integer argument bellow
	:param save_cov: Save the Covariance Matrix in a file named 'Covariance_<index>.txt'.
	:param plot_corner: Make a corner plot when using DALI methods.
	:param save_samples: Save GW samples in a file named 'samples_<index>.txt' where each column correspond to the samples of one free parameter specified above;
	:param hide_info: Hide software outputs in the screen.
	:param index: Integer argument used in the saved .txt files.
	:param rcond: Same as rcond in `numpy.linalg.pinv <https://numpy.org/doc/stable/reference/generated/numpy.linalg.pinv.html>`_;
	:param diff_order: (Avalible 2 or 4) Numerical derivative precision, e.g. for a given step h, if diff_orde=2 the uncertainty is of order :math:`h^3`, if diff_order=4 the uncertainty is of order :math:`h^5`;  
	:param step_size: Relative step size in the numerical derivative, i.e., dx = max( step_size, step_size*x ) where x is some parameter value;
	:param npoints: Same as npoints, nsteps, nwalkers in `bilby package <https://lscsoft.docs.ligo.org/bilby/>`_;
	
	:type Detection_Dict: dict
	:type FreeParams: list
	:type detectors: list
	:type approximant: str
	:type fmin: float
	:type fmax: float
	:type fsize: float
	:type dali_method: str
	:type sampler_method: str
	:type new_priors: dict
	:type save_fisher: bool
	:type save_cov: bool
	:type plot_corner: bool
	:type save_samples: bool
	:type hide_info: bool
	:type index: int
	:type rcond: float
	:type diff_order: int
	:type step_size: float
	:type npoints: int

	:return: Return a dictionary with the following keys

		- ``Samples``: array_like with shape (len(FreeParams) , number of samples points)
	
		- ``Fisher``: array_like with shape (len(FreeParams),len(FreeParams))
	
		- ``CovFisher``: array_like with shape (len(FreeParams),len(FreeParams))
	
		- ``Covariance``: array_like with shape (len(FreeParams),len(FreeParams))
	
		- ``Recovery``: list of recovered parameters (when using DALI methods)
	
		- ``Error``: list of parameters uncertainties (Confidence Level = 60%)
	
		- ``SNR``: value of the GW source signal to noise ratio (float)

		- ``Tensors``: Arrays of DALI Tensors (e.g. for N free parameters we have: Fisher[dim= :math:`N^2`], Doublet12 [dim= :math:`N^3`], Doublet22 [dim= :math:`N^4`], Triplet13 [dim= :math:`N^4`] , Triplet23 [dim= :math:`N^5`], Triplet33 [dim= :math:`N^6`] )