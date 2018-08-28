summary_cat()
=============
Returns a data table as a Pandas DataFrame that includes the counts and
percentages of each category. If there are missing data
present (numpy.nan), they will be excluded from the counts. However, if the
missing data is coded as a string, it will be included as it's own category.

Arguments
---------
**summary_cat(group1, ascending= False)**

  * **group1**, can be a Pandas Series or DataFrame with multiple columns stated
  * **ascending**, determines the output ascending order or not. Default is
      descending.

**returns**

  * Pandas DataFrame

Examples
--------
.. code:: ipython3

    import numpy, pandas, researchpy

    numpy.random.seed(123)

    df = pandas.DataFrame(numpy.random.randint(2, size= (101, 2)),
                      columns= ['disease', 'treatment'])

.. code:: ipython3

    # Handles a single Pandas Series
    researchpy.summary_cat(df['disease'])




.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>Outcome</th>
          <th>Count</th>
          <th>Percent</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>disease</td>
          <td>0</td>
          <td>53</td>
          <td>52.48</td>
        </tr>
        <tr>
          <th>1</th>
          <td></td>
          <td>1</td>
          <td>48</td>
          <td>47.52</td>
        </tr>
      </tbody>
    </table>
    </div>

.. code:: ipython3

    # Can handle multiple Series, although the output is not pretty
    researchpy.summary_cat(df[['disease', 'treatment']])

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>Outcome</th>
          <th>Count</th>
          <th>Percent</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>disease</td>
          <td>0</td>
          <td>53</td>
          <td>52.48</td>
        </tr>
        <tr>
          <th>1</th>
          <td></td>
          <td>1</td>
          <td>48</td>
          <td>47.52</td>
        </tr>
        <tr>
          <th>2</th>
          <td>treatment</td>
          <td>1</td>
          <td>52</td>
          <td>51.49</td>
        </tr>
        <tr>
          <th>3</th>
          <td></td>
          <td>0</td>
          <td>49</td>
          <td>48.51</td>
        </tr>
      </tbody>
    </table>
    </div>

.. code:: ipython3

    # If missing is a string, it will show up as it's own category
    df['disease'][0] = ""

    researchpy.summary_cat(df['disease'])

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>Outcome</th>
          <th>Count</th>
          <th>Percent</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>disease</td>
          <td>0</td>
          <td>52</td>
          <td>51.49</td>
        </tr>
        <tr>
          <th>1</th>
          <td></td>
          <td>1</td>
          <td>48</td>
          <td>47.52</td>
        </tr>
        <tr>
          <th>2</th>
          <td></td>
          <td></td>
          <td>1</td>
          <td>0.99</td>
        </tr>
      </tbody>
    </table>
    </div>

.. code:: ipython3

    # However, is missing is a numpy.nan, it will be excluded from the counts
    df['disease'][0] = numpy.nan

    researchpy.summary_cat(df['disease'])


.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>Outcome</th>
          <th>Count</th>
          <th>Percent</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>disease</td>
          <td>0</td>
          <td>52</td>
          <td>52.0</td>
        </tr>
        <tr>
          <th>1</th>
          <td></td>
          <td>1</td>
          <td>48</td>
          <td>48.0</td>
        </tr>
      </tbody>
    </table>
    </div>

.. code:: ipython3

    # Results can easily be exported using many methods including the default
    # Pandas exporting methods
    results = researchpy.summary_cat(df['disease'])

    results.to_csv("summary_cats.csv", index= False)

.. code:: ipython3

    # This is the default, showing for comparison of immediately below
    researchpy.summary_cat(df['disease'], ascending= False)

.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>Outcome</th>
          <th>Count</th>
          <th>Percent</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>disease</td>
          <td>0</td>
          <td>52</td>
          <td>52.0</td>
        </tr>
        <tr>
          <th>1</th>
          <td></td>
          <td>1</td>
          <td>48</td>
          <td>48.0</td>
        </tr>
      </tbody>
    </table>
    </div>

.. code:: ipython3

    researchpy.summary_cat(df['disease'], ascending= True)


.. raw:: html

    <div>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Variable</th>
          <th>Outcome</th>
          <th>Count</th>
          <th>Percent</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>disease</td>
          <td>1</td>
          <td>48</td>
          <td>48.0</td>
        </tr>
        <tr>
          <th>1</th>
          <td></td>
          <td>0</td>
          <td>52</td>
          <td>52.0</td>
        </tr>
      </tbody>
    </table>
    </div>
