codebook()
==============
Prints out descriptive information for a Pandas Series or DataFrame object.

Arguments
----------
**codebook(data)**

  * **data** must either be a Pandas Series or DataFrame

**returns**

  * Information from a print()

Examples
--------

.. code:: python

    import researchpy as rp
    import pandas as pd
    import patsy

    pdf = pd.DataFrame(patsy.demo_data("y", "x", "age", "disease",
                                   nlevels=3,
                                   min_rows= 20))
    rp.codebook(pdf)



.. raw:: html

  <div>
  <pre>
  Variable: age    Data Type: object

  Number of Obs.: 27
  Number of missing obs.: 0
  Percent missing: 0.0
  Number of unique values: 3

  Data Values and Counts:

  Values  Frequency
  age1          9
  age2          9
  age3          9




  Variable: disease    Data Type: object

  Number of Obs.: 27
  Number of missing obs.: 0
  Percent missing: 0.0
  Number of unique values: 3

  Data Values and Counts:

  Values       Frequency
  disease1          9
  disease2          9
  disease3          9




  Variable: x    Data Type: float64

  Number of Obs.: 27
  Number of missing obs.: 0
  Percent missing: 0.0
  Number of unique values: 27

  Range: [-2.5529898158340787, 2.2697546239876076]
  Mean: 0.39
  Standard Deviation: 1.12
  Mode: -2.5529898158340787
  10th Percentile: -0.9033685955315993
  25th Percentile: -0.12728803004562786
  50th Percentile: 0.4001572083672233
  75th Percentile: 0.9644132008156643
  90th Percentile: 1.8054546036405856





  Variable: y    Data Type: float64

  Number of Obs.: 27
  Number of missing obs.: 0
  Percent missing: 0.0
  Number of unique values: 27

  Range: [-1.980796468223927, 1.9507753952317897]
  Mean: -0.21
  Standard Deviation: 1.06
  Mode: -1.980796468223927
  10th Percentile: -1.4975699013305657
  25th Percentile: -0.9720097631303841
  50th Percentile: -0.3479121493261526
  75th Percentile: 0.38253250873071776
  90th Percentile: 1.325917916396747
  </pre>
  </div>
