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


Now let's take a quick view of the data set.

.. code:: python

  systolic.info()


.. parsed-literal::

         drug  disease  systolic
     0     1        1        42
     1     1        1        44
     2     1        1        36
     3     1        1        13
     4     1        1        19



Now to take a look at the descriptives of the univariate data.

.. code:: python

  rp.summarize(systolic["disease"])


.. .. parsed-literal::

  Name   N    Mean Median Variance     SD      SE 95% Conf. Interval
  0  disease  58  2.0172      2   0.6839  0.827  0.1086   [1.7998, 2.2347]



.. code:: python

  rp.summary_cat(systolic["disease"])


.. code:: python

  rp.summarize(systolic["systolic"])
