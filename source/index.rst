.. researchpy documentation master file, created by
   sphinx-quickstart on Mon Jul 23 11:23:25 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to researchpy's documentation!
======================================

Researchpy produces Pandas DataFrames that contains relevant statistical testing
information that is commonly required for academic research. The information is
returned as Pandas DataFrames to make for quick and easy exporting of results to
any format/method that works with the traditional Pandas DataFrame.

Researchpy is essentially a wrapper that combines various established packages
such as pandas, scipy.stats, numpy, and statsmodels to get all the standard required
information in one method. If analyses were not available in
these packages, code was developed to fill the gap.

Formula's are provided with citations if the code originated from researchpy.
All output has been tested and verified by comparing to established software
packages such as Stata, SAS, SPSS, and/or R.

Moving forward, Researchpy will be using formula_like approach and is being
streamlined during this process. For an example, see the new -difference_test- method.


.. note::

  researchpy is only compatible with Python 3.x. Download using either:

   * pip install researchpy
      * For standard install

   * conda install -c researchpy researchpy
      * For installation through conda



.. toctree::
   :maxdepth: 2
   :caption: Contents:

   codebook_documentation
   summary_cont_documentation
   summary_cat_documentation
   difference_test_documentation
   ttest_documentation
   crosstab_documentation
   corr_case_documentation
   corr_pair_documentation
   install

.. Indices and tables
.. ==================

.. * :ref:`genindex`
.. * :ref:`modindex`
.. * :ref:`search`
