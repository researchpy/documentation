anova()
=======
This method performs an analysis of variance (ANOVA). It is a wrapper for various
statsmodels packages that provides the desired output with 1 method.

Statsmodels currently only has the option to use Sum of Square I in it's
calculation. This is the same sum of squares that is used by default in R; but
is different from SPSS, Stata, and SAS where these companies use sum of squares
III.
