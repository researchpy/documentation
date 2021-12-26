*******
anova()
*******

Performs the analysis-of-variance (ANOVA) and analysis-of-covariance (ANCOVA).






Arguments
---------

**anova(self, formula_like, data = {}, sum_of_squares = 3, conf_level = 0.95)**

  * **formula_like** : A valid formula which will parse the data into a design matrix.
  * **data** : The dataframe which contains the data to be analyzed.
  * **sum_of_squares** : The type of sum of squares which is desired, the default it Type 3.
  * **conf_level** : The confidence interval desired.


Returned
--------
  * A data object with class "anova". This object has accessible methods.

anova methods
^^^^^^^^^^^^^

  * **results(return_type = "Dataframe", decimals = 4, pretty_format = True)**
    * **return_type** : The type of data structre the results should be returned as. Supported options
    are 'Dataframe' which will return a Pandas DataFrame or 'Dictionary' which will return a dictionary.
    * **decimals** : The number of decimal places the data should be rounded too.
    * **pretty_format ** : If pretty formatting should be applied. This adds extra empty spaces in the returned data
    structure for visualization of the results.



Examples
--------
First to load required libraries,

.. code:: python

  import pandas
   import statsmodels.datasets
   import researchpy as rp

   systolic = statsmodels.datasets.webuse('systolic')


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
