************************
predict()
************************

Description
===========
Calculates linear predictions, various residuals, leverage statistics, distance statistics, and others.


Parameters
==========

Input
-----
**predict(mdl_data = {}, estimate = None)**

  * **mdl_data** : Class object from ols or anova
  * **estimate** : Desired estimate. Available options are:

    * *"y"* or *"xb"* : Linear prediction
    * *"residuals"*, *"res"*, or *"r"* : Residuals
    * *"standardized_residuals"*, *"standardized_r"*, or *"r_std"* : Standardized residuals
    * *"studentized_residuals"*, *"student_r"*, or *"r_stud"* : Studentized (jackknifed) residuals



Returns
-------
Numpy array with n x 1 dimensions; where n is the number of observations



Formulas
==================


Linear prediction :footcite:p:`kutner2005`
----------------------------------------------
The linear prediction is calculated as:

.. math::

  y_e = IV @ \text{betas}


Residuals :footcite:p:`kutner2005`
----------------------------------------------
Residuals are calculated as:

.. math::

  \text{e} = y_{obs} - y_e


Standardized residuals :footcite:p:`kutner2005`
--------------------------------------------------
Standardized residuals are calculated using the following formula:

.. math::

  t_i = \frac{e_i}{\sqrt{\text{MSE} * (1 - H_{ii})}}


Studentized (jackknifed) residuals :footcite:p:`kutner2005`
-------------------------------------------------------------
Studentized (jackknifed) residuals are calculated using the following formula:

.. math::

  t_i = r_i * (\frac{n - k -2}{n - k - 1 - r_i^2})^\frac{1}{2}






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

    <div style="overflow-x: auto;">
    <table class="dataframe">
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

    <div style="overflow-x: auto;">
    <table class="dataframe">
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

    <div style="overflow-x: auto;">
    <table class="dataframe">
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

    <div style="overflow-x: auto;">
    <table class="dataframe">
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

    <div style="overflow-x: auto;">
    <table class="dataframe">
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

    <div style="overflow-x: auto;">
    <table class="dataframe">
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

    <div style="overflow-x: auto;">
    <table class="dataframe">
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
===========

.. footbibliography::
