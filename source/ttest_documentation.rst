*******
ttest()
*******

Description
===========
Conducts various comparison tests between two groups and returns data tables as
Pandas DataFrames with relevant information pertaining to the statistical test conducted.

This method can perform the following tests:
  * Independent sample t-test :cite:`scipy_ttest_ind`

      * `psudo-code: ttest(group1, group2, equal_variances = True, paired = False)`

  * Paired sample t-test :cite:`scipy_ttest_rel`

      * `psudo-code: ttest(group1, group2, equal_variances = True, paired = True)`

  * Welch's t-test :cite:`scipy_ttest_ind`

      * `psudo-code: ttest(group1, group2, equal_variances = False, paired = False)`

  * Wilcoxon ranked-sign test :cite:`scipy_wilcoxon`

      * `psudo-code: ttest(group1, group2, equal_variances = False, paired = True)`



.. note:: **Deprecation Warning**

    This function is being deprecated in the future during the updating and streamlining of the package.





Parameters
==========

Input
-----
**ttest(group1, group2, group1_name= None, group2_name= None, equal_variances= True, paired= False, wilcox_parameters = {"zero_method" : "pratt", "correction" : False, "mode" : "auto"})**

  * **group1** and **group2** : requires the data to be a Pandas Series
  * **group1_name** and **group2_name** : will override the series name
  * **equal_variances** : tells whether equal variances is assumed or not.
      If not, Welch's t-test is used if data is unpaired, or Wilcoxon
      rank-signed test is used if data is paired. The default is True.
  * **paired** : tells whether the data is paired. If data is paired and equal
      variance is assumed, a paired sample t-test is conducted, otherwise a Wilcoxon
      ranked-sign test is conducted. The default is False.
  * **wilcox_parameters** : A dictionary which contains the testing specifications for the Wilcoxon signed-rank test.


Returns
-------
Will return 2 Pandas DataFrames (default) as a tuple. The first returned DataFrame
will contain the summary statistics while the second returned DataFrame contains the test results.


DataFrame 1
^^^^^^^^^^^
(All except Wilcoxon signed-rank test) has summary statistic information including variable name, total
number of non-missing observations, standard deviation, standard error, and
the 95% confidence interval. This is the same information returned from the
*summary_cont()* method.

For the Wilcoxon signed-rank test, this will contain descriptive information regarding
the signed-rank.


DataFrame 2
^^^^^^^^^^^
(All except Wilcoxon signed-rank test) has the test results for the
statistical tests. Included in this is an effect size measures of r, Cohen's d,
Hedge's g, and Glass's :math:`\Delta` for the independent sample t-test,
paired sample t-test, and Welch's t-test.

For the Wilcoxon signed-rank test, the returned DataFrame contains the mean
for both comparison points, the W-statistic, the Z-statistic, the two-sided p-value, and
effect size measures of Pearson r and Rank-Biserial r.



Effect size measures formulas
=============================

Cohen's d\ :sub:`s` (between subjects design)
---------------------------------------------
Cohen's d\ :sub:`s` :cite:`cohen1988` for a between groups design is calculated
with the following equation:

.. math::

  d_s = \frac{\bar{x}_1 - \bar{x}_2}{\sqrt{\frac{(n_1 - 1)SD^2_1 + (n_2 - 1)SD^2_2}{n_1 + n_2 - 2}}}



Hedges's g\ :sub:`s` (between subjects design)
----------------------------------------------
Cohen's d\ :sub:`s` gives a biased estimate of the effect size for a population
and Hedges and Olkin :cite:`hedges1985` provides an unbiased estimation. The
differences between Hedges's g and Cohen's d is negligible when sample sizes
are above 20, but it is still preferable to report Hedges's g :cite:`kline2004`.
Hedge's g\ :sub:`s` is calculated using the following formula:

.. math::

  \text{Hedges's g}_s = \text{Cohen's d}_s \times (1 - \frac{3}{4(n_1 + n_2 - 9)})



Glass's :math:`\Delta` (between or within subjects design)
----------------------------------------------------------
Glass's :math:`\Delta` is the mean differences between the two groups divided by
the standard deviation of the control group. When used in a within subjects
design, it is recommended to use the pre- standard deviation in the denominator
:cite:`lakens2013`; the following formula is used to calculate Glass's
:math:`\Delta`:

.. math::

  \Delta = \frac{(\bar{x}_1 - \bar{x}_2)}{SD_1}



Cohen's d\ :sub:`z` (within subject design)
-------------------------------------------
Another version of Cohen's d is used in within subject designs. This is noted
by the subscript "z". The formula for Cohen's d\ :sub:`z` :cite:`cohen1988` is
as follows:

.. math::

  d_z = \frac{M_{diff}}{\sqrt{\frac{\sum (X_{diff} - M_{diff})^2}{N - 1}}}



Pearson correlation coefficient r (between or within subjects design)
---------------------------------------------------------------------
Rosenthal :cite:`rosenthal1991` provided the following formula to calculate
the Pearson correlation coefficient r using the t-value and degrees of freedom:

.. math::

  r = \sqrt{\frac{t^2}{t^2 + df}}

Rosenthal :cite:`rosenthal1991` provided the following formula to calculate
the Pearson correlation coefficient r using the z-value and N. This formula
is used to calculate the r coefficient for the Wilcoxon ranked-sign test. Note,
that N is the total number of observations.

.. math::

    r = \frac{Z}{\sqrt{N}}


Rank-Biserial correlation coefficient r (between or within subjects design)
---------------------------------------------------------------------------
The Rank-Biserial r is also provided for the Wilcoxon signed-rank test as is
calculated as:

.. math::

  \text{Rank-Biserial r} = \frac{W}{\sum{\text{rank}}}





Examples
========
.. code:: python

    import numpy, pandas, researchpy

    numpy.random.seed(12345678)

    df = pandas.DataFrame(numpy.random.randint(10, size= (100, 2)),
                      columns= ['healthy', 'non-healthy'])

Independent t-test
------------------
.. code:: python

    # Independent t-test

    # If you don't store the 2 returned DataFrames, it outputs as a tuple and
    # is displayed
    researchpy.ttest(df['healthy'], df['non-healthy'])

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
    des, res = researchpy.ttest(df['healthy'], df['non-healthy'])

    des

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>N</th>
          <th>Mean</th>
          <th>SD</th>
          <th>SE</th>
          <th>95% Conf.</th>
          <th>Interval</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>healthy</td>
          <td>100.0</td>
          <td>4.590</td>
          <td>2.749086</td>
          <td>0.274909</td>
          <td>4.044522</td>
          <td>5.135478</td>
        </tr>
        <tr>
          <th>1</th>
          <td>non-healthy</td>
          <td>100.0</td>
          <td>4.160</td>
          <td>3.132495</td>
          <td>0.313250</td>
          <td>3.538445</td>
          <td>4.781555</td>
        </tr>
        <tr>
          <th>2</th>
          <td>combined</td>
          <td>200.0</td>
          <td>4.375</td>
          <td>2.947510</td>
          <td>0.208420</td>
          <td>3.964004</td>
          <td>4.785996</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: python

    res

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Independent t-test</th>
          <th>results</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>Difference (healthy - non-healthy) =</td>
          <td>0.4300</td>
        </tr>
        <tr>
          <th>1</th>
          <td>Degrees of freedom =</td>
          <td>198.0000</td>
        </tr>
        <tr>
          <th>2</th>
          <td>t =</td>
          <td>1.0317</td>
        </tr>
        <tr>
          <th>3</th>
          <td>Two side test p value =</td>
          <td>0.3035</td>
        </tr>
        <tr>
          <th>4</th>
          <td>Difference < 0 p value =</td>
          <td>0.8483</td>
        </tr>
        <tr>
          <th>5</th>
          <td>Difference > 0 p value =</td>
          <td>0.1517</td>
        </tr>
        <tr>
          <th>6</th>
          <td>Cohen's d =</td>
          <td>0.1459</td>
        </tr>
        <tr>
          <th>7</th>
          <td>Hedge's g =</td>
          <td>0.1454</td>
        </tr>
        <tr>
          <th>8</th>
          <td>Glass's delta =</td>
          <td>0.1564</td>
        </tr>
        <tr>
          <th>9</th>
          <td>r =</td>
          <td>0.0731</td>
        </tr>
      </tbody>
    </table>
    </div>


Paired Sample t-test
--------------------
.. code:: python

    # Paired samples t-test
    des, res = researchpy.ttest(df['healthy'], df['non-healthy'],
                                paired= True)

    des

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>N</th>
          <th>Mean</th>
          <th>SD</th>
          <th>SE</th>
          <th>95% Conf.</th>
          <th>Interval</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>healthy</td>
          <td>100.0</td>
          <td>4.59</td>
          <td>2.749086</td>
          <td>0.274909</td>
          <td>4.044522</td>
          <td>5.135478</td>
        </tr>
        <tr>
          <th>1</th>
          <td>non-healthy</td>
          <td>100.0</td>
          <td>4.16</td>
          <td>3.132495</td>
          <td>0.313250</td>
          <td>3.538445</td>
          <td>4.781555</td>
        </tr>
        <tr>
          <th>2</th>
          <td>diff</td>
          <td>100.0</td>
          <td>0.43</td>
          <td>4.063275</td>
          <td>0.406327</td>
          <td>-0.376242</td>
          <td>1.236242</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: python

    res

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Paired samples t-test</th>
          <th>results</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>Difference (healthy - non-healthy) =</td>
          <td>0.4300</td>
        </tr>
        <tr>
          <th>1</th>
          <td>Degrees of freedom =</td>
          <td>99.0000</td>
        </tr>
        <tr>
          <th>2</th>
          <td>t =</td>
          <td>1.0583</td>
        </tr>
        <tr>
          <th>3</th>
          <td>Two side test p value =</td>
          <td>0.2925</td>
        </tr>
        <tr>
          <th>4</th>
          <td>Difference < 0 p value =</td>
          <td>0.8537</td>
        </tr>
        <tr>
          <th>5</th>
          <td>Difference > 0 p value =</td>
          <td>0.1463</td>
        </tr>
        <tr>
          <th>6</th>
          <td>Cohen's d =</td>
          <td>0.1058</td>
        </tr>
        <tr>
          <th>7</th>
          <td>Hedge's g =</td>
          <td>0.1054</td>
        </tr>
        <tr>
          <th>8</th>
          <td>Glass's delta =</td>
          <td>0.1564</td>
        </tr>
        <tr>
          <th>9</th>
          <td>r =</td>
          <td>0.1058</td>
        </tr>
      </tbody>
    </table>
    </div>


Welch's t-test
--------------
.. code:: python

    # Welch's t-test
    des, res = researchpy.ttest(df['healthy'], df['non-healthy'],
                                equal_variances= False)

    des

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>N</th>
          <th>Mean</th>
          <th>SD</th>
          <th>SE</th>
          <th>95% Conf.</th>
          <th>Interval</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>healthy</td>
          <td>100.0</td>
          <td>4.590</td>
          <td>2.749086</td>
          <td>0.274909</td>
          <td>4.044522</td>
          <td>5.135478</td>
        </tr>
        <tr>
          <th>1</th>
          <td>non-healthy</td>
          <td>100.0</td>
          <td>4.160</td>
          <td>3.132495</td>
          <td>0.313250</td>
          <td>3.538445</td>
          <td>4.781555</td>
        </tr>
        <tr>
          <th>2</th>
          <td>combined</td>
          <td>200.0</td>
          <td>4.375</td>
          <td>2.947510</td>
          <td>0.208420</td>
          <td>3.964004</td>
          <td>4.785996</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: python

    res

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Welch's t-test</th>
          <th>results</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>Difference (healthy - non-healthy) =</td>
          <td>0.4300</td>
        </tr>
        <tr>
          <th>1</th>
          <td>Degrees of freedom =</td>
          <td>194.7181</td>
        </tr>
        <tr>
          <th>2</th>
          <td>t =</td>
          <td>1.0317</td>
        </tr>
        <tr>
          <th>3</th>
          <td>Two side test p value =</td>
          <td>0.3035</td>
        </tr>
        <tr>
          <th>4</th>
          <td>Difference < 0 p value =</td>
          <td>0.8483</td>
        </tr>
        <tr>
          <th>5</th>
          <td>Difference > 0 p value =</td>
          <td>0.1517</td>
        </tr>
        <tr>
          <th>6</th>
          <td>Cohen's d =</td>
          <td>0.1459</td>
        </tr>
        <tr>
          <th>7</th>
          <td>Hedge's g =</td>
          <td>0.1454</td>
        </tr>
        <tr>
          <th>8</th>
          <td>Glass's delta =</td>
          <td>0.1564</td>
        </tr>
        <tr>
          <th>9</th>
          <td>r =</td>
          <td>0.0737</td>
        </tr>
      </tbody>
    </table>
    </div>


Wilcoxon Signed-Rank Test
-------------------------
.. code:: python

    # Wilcoxon signed-rank test
    researchpy.ttest(df['healthy'], df['non-healthy'],
                     equal_variances= False, paired= True)

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Wilcoxon signed-rank test</th>
          <th>results</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>Mean for healthy =</td>
          <td>4.5900</td>
        </tr>
        <tr>
          <th>1</th>
          <td>Mean for non-healthy =</td>
          <td>4.1600</td>
        </tr>
        <tr>
          <th>2</th>
          <td>T value =</td>
          <td>1849.5000</td>
        </tr>
        <tr>
          <th>3</th>
          <td>Z value =</td>
          <td>-0.9638</td>
        </tr>
        <tr>
          <th>4</th>
          <td>Two sided p value =</td>
          <td>0.3347</td>
        </tr>
        <tr>
          <th>5</th>
          <td>r =</td>
          <td>-0.0681</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: python

    # Exporting descriptive table (des) and result table (res) to same
    # csv file
    des, res = researchpy.ttest(df['healthy'], df['non-healthy'])

    des.to_csv("C:\\Users\\...\\test.csv", index= False)
    res.to_csv("C:\\Users\\...\\test.csv", index= False, mode= 'a')





References
----------
.. bibliography:: refs.bib
   :cited:
   :list: bullet
