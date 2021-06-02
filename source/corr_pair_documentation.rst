corr_pair()
===========
Conducts Pearson (default method), Spearman rank, or Kendall's Tau-b correlation analysis using
pair wise deletion. Returns the relevant information and results in 1 DataFrame
for easy exporting.

DataFrame 1 contains the variables being compared in the index, followed by the
corresponding r value, p-value, and N for the groups being compared.



Arguments
---------
**corr_pair(dataframe, method= "pearson")**

* **dataframe** can either be a single Pandas Series or multiple Series/an
  entire DataFrame.
* **method** takes the values of "pearson" :cite:`scipy_pearsonr` (the default if nothing is passed),
  "spearman" :cite:`scipy_spearmanr`, or "kendall" :cite:`scipy_kendalltau`.

..  scipy.stats methods used in corr_case()
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. * For `Pearson correlation`_
.. * For `Spearman correlation`_
.. * For `Kendall Tau-b`_

.. _Pearson correlation: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.pearsonr.html
.. _Spearman correlation: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html
.. _Kendall Tau-b: https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kendalltau.html


Examples
--------
.. code:: python

    import researchpy, numpy, pandas


    numpy.random.seed(12345)

    df = pandas.DataFrame(numpy.random.randint(10, size= (100, 4)),
                      columns= ['mental_score', 'physical_score', 'emotional_score',
                               'happiness_index'])

.. code:: python

    # Can pass the entire DataFrame or multiple Series

    researchpy.correlation.corr_pair(df)

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>r value</th>
          <th>p-value</th>
          <th>N</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>mental_score &amp; physical_score</th>
          <td>0.0557</td>
          <td>0.5823</td>
          <td>100</td>
        </tr>
        <tr>
          <th>mental_score &amp; emotional_score</th>
          <td>-0.0237</td>
          <td>0.8153</td>
          <td>100</td>
        </tr>
        <tr>
          <th>mental_score &amp; happiness_index</th>
          <td>0.1360</td>
          <td>0.1773</td>
          <td>100</td>
        </tr>
        <tr>
          <th>physical_score &amp; emotional_score</th>
          <td>0.0580</td>
          <td>0.5663</td>
          <td>100</td>
        </tr>
        <tr>
          <th>physical_score &amp; happiness_index</th>
          <td>-0.1366</td>
          <td>0.1754</td>
          <td>100</td>
        </tr>
        <tr>
          <th>emotional_score &amp; happiness_index</th>
          <td>-0.0632</td>
          <td>0.5323</td>
          <td>100</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: python

    # Demonstrating how the output looks if there are different Ns for groups
    df['happiness_index'][0:30] = numpy.nan

    researchpy.correlation.corr_pair(df)

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>r value</th>
          <th>p-value</th>
          <th>N</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>mental_score &amp; physical_score</th>
          <td>0.0557</td>
          <td>0.5823</td>
          <td>100</td>
        </tr>
        <tr>
          <th>mental_score &amp; emotional_score</th>
          <td>-0.0237</td>
          <td>0.8153</td>
          <td>100</td>
        </tr>
        <tr>
          <th>mental_score &amp; happiness_index</th>
          <td>0.0933</td>
          <td>0.4423</td>
          <td>70</td>
        </tr>
        <tr>
          <th>physical_score &amp; emotional_score</th>
          <td>0.0580</td>
          <td>0.5663</td>
          <td>100</td>
        </tr>
        <tr>
          <th>physical_score &amp; happiness_index</th>
          <td>-0.0268</td>
          <td>0.8254</td>
          <td>70</td>
        </tr>
        <tr>
          <th>emotional_score &amp; happiness_index</th>
          <td>-0.0873</td>
          <td>0.4726</td>
          <td>70</td>
        </tr>
      </tbody>
    </table>
    </div>



References
----------
.. bibliography:: ref.bib
   :cited:
   :list: bullet
