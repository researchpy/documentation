*************
anova()
*************

Description
===========
Performs the analysis-of-variance (ANOVA) and analysis-of-covariance (ANCOVA).



Parameters
==========

Input
-----
**anova(self, formula_like, data = {}, sum_of_squares = 3, conf_level = 0.95)**

  * **formula_like** : A valid formula which will parse the data into a design matrix.
  * **data** : The dataframe which contains the data to be analyzed.
  * **sum_of_squares** : The type of sum of squares which is desired, the default is Type 3.
  * **conf_level** : The confidence interval desired.


Returns
-------
Returns an object with class "anova"; this object has accessible methods which are
described below.

anova methods
^^^^^^^^^^^^^

  * **results(return_type = "Dataframe", decimals = 4, pretty_format = True)**

      * **return_type** : The type of data structure the results should be returned as. Supported options
      are 'Dataframe' which will return a Pandas DataFrame or 'Dictionary' which will return a dictionary.
      * **decimals** : The number of decimal places the data should be rounded too.
      * **pretty_format ** : If pretty formatting should be applied. This adds extra empty spaces in the returned data
      structure for visualization of the results.



Examples
========
First to load required libraries for this example. Below, an example data set will be loaded
in using statsmodels.datasets; the data loaded in is a data set available through Stata
called 'systolic'.

.. code:: python

  import researchpy as rp
  # Used to load example data #
  import statsmodels.datasets


  systolic = statsmodels.datasets.webuse('systolic')


Now let's get some quick information regarding the data set.

.. code:: python

  systolic.info()


.. parsed-literal::

    <class 'pandas.core.frame.DataFrame'>
     Int64Index: 58 entries, 0 to 57
    Data columns (total 3 columns):
    #   Column    Non-Null Count  Dtype
    ---  ------    --------------  -----
    0   drug      58 non-null     int16
    1   disease   58 non-null     int16
    2   systolic  58 non-null     int16



Now to take a look at the descriptive statistics of the univariate data. The output
indicates that all the columns are integers which is a discrete data type; however,
when we conduct the ANOVA the independent variables, drug and disease, will be
treated as categorical while the dependent variable will be treated as continuous.



.. code:: python

  rp.summarize(systolic["systolic"])

.. raw:: html

  <table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Name</th>      <th>N</th>      <th>Mean</th>      <th>Median</th>      <th>Variance</th>      <th>SD</th>      <th>SE</th>      <th>95% Conf. Interval</th>    </tr>  </thead>
  <tbody>    <tr>      <th>0</th>      <td>systolic</td>      <td>58</td>      <td>18.8793</td>      <td>21</td>      <td>163.862</td>      <td>12.8009</td>      <td>1.6808</td>      <td>[15.5135, 22.2451]</td>    </tr>  </tbody>
  </table>







.. code:: python

  rp.crosstab(systolic["disease"], systolic["drug"])

.. raw:: html

  <table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Variable</th>      <th>Outcome</th>      <th>Count</th>      <th>Percent</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>drug</td>      <td>4</td>      <td>16</td>      <td>27.59</td>    </tr>    <tr>      <th>1</th>      <td></td>      <td>2</td>      <td>15</td>      <td>25.86</td>    </tr>    <tr>      <th>2</th>      <td></td>      <td>1</td>      <td>15</td>      <td>25.86</td>    </tr>    <tr>      <th>3</th>      <td></td>      <td>3</td>      <td>12</td>      <td>20.69</td>    </tr>    <tr>      <th>4</th>      <td>disease</td>      <td>3</td>      <td>20</td>      <td>34.48</td>    </tr>    <tr>      <th>5</th>      <td></td>      <td>2</td>      <td>19</td>      <td>32.76</td>    </tr>    <tr>      <th>6</th>      <td></td>      <td>1</td>      <td>19</td>      <td>32.76</td>    </tr>  </tbody></table>


Now to conduct the ANOVA; by default Type 3 sum of squares are used.


.. code:: python

  mod = anova("systolic ~ C(drug) + C(disease) + C(drug):C(disease)", data = systolic, sum_of_squares = 3)
   mod.results()

.. raw:: html

  <table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Source</th>      <th>Sum of Squares</th>      <th>Degrees of Freedom</th>      <th>Mean Squares</th>      <th>F value</th>      <th>p-value</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>Model</td>      <td>4259.34</td>      <td>11</td>      <td>387.213</td>      <td>3.5057</td>      <td>0.0013</td>    </tr>    <tr>      <th>1</th>      <td></td>      <td></td>      <td></td>      <td></td>      <td></td>      <td></td>    </tr>    <tr>      <th>2</th>      <td>drug</td>      <td>2997.47</td>      <td>3</td>      <td>999.157</td>      <td>9.046</td>      <td>0.0001</td>    </tr>    <tr>      <th>3</th>      <td>disease</td>      <td>415.873</td>      <td>2</td>      <td>207.936</td>      <td>1.8826</td>      <td>0.1637</td>    </tr>    <tr>      <th>4</th>      <td>drug:disease</td>      <td>707.266</td>      <td>6</td>      <td>117.878</td>      <td>1.0672</td>      <td>0.3958</td>    </tr>    <tr>      <th>5</th>      <td></td>      <td></td>      <td></td>      <td></td>      <td></td>      <td></td>    </tr>    <tr>      <th>6</th>      <td>Residual</td>      <td>5080.82</td>      <td>46</td>      <td>110.453</td>      <td></td>      <td></td>    </tr>    <tr>      <th>7</th>      <td>Total</td>      <td>9340.16</td>      <td>57</td>      <td>163.862</td>      <td></td>      <td></td>    </tr>  </tbody></table>
