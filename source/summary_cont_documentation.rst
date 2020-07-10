summary_cont()
==============
Returns a nice data table as a Pandas DataFrame that includes the variable name,
total number of non-missing observations, standard deviation, standard error,
and the 95% confidence interval. This is compatible with Pandas Series,
DataFrame, and GroupBy objects.

Arguments
----------
**summary_cont(group1, conf = 0.95, decimals = 4)**

  * **group1**, must either be a Pandas Series or DataFrame with multiple
      columns stated
  * **conf**, must be entered in decimal format. The default confidence interval being calculated is at 95%
  * **decimals**, rounds the output table to the specified decimal.

**returns**

  * Pandas DataFrame

Examples
--------

.. code:: python

    import numpy, pandas, researchpy

    numpy.random.seed(12345678)

    df = pandas.DataFrame(numpy.random.randint(10, size= (100, 2)),
                      columns= ['healthy', 'non-healthy'])
    df['tx'] = ""
    df['tx'].iloc[0:50] = "Placebo"
    df['tx'].iloc[50:101] = "Experimental"

    df['dose'] = ""
    df['dose'].iloc[0:26] = "10 mg"
    df['dose'].iloc[26:51] = "25 mg"
    df['dose'].iloc[51:76] = "10 mg"
    df['dose'].iloc[76:101] = "25 mg"



.. code:: python

    # Summary statistics for a Series (single variable)
    researchpy.summary_cont(df['healthy'])

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
      </tbody>
    </table>
    </div>



.. code:: python

    # Summary statistics for multiple Series
    researchpy.summary_cont(df[['healthy', 'non-healthy']])

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
      </tbody>
    </table>
    </div>



.. code:: python

    # Easy to export results, assign to Python object which will have
    # the Pandas DataFrame class
    results = researchpy.summary_cont(df[['healthy', 'non-healthy']])

    results.to_csv("results.csv", index= False)



.. code:: python

    # This works with GroupBy objects as well
    researchpy.summary_cont(df['healthy'].groupby(df['tx']))

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>N</th>
          <th>Mean</th>
          <th>SD</th>
          <th>SE</th>
          <th>95% Conf.</th>
          <th>Interval</th>
        </tr>
        <tr>
          <th>tx</th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>Experimental</th>
          <td>50</td>
          <td>4.66</td>
          <td>2.560373</td>
          <td>0.362091</td>
          <td>3.943096</td>
          <td>5.376904</td>
        </tr>
        <tr>
          <th>Placebo</th>
          <td>50</td>
          <td>4.52</td>
          <td>2.950199</td>
          <td>0.417221</td>
          <td>3.693944</td>
          <td>5.346056</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: python

    # Even with a GroupBy object with a hierarchical index
    researchpy.summary_cont(df.groupby(['tx', 'dose'])['healthy', 'non-healthy'])

.. raw:: html

    <div style="overflow-x:auto;">
    <table border="1" class="dataframe">
      <thead>
        <tr>
          <th></th>
          <th></th>
          <th colspan="6" halign="left">healthy</th>
          <th colspan="6" halign="left">non-healthy</th>
        </tr>
        <tr>
          <th></th>
          <th></th>
          <th>count</th>
          <th>mean</th>
          <th>std</th>
          <th>sem</th>
          <th>95% Conf.</th>
          <th>Interval</th>
          <th>count</th>
          <th>mean</th>
          <th>std</th>
          <th>sem</th>
          <th>95% Conf.</th>
          <th>Interval</th>
        </tr>
        <tr>
          <th>tx</th>
          <th>dose</th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th rowspan="2" valign="top">Experimental</th>
          <th>10 mg</th>
          <td>25</td>
          <td>4.360000</td>
          <td>2.514624</td>
          <td>0.502925</td>
          <td>3.374267</td>
          <td>5.345733</td>
          <td>25</td>
          <td>4.160000</td>
          <td>3.197395</td>
          <td>0.639479</td>
          <td>2.906621</td>
          <td>5.413379</td>
        </tr>
        <tr>
          <th>25 mg</th>
          <td>25</td>
          <td>4.960000</td>
          <td>2.621704</td>
          <td>0.524341</td>
          <td>3.932292</td>
          <td>5.987708</td>
          <td>25</td>
          <td>4.240000</td>
          <td>3.205204</td>
          <td>0.641041</td>
          <td>2.983560</td>
          <td>5.496440</td>
        </tr>
        <tr>
          <th rowspan="2" valign="top">Placebo</th>
          <th>10 mg</th>
          <td>26</td>
          <td>4.115385</td>
          <td>2.984318</td>
          <td>0.585273</td>
          <td>2.968250</td>
          <td>5.262520</td>
          <td>26</td>
          <td>3.961538</td>
          <td>3.143002</td>
          <td>0.616393</td>
          <td>2.753407</td>
          <td>5.169670</td>
        </tr>
        <tr>
          <th>25 mg</th>
          <td>24</td>
          <td>4.958333</td>
          <td>2.911434</td>
          <td>0.594294</td>
          <td>3.793517</td>
          <td>6.123150</td>
          <td>24</td>
          <td>4.291667</td>
          <td>3.168859</td>
          <td>0.646841</td>
          <td>3.023859</td>
          <td>5.559474</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: python

    # Above is the default output, but if the results want to be compared
    # above/below each other use .apply()

    df.groupby(['tx', 'dose'])['healthy', 'non-healthy'].apply(researchpy.summary_cont)

.. raw:: html

    <div style="overflow-x:auto;">
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th></th>
          <th></th>
          <th>Variable</th>
          <th>N</th>
          <th>Mean</th>
          <th>SD</th>
          <th>SE</th>
          <th>95% Conf.</th>
          <th>Interval</th>
        </tr>
        <tr>
          <th>tx</th>
          <th>dose</th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th rowspan="4" valign="top">Experimental</th>
          <th rowspan="2" valign="top">10 mg</th>
          <th>0</th>
          <td>healthy</td>
          <td>25.0</td>
          <td>4.360000</td>
          <td>2.514624</td>
          <td>0.502925</td>
          <td>3.322014</td>
          <td>5.397986</td>
        </tr>
        <tr>
          <th>1</th>
          <td>non-healthy</td>
          <td>25.0</td>
          <td>4.160000</td>
          <td>3.197395</td>
          <td>0.639479</td>
          <td>2.840180</td>
          <td>5.479820</td>
        </tr>
        <tr>
          <th rowspan="2" valign="top">25 mg</th>
          <th>0</th>
          <td>healthy</td>
          <td>25.0</td>
          <td>4.960000</td>
          <td>2.621704</td>
          <td>0.524341</td>
          <td>3.877814</td>
          <td>6.042186</td>
        </tr>
        <tr>
          <th>1</th>
          <td>non-healthy</td>
          <td>25.0</td>
          <td>4.240000</td>
          <td>3.205204</td>
          <td>0.641041</td>
          <td>2.916957</td>
          <td>5.563043</td>
        </tr>
        <tr>
          <th rowspan="4" valign="top">Placebo</th>
          <th rowspan="2" valign="top">10 mg</th>
          <th>0</th>
          <td>healthy</td>
          <td>26.0</td>
          <td>4.115385</td>
          <td>2.984318</td>
          <td>0.585273</td>
          <td>2.909992</td>
          <td>5.320777</td>
        </tr>
        <tr>
          <th>1</th>
          <td>non-healthy</td>
          <td>26.0</td>
          <td>3.961538</td>
          <td>3.143002</td>
          <td>0.616393</td>
          <td>2.692052</td>
          <td>5.231024</td>
        </tr>
        <tr>
          <th rowspan="2" valign="top">25 mg</th>
          <th>0</th>
          <td>healthy</td>
          <td>24.0</td>
          <td>4.958333</td>
          <td>2.911434</td>
          <td>0.594294</td>
          <td>3.728942</td>
          <td>6.187724</td>
        </tr>
        <tr>
          <th>1</th>
          <td>non-healthy</td>
          <td>24.0</td>
          <td>4.291667</td>
          <td>3.168859</td>
          <td>0.646841</td>
          <td>2.953575</td>
          <td>5.629758</td>
        </tr>
      </tbody>
    </table>
    </div>
