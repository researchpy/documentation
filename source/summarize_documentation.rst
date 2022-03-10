*************
summarize()
*************

Description
===========
Calculates univariate descriptive statistics and returns the information as either a
Pandas DataFrame object (the default) or Python dictionary object.

The following calculations are available:

  * count of non-missing (N),
  * the average (Mean),
  * the median (Median),
  * variance (Variance),
  * standard deviation (SD),
  * standard error (SE),
  * confidence interval (CI),
  * minimum value (Min),
  * maximum value (Max),
  * range (Range),
  * the kurtosis (Kurtosis), and
  * the skew (Skew).

If no univariate descriptive statistics are specified, by default the N, Mean, Median,
Variance, SD, SE, and 95% confidence interval are calculated.


Parameters
==========

Input
-----
**summarize(data = {}, name = None, stats = [], ci_level = 0.95, decimals = 4, return_type = "Dataframe")**

  * **data** : The Pandas DataFrame or array_like object which contains the data to be analyzed.
  * **stats** : The univariate statistics to be calculated entered as a list; the default is ["N", "Mean", "Median", "Variance", "SD", "SE", "CI"].
  * **ci_level** : The confidence interval to be calculated; the default is 0.95, i.e., 95% confidence interval.
  * **decimals** : The rounding to be applied to the data; the default is 4.
  * **return_type** : The data structure to be returned; available options are "Dataframe" or "Dictionary" with the default being "Dataframe".

Returns
-------
Pandas DataFrame or Python dictionary object containing the univariate descriptive statistics.


Examples
========
First to load required libraries for this example. Below, an example data set will be loaded
in using statsmodels.datasets; the data loaded in is a data set available through Stata
called 'auto'.

.. code:: python

 import researchpy as rp
  import pandas as pd
  # Used to load example data #
  import statsmodels.datasets

  auto = statsmodels.datasets.webuse('auto')
  auto.info()


.. .. parsed-literal::

  <class 'pandas.core.frame.DataFrame'>
  Int64Index: 74 entries, 0 to 73
  Data columns (total 12 columns):
  #   Column        Non-Null Count  Dtype
  ---  ------        --------------  -----
  0   make          74 non-null     object
  1   price         74 non-null     int16
  2   mpg           74 non-null     int16
  3   rep78         69 non-null     float64
  4   headroom      74 non-null     float32
  5   trunk         74 non-null     int16
  6   weight        74 non-null     int16
  7   length        74 non-null     int16
  8   turn          74 non-null     int16
  9   displacement  74 non-null     int16
  10  gear_ratio    74 non-null     float32
  11  foreign       74 non-null     category
  dtypes: category(1), float32(2), float64(1), int16(7), object(1)
  memory usage: 3.5+ KB
