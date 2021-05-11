difference_test()
=================
Conducts a few different statistical tests which test for a difference between
independent or related samples with or without equal variances and has the ability
to calculate the effect size of the observed difference. The data is
returned in a Pandas DataFrame by default, but can be returned as a dictionary
if specified.

This method is similar to researchpy.ttest(), except it allows the user to use
the formula syntax.

This method can perform the following tests:
  * Independent sample t-test :cite:`scipy_ttest_ind`

      * `psudo-code: difference_test(formula_like, data, equal_variances = True, independent_samples = True)`

  * Paired sample t-test :cite:`scipy_ttest_rel`

      * `psudo-code: difference_test(formula_like, data, equal_variances = True, independent_samples = False)`

  * Welch's t-test :cite:`scipy_ttest_ind`

      * `psudo-code: difference_test(formula_like, data, equal_variances = False, independent_samples = True)`

  * Wilcoxon ranked-sign test :cite:`scipy_wilcoxon`

      * By default, discards all zero-differences; this is known as the 'wilcox' method.
      * `psudo-code: difference_test(formula_like, data, equal_variances = False, independent_samples = False)`

2 objects will be returned for all available tests; the first object will be a
descriptive summary table and the second will be the testing result information which
will include the effect size measures if indicated.





Arguments
-----------------
**difference_test(formula_like, data = {}, conf_level = 0.95, equal_variances = True, independent_samples = True,
                  wilcox_parameters = {"zero_method" : "wilcox", "correction" : False, "mode" : "auto"}, **keywords)**

  * **formula_like**: A valid `formula < https://patsy.readthedocs.io/en/latest/formulas.html >`_ ; for example, "DV ~ IV".
  * **data**: data to perform the analysis on - contains the dependent and independent variables.
  * **conf_level**: Specify the confidence interval to be calculated.
  * **equal_variances**: Boolean to indicate if equal variances are assumed.
  * **independent_samples**: Boolean to indicate if groups and independent of each other.
  * **wilcox_parameters**: A dictionary with optional methods for calculating the Wilcoxon signed-rank test. For more information, see `scipy.stats.wilcoxon <https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.wilcoxon.html#scipy.stats.wilcoxon>`.

**conduct(return_type = "Dataframe", effect_size = None)**

  * **return_type**: Specify if the results should be returned as a Pandas DataFrame (default) or a Python dictionary (= 'Dictionary').
  * **effect_size**: Specify if effect sizes should be calculated, default value is None.
    * Available options are: None, "Cohen's D", "Hedge's G", "Glass's delta1", "Glass's delta2", "r", and/or "all".
      * User can specify any combination of effect sizes, or use "all" which will calculate all effect sizes.
      * Only effect size "r" is supported for the Wilcoxon ranked-sign test.





**returns**

  * 2 objects will be returned within a tuple:
      * First object will be the descriptive summary information.
      * Second object will be the statistical testing information.





.. note::
    This can be a one step, or two step process.

    **One step**
    .. code:: python

        difference_test("DV ~ IV", data).conduct()

    **Two step**
    .. code:: python

        model = difference_test("DV ~ IV", data)
        model.conduct()





Effect size measures formulas
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Cohen's d\ :sub:`s` (between subjects design)
""""""""""""""""""""""""""""""""""""""""""""""
Cohen's d\ :sub:`s` :cite:`cohen1988` for a between groups design is calculated
with the following equation:

.. math::

  d_s = \frac{\bar{x}_1 - \bar{x}_2}{\sqrt{\frac{(n_1 - 1)SD^2_1 + (n_2 - 1)SD^2_2}{n_1 + n_2 - 2}}}





Cohen's d\ :sub:`av` (within subject design)
"""""""""""""""""""""""""""""""""""""""""""
Another version of Cohen's d is used in within subject designs. This is noted
by the subscript "av". The formula for Cohen's d\ :sub:`av` :cite:`lakens2013` is
as follows:

.. math::

  d_{av} = \frac{M_{diff}}{\frac{SD_{1} + SD_{2}}{2}}





Hedges's g\ :sub:`s` (between subjects design)
""""""""""""""""""""""""""""""""""""""""""""""""
Cohen's d\ :sub:`s` gives a biased estimate of the effect size for a population
and Hedges and Olkin :cite:`hedges1985` provides an unbiased estimation. The
differences between Hedges's g and Cohen's d is negligible when sample sizes
are above 20, but it is still preferable to report Hedges's g :cite:`kline2004`.
Hedge's g\ :sub:`s` is calculated using the following formula:

.. math::

  \text{Hedges's g}_s = \text{Cohen's d}_s \times (1 - \frac{3}{4(n_1 + n_2 - 9)})





Hedges's g\ :sub:`av` (within subjects design)
""""""""""""""""""""""""""""""""""""""""""""""""
Cohen's d\ :sub:`av` gives a biased estimate of the effect size for a population
and Hedges and Olkin :cite:`hedges1985` provides a correction to be applied to provide an unbiased estimate.
Hedge's g\ :sub:`av` is calculated using the following formula :cite:`lakens2013` :

.. math::

  \text{Hedges's g}_{av} = \text{Cohen's d}_av \times (1 - \frac{3}{4(n_1 + n_2 - 9)})



Glass's :math:`\Delta` (between or within subjects design)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
Glass's :math:`\Delta` is the mean differences between the two groups divided by
the standard deviation of the first condition/group or by the second condition/group.
When used in a within subjects design, it is recommended to use the pre- standard
deviation in the denominator :cite:`lakens2013`; the following formulas are used
to calculate Glass's :math:`\Delta`:

.. math::

  \Delta_1 = \frac{(\bar{x}_1 - \bar{x}_2)}{SD_1}

  \Delta_2 = \frac{(\bar{x}_1 - \bar{x}_2)}{SD_2}



Point-Biserial correlation coefficient r (between or within subjects design)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
Rosenthal :cite:`rosenthal1991` provided the following formula to calculate
the Pearson correlation coefficient r using the t-value and degrees of freedom:

.. math::

  r = \sqrt{\frac{t}{t^2 + df}}

The following formula is used to calculate the Point-Biserial
correlation coefficient r using the W-value and N. This formula
is used to calculate the r coefficient for the Wilcoxon ranked-sign test.

  .. math::

    r = \sqrt{\frac{W}{\sum{\text{rank}}}}





Examples
--------
First let's create an example data set to work through the examples. This will be done using
numpy (to create fake data) and pandas (to hold the data in a data frame).

.. code:: python

    import numpy, pandas, researchpy

    numpy.random.seed(12345678)

    df = pandas.DataFrame(numpy.random.randint(10, size= (100, 2)),
                      columns= ['No', 'Yes'])

    df["id"] = range(1, df.shape[0] + 1)

    df.head()

.. parsed-literal::

    No  Yes  id
    3    2   1
    4    1   2
    0    1   3
    8    2   4
    6    6   5

If one has data like this and doesn't want to reshape the data, then *researchpy.different_test()* will not work and
one should use *researchpy.ttest()* instead. However, moving forward researchpy will be going in the
direction of syntax style input and it is recommended to get comfortable using this
approach if one plans to use researchpy in the future.

Currently the data is in a wide format and it needs to be in a long format, i.e. one variable
with the dependent variable data and another with the independent variable data. The current data
structure won't work and it needs to be reshaped; there are a few ways to do this, one
will be shown below.

.. code-block:: python

    df2 = pandas.melt(df, id_vars = "id", value_vars = ["No", "Yes"],
                      var_name = "Exercise", value_name = "StressReactivity")

    df2.head()

.. parsed-literal::

    id Exercise  StressReactivity
    1       No                 3
    2       No                 4
    3       No                 0
    4       No                 8
    5       No                 6

Now the data is in the correct structure.


.. code:: python

    # Independent t-test

    # If you don't store the 2 returned DataFrames, it outputs as a tuple and
    # is displayed
    difference_test("StressReactivity ~ C(Exercise)",
                    data = df2,
                    equal_variances = True,
                    independent_samples = True).conduct(effect_size = "all")

.. parsed-literal::

    (      Variable      N   Mean        SD        SE  95% Conf.  Interval
     0      healthy  100.0  4.590  2.749086  0.274909   4.044522  5.135478
     1  non-healthy  100.0  4.160  3.132495  0.313250   3.538445  4.781555
     2     combined  200.0  4.375  2.947510  0.208420   3.964004  4.785996,
                                      Independent t-test   results
     0             Difference (healthy - non-healthy) =     0.4300
     1                             Degrees of freedom =   198.0000
     2                                              t =     1.0317
     3                          Two side test p value =     0.3035
     4                         Difference < 0 p value =     0.8483
     5                         Difference > 0 p value =     0.1517
     6                                      Cohen's d =     0.1459
     7                                      Hedge's g =     0.1454
     8                                  Glass's delta =     0.1564
     9                                              r =     0.0731)



.. code:: python

    # Otherwise you can store them as objects
    summary, results = difference_test("StressReactivity ~ C(Exercise)",
                                       data = df2,
                                       equal_variances = True,
                                       independent_samples = True).conduct(effect_size = "all")

    summary

.. parsed-literal::

           Name    N   Mean Variance       SD        SE  95% Conf.  Interval
    0        No  100  4.590  7.55747  2.74909  0.274909   4.044522  5.135478
    1       Yes  100  4.160  9.81253   3.1325  0.313250   3.538445  4.781555
    2  combined  200  4.375  8.68781  2.94751  0.208420   3.964004  4.785996
    3      diff       0.430                    0.416773  -0.391884  1.251884



.. code:: python

    results

.. parsed-literal::

       Independent samples t-test     Results
    0       Difference (No - Yes)    0.430000
    1        Degrees of freedom =  198.000000
    2                         t =    1.031736
    3    Two sided test p-value =    0.303454
    4    Difference < 0 p-value =    0.848273
    5    Difference > 0 p-value =    0.151727
    6                  Cohen's Ds    0.145909
    7                   Hedge's G    0.145356
    8              Glass's delta1    0.156416
    9              Glass's delta2    0.137271
    10           Point-Biserial r    0.073126



.. code:: python

    # Paired samples t-test
    summary, results = difference_test("StressReactivity ~ C(Exercise)",
                                       data = df2,
                                       equal_variances = True,
                                       independent_samples = False).conduct(effect_size = "all")

    summary

.. parsed-literal::

       Name    N  Mean Variance        SD        SE  95% Conf.  Interval
    0    No  100  4.59  7.55747  2.749086  0.274909   4.044522  5.135478
    1   Yes  100  4.16  9.81253  3.132495  0.313250   3.538445  4.781555
    3  diff       0.43           4.063275  0.406327  -0.376242  1.236242



.. code:: python

    results

.. parsed-literal::

           Paired samples t-test    Results
    0      Difference (No - Yes)   0.430000
    1       Degrees of freedom =  99.000000
    2                        t =   1.058260
    3   Two sided test p-value =   0.292512
    4   Difference < 0 p-value =   0.853744
    5   Difference > 0 p-value =   0.146256
    6                Cohen's Dav   0.146219
    7                Hedge's Gav   0.145665
    8             Glass's delta1   0.156416
    9             Glass's delta2   0.137271
    10          Point-Biserial r   0.105763



.. code:: python

    # Welch's t-test
    summary, results = difference_test("StressReactivity ~ C(Exercise)",
                                       data = df2,
                                       equal_variances = False,
                                       independent_samples = True).conduct(effect_size = "all")

    summary

.. parsed-literal::

           Name    N   Mean Variance       SD        SE  95% Conf.  Interval
    0        No  100  4.590  7.55747  2.74909  0.274909   4.044522  5.135478
    1       Yes  100  4.160  9.81253   3.1325  0.313250   3.538445  4.781555
    2  combined  200  4.375  8.68781  2.94751  0.208420   3.964004  4.785996
    3      diff       0.430                    0.416773  -0.391919  1.251919



.. code:: python

    results

.. parsed-literal::

                  Welch's t-test     Results
    0      Difference (No - Yes)    0.430000
    1       Degrees of freedom =  196.651845
    2                        t =    1.031736
    3   Two sided test p-value =    0.303476
    4   Difference < 0 p-value =    0.848268
    5   Difference > 0 p-value =    0.151732
    6                 Cohen's Ds    0.145909
    7                  Hedge's G    0.145356
    8             Glass's delta1    0.156416
    9             Glass's delta2    0.137271
    10          Point-Biserial r    0.073375



.. code:: python

    # Wilcoxon signed-rank test
    summary, results = difference_test("StressReactivity ~ C(Exercise)",
                                       data = df2,
                                       equal_variances = False,
                                       independent_samples = False).conduct(effect_size = "r")

    summary

.. parsed-literal::

      Name    N  Mean Variance       SD        SE  95% Conf.  Interval
    0   No  100  4.59  7.55747  2.74909  0.274909   4.044522  5.135478
    1  Yes  100  4.16  9.81253   3.1325  0.313250   3.538445  4.781555

.. code:: python

    results

.. parsed-literal::

      Wilcoxon signed-rank test   Results
    0                (No = Yes)
    1                       W =    1849.5
    2       Two sided p-value =  0.333755
    3          Point-Biserial r  0.366238


.. code:: python

    # Exporting descriptive table (summary) and result table (results) to same
    # csv file
    summary.to_csv("C:\\Users\\...\\test.csv", index= False)
    results.to_csv("C:\\Users\\...\\test.csv", index= False, mode= 'a')





References
----------
.. bibliography:: difference_test.bib
  :style: plain
