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


Now to take a look at the descriptive statistics of the univariate data.

.. code:: python

  rp.summarize(systolic["systolic"])


.. parsed-literal::

  Name   N     Mean Median Variance       SD      SE  95% Conf. Interval
   0  systolic  58  18.8793     21  163.862  12.8009  1.6808  [15.5135, 22.2451]



.. code:: python

  rp.summary_cat(systolic[["drug", "disease"]])


.. parsed-literal::

  Variable  Outcome  Count  Percent
   0     drug        4     16    27.59
   1                 2     15    25.86
   2                 1     15    25.86
   3                 3     12    20.69
   4  disease        3     20    34.48
   5                 2     19    32.76
   6                 1     19    32.76
