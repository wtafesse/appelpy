<header>
<pre><p style="font-size:28px;"><b>diagnostics</b></p></pre>
</header>

# Overview
The **`diagnostics`** module has classes and functions to examine the fit of OLS models and the extreme observations in datasets.

The main class is the `BadApples` class, which consumes an OLS model object and is used to examine the outliers, high-leverage points and influential points in a model.  In essence it is used to examine the 'bad apples' that may be stinking up a model's results.

The main methods are:

- `variance_inflation_factors`
- `heteroskedasticity_test`
- `partial_regression_plot`

There are also methods for diagnostic plots such as `pp_plot` but they are exposed more conveniently in an OLS model object method.

## `BadApples`
The **10 Minutes To Appelpy** notebook fits a **BadApples** instance, consuming a model of the California Test Score dataset.

- [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/mfarragher/appelpy-examples/master?filepath=00_ten-minutes-to-appelpy.ipynb): interactive experience of the *10 Minutes to Appelpy* tutorial via Binder.
- [![nbviewer](https://img.shields.io/badge/render-nbviewer-orange.svg)](https://nbviewer.jupyter.org/github/mfarragher/appelpy-examples/blob/master/00_ten-minutes-to-appelpy.ipynb): static render of the *10 Minutes to Appelpy* notebook.

```python
from appelpy.diagnostics import BadApples
bad_apples = BadApples(model_hc1).fit()
```

### Attributes
- Measures: `measures_influence`, `measures_leverage` and `measures_outliers`.
- Indices: `indices_high_influence`, `indices_high_leverage` and `indices_outliers`.

INFLUENCE:

- dfbeta (for each independent variable): DFBETA diagnostic.
    Extreme if val > 2 / sqrt(n)
- dffits (for each independent variable): DFFITS diagnostic.
    Extreme if val > 2 * sqrt(k / n)
- cooks_d: Cook's distance.  Extreme if val > 4 / n

LEVERAGE:

- leverage: value from hat matrix diagonal.  Extreme if
    val > (2*k + 2) / n

OUTLIERS:

- resid_standardized: standardized residual.  Extreme if
    |val| > 2, i.e. approx. 5% of observations will be
    flagged.
- resid_studentized: studentized residual.  Extreme if
    |val| > 2, i.e. approx. 5% of observations will be
    flagged.

### Methods
The `plot_leverage_vs_residuals_squared` method plots leverage values (y-axis) against the residuals squared (x-axis).  The plot can be annotated with the index values.

## Variance inflation factors
The `variance_inflation_factors` method takes a dataframe and calculates the variance inflation factors of its regressors.

## Heteroskedasticity test
The `heteroskedasticity_test` method takes an OLS model object and returns the results of a heteroskedasticity test (the test statistic and p-value).  Examples of heteroskedasticity tests include:

- Breusch-Pagan test (`breusch_pagan`)
- Breusch-Pagan studentized test (`breusch_pagan_studentized`)
- White test (`white`)

The **10 Minutes To Appelpy** notebook shows the results of heteroskedasticity tests, given a model fitted to the California Test Score dataset.

- [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/mfarragher/appelpy-examples/master?filepath=00_ten-minutes-to-appelpy.ipynb): interactive experience of the *10 Minutes to Appelpy* tutorial via Binder.
- [![nbviewer](https://img.shields.io/badge/render-nbviewer-orange.svg)](https://nbviewer.jupyter.org/github/mfarragher/appelpy-examples/blob/master/00_ten-minutes-to-appelpy.ipynb): static render of the *10 Minutes to Appelpy* notebook.

Here is a code snippet for a heteroskedasticity test.
```python
from appelpy.diagnostics import heteroskedasticity_test

ep, pval = heteroskedasticity_test('breusch_pagan_studentized', model_nonrobust)
print('Breusch-Pagan test (studentized)')
print('Test statistic: {:.4f}'.format(ep))
print('Test p-value: {:.4f}'.format(pval))
```

## Partial regression plot
Also known as the added variable plot, the partial regression plot shows the effect of adding another regressor (independent variable) to a regression model.

The method requires these parameters:

- `appelpy_model_object`: a fitted OLS model object.
- `df`: the dataframe used in the model.
- `regressor`: the additional variable in the partial regression.