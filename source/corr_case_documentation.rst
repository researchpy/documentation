corr_case()
===========
Conducts Pearson (default method), Spearman rank, or Kendall's Tau-b correlation analysis using
case wise deletion. Returns the relevant information and results in 3 DataFrames
for easy exporting.

DataFrame 1 is the testing information, i.e. the type of correlation analysis
conducted and the number of observations used.

DataFrame 2 contains the r value results in a matrix style look.

DataFrame 3 contains the p-values in a matrix style look.

Arguments
---------
**def corr_case(dataframe, method = "pearson")**

  * **dataframe** can either be a single Pandas Series or multiple Series/an
    entire DataFrame.
  * **method** takes the values of "pearson" :cite:`scipy_pearsonr` (the default if nothing is passed),
    "spearman" :cite:`scipy_spearmanr`, or "kendall" :cite:`scipy_kendalltau`.

.. scipy.stats methods used in corr_case()
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
..  * For `Pearson correlation`_
..  * For `Spearman correlation`_
.. * For `Kendall Tau-b`_

 .. _Pearson correlation: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.pearsonr.html
 .. _Spearman correlation: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html
 .. _Kendall Tau-b: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kendalltau.html

Examples
--------
.. code:: python

    import researchpy, numpy, pandas


    numpy.random.seed(12345)

    df = pandas.DataFrame(numpy.random.randint(10, size= (100, 2)),
                      columns= ['beck', 'srq'])

.. code:: python

    # Since it returns 3 DataFrames for easy exporting, if the DataFrames
    # aren't assigned to an object the outputting tuple is rather messy
    # and ugly

    researchpy.correlation.corr_case(df[['beck', 'srq']])

.. parsed-literal::

    (  Pearson correlation test using list-wise deletion
     0                     Total observations used = 100,         beck     srq
     beck       1  0.0029
     srq   0.0029       1,         beck     srq
     beck  0.0000  0.9775
     srq   0.9775  0.0000)



.. code:: python

    # As noted above, the 3 return DataFrames are information, r values,
    # and p-values
    # The 3 DataFrame design was decided on so each DataFrame can easily
    # be exported using already supported Pandas methods

    i, r, p  = researchpy.correlation.corr_case(df[['beck', 'srq']])

    i


+--------------------------------------------------+
| Pearson correlation test using list-wise deletion|
+--------------------------------------------------+
| Total observations used = 100                    |
+--------------------------------------------------+

.. code:: python

  r

====  ======  ======
|      beck     srq
====  ======  ======
beck  1       0.0029
srq   0.0029  1
====  ======  ======

.. code:: python

  p

====  ======  ======
|      beck     srq
====  ======  ======
beck  0.0000  0.9775
srq   0.9775  0.0000
====  ======  ======




References
----------
.. bibliography::
   :cited:
   :list: bullet
