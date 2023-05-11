# broom and dplyr

While broom is useful for summarizing the result of a single analysis in a consistent format, it is really designed for high-throughput applications, where you must combine results from multiple analyses. These could be subgroups of data, analyses using different models, bootstrap replicates, permutations, and so on. In particular, it plays well with the `nest/unnest` functions in `tidyr` and the `map` function in `purrr`. First, loading necessary packages and setting some defaults:


```r
library(broom)
library(tibble)
library(ggplot2)
library(dplyr)
library(tidyr)
library(purrr)

theme_set(theme_minimal())
```

Let's try this on a simple dataset, the built-in `Orange`. We start by coercing `Orange` to a `tibble`. This gives a nicer print method that will especially useful later on when we start working with list-columns.


```r
data(Orange)

Orange <- as_tibble(Orange)
Orange
```

```
## # A tibble: 35 x 3
##    Tree    age circumference
##    <ord> <dbl>         <dbl>
##  1 1       118            30
##  2 1       484            58
##  3 1       664            87
##  4 1      1004           115
##  5 1      1231           120
##  6 1      1372           142
##  7 1      1582           145
##  8 2       118            33
##  9 2       484            69
## 10 2       664           111
## # ... with 25 more rows
```

This contains 35 observations of three variables: `Tree`, `age`, and `circumference`. `Tree` is a factor with five levels describing five trees. As might be expected, age and circumference are correlated:


```r
cor(Orange$age, Orange$circumference)
```

```
## [1] 0.9135189
```

```r
ggplot(Orange, aes(age, circumference, color = Tree)) +
  geom_line()
```

![](05_broom_and_dplyr_files/figure-latex/unnamed-chunk-3-1.pdf)<!-- --> 

Suppose you want to test for correlations individually *within* each tree. You can do this with dplyr's `group_by`:


```r
Orange %>%
  group_by(Tree) %>%
  summarize(correlation = cor(age, circumference))
```

```
## # A tibble: 5 x 2
##   Tree  correlation
##   <ord>       <dbl>
## 1 3           0.988
## 2 1           0.985
## 3 5           0.988
## 4 2           0.987
## 5 4           0.984
```

(Note that the correlations are much higher than the aggregated one, and furthermore we can now see it is similar across trees).

Suppose that instead of simply estimating a correlation, we want to perform a hypothesis test with `cor.test`:


```r
ct <- cor.test(Orange$age, Orange$circumference)
ct
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  Orange$age and Orange$circumference
## t = 12.9, df = 33, p-value = 1.931e-14
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.8342364 0.9557955
## sample estimates:
##       cor 
## 0.9135189
```

This contains multiple values we could want in our output. Some are vectors of length 1, such as the p-value and the estimate, and some are longer, such as the confidence interval. We can get this into a nicely organized tibble using the `tidy` function:


```r
tidy(ct)
```

```
## # A tibble: 1 x 8
##   estimate statistic  p.value parameter conf.low conf.high method        alter~1
##      <dbl>     <dbl>    <dbl>     <int>    <dbl>     <dbl> <chr>         <chr>  
## 1    0.914      12.9 1.93e-14        33    0.834     0.956 Pearson's pr~ two.si~
## # ... with abbreviated variable name 1: alternative
```

Often, we want to perform multiple tests or fit multiple models, each on a different part of the data. In this case, we recommend a `nest-map-unnest` workflow. For example, suppose we want to perform correlation tests for each different tree. We start by `nest`ing our data based on the group of interest:


```r
nested <- Orange %>%
  nest(data = -Tree)
```

Then we run a correlation test for each nested tibble using `purrr::map`:


```r
nested %>%
  mutate(test = map(data, ~ cor.test(.x$age, .x$circumference)))
```

```
## # A tibble: 5 x 3
##   Tree  data             test   
##   <ord> <list>           <list> 
## 1 1     <tibble [7 x 2]> <htest>
## 2 2     <tibble [7 x 2]> <htest>
## 3 3     <tibble [7 x 2]> <htest>
## 4 4     <tibble [7 x 2]> <htest>
## 5 5     <tibble [7 x 2]> <htest>
```

This results in a list-column of S3 objects. We want to tidy each of the objects, which we can also do with `map`.


```r
nested %>%
  mutate(
    test = map(data, ~ cor.test(.x$age, .x$circumference)), # S3 list-col
    tidied = map(test, tidy)
  )
```

```
## # A tibble: 5 x 4
##   Tree  data             test    tidied          
##   <ord> <list>           <list>  <list>          
## 1 1     <tibble [7 x 2]> <htest> <tibble [1 x 8]>
## 2 2     <tibble [7 x 2]> <htest> <tibble [1 x 8]>
## 3 3     <tibble [7 x 2]> <htest> <tibble [1 x 8]>
## 4 4     <tibble [7 x 2]> <htest> <tibble [1 x 8]>
## 5 5     <tibble [7 x 2]> <htest> <tibble [1 x 8]>
```

Finally, we want to unnest the tidied data frames so we can see the results in a flat tibble. All together, this looks like:


```r
Orange %>%
  nest(data = -Tree) %>%
  mutate(
    test = map(data, ~ cor.test(.x$age, .x$circumference)), # S3 list-col
    tidied = map(test, tidy)
  ) %>%
  unnest(tidied)
```

```
## # A tibble: 5 x 11
##   Tree  data     test    estimate stati~1 p.value param~2 conf.~3 conf.~4 method
##   <ord> <list>   <list>     <dbl>   <dbl>   <dbl>   <int>   <dbl>   <dbl> <chr> 
## 1 1     <tibble> <htest>    0.985    13.0 4.85e-5       5   0.901   0.998 Pears~
## 2 2     <tibble> <htest>    0.987    13.9 3.43e-5       5   0.914   0.998 Pears~
## 3 3     <tibble> <htest>    0.988    14.4 2.90e-5       5   0.919   0.998 Pears~
## 4 4     <tibble> <htest>    0.984    12.5 5.73e-5       5   0.895   0.998 Pears~
## 5 5     <tibble> <htest>    0.988    14.1 3.18e-5       5   0.916   0.998 Pears~
## # ... with 1 more variable: alternative <chr>, and abbreviated variable names
## #   1: statistic, 2: parameter, 3: conf.low, 4: conf.high
```

This workflow becomes even more useful when applied to regressions. Untidy output for a regression looks like:


```r
lm_fit <- lm(age ~ circumference, data = Orange)
summary(lm_fit)
```

```
## 
## Call:
## lm(formula = age ~ circumference, data = Orange)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -317.88 -140.90  -17.20   96.54  471.16 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)    16.6036    78.1406   0.212    0.833    
## circumference   7.8160     0.6059  12.900 1.93e-14 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 203.1 on 33 degrees of freedom
## Multiple R-squared:  0.8345,	Adjusted R-squared:  0.8295 
## F-statistic: 166.4 on 1 and 33 DF,  p-value: 1.931e-14
```

where we tidy these results, we get multiple rows of output for each model:


```r
tidy(lm_fit)
```

```
## # A tibble: 2 x 5
##   term          estimate std.error statistic  p.value
##   <chr>            <dbl>     <dbl>     <dbl>    <dbl>
## 1 (Intercept)      16.6     78.1       0.212 8.33e- 1
## 2 circumference     7.82     0.606    12.9   1.93e-14
```

Now we can handle multiple regressions at once using exactly the same workflow as before:


```r
Orange %>%
  nest(data = -Tree) %>%
  mutate(
    fit = map(data, ~ lm(age ~ circumference, data = .x)),
    tidied = map(fit, tidy)
  ) %>%
  unnest(tidied)
```

```
## # A tibble: 10 x 8
##    Tree  data             fit    term          estimate std.er~1 stati~2 p.value
##    <ord> <list>           <list> <chr>            <dbl>    <dbl>   <dbl>   <dbl>
##  1 1     <tibble [7 x 2]> <lm>   (Intercept)    -265.     98.6    -2.68  4.36e-2
##  2 1     <tibble [7 x 2]> <lm>   circumference    11.9     0.919  13.0   4.85e-5
##  3 2     <tibble [7 x 2]> <lm>   (Intercept)    -132.     83.1    -1.59  1.72e-1
##  4 2     <tibble [7 x 2]> <lm>   circumference     7.80    0.560  13.9   3.43e-5
##  5 3     <tibble [7 x 2]> <lm>   (Intercept)    -210.     85.3    -2.46  5.74e-2
##  6 3     <tibble [7 x 2]> <lm>   circumference    12.0     0.835  14.4   2.90e-5
##  7 4     <tibble [7 x 2]> <lm>   (Intercept)     -76.5    88.3    -0.867 4.26e-1
##  8 4     <tibble [7 x 2]> <lm>   circumference     7.17    0.572  12.5   5.73e-5
##  9 5     <tibble [7 x 2]> <lm>   (Intercept)     -54.5    76.9    -0.709 5.10e-1
## 10 5     <tibble [7 x 2]> <lm>   circumference     8.79    0.621  14.1   3.18e-5
## # ... with abbreviated variable names 1: std.error, 2: statistic
```

You can just as easily use multiple predictors in the regressions, as shown here on the `mtcars` dataset. We nest the data into automatic and manual cars (the `am` column), then perform the regression within each nested tibble.


```r
data(mtcars)
mtcars <- as_tibble(mtcars) # to play nicely with list-cols
mtcars
```

```
## # A tibble: 32 x 11
##      mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
##  1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
##  2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
##  3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
##  4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
##  5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
##  6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
##  7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
##  8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
##  9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
## 10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
## # ... with 22 more rows
```

```r
mtcars %>%
  nest(data = -am) %>%
  mutate(
    fit = map(data, ~ lm(wt ~ mpg + qsec + gear, data = .x)), # S3 list-col
    tidied = map(fit, tidy)
  ) %>%
  unnest(tidied)
```

```
## # A tibble: 8 x 8
##      am data               fit    term        estimate std.error stati~1 p.value
##   <dbl> <list>             <list> <chr>          <dbl>     <dbl>   <dbl>   <dbl>
## 1     1 <tibble [13 x 10]> <lm>   (Intercept)   4.28      3.46    1.24   2.47e-1
## 2     1 <tibble [13 x 10]> <lm>   mpg          -0.101     0.0294 -3.43   7.50e-3
## 3     1 <tibble [13 x 10]> <lm>   qsec          0.0398    0.151   0.264  7.98e-1
## 4     1 <tibble [13 x 10]> <lm>   gear         -0.0229    0.349  -0.0656 9.49e-1
## 5     0 <tibble [19 x 10]> <lm>   (Intercept)   4.92      1.40    3.52   3.09e-3
## 6     0 <tibble [19 x 10]> <lm>   mpg          -0.192     0.0443 -4.33   5.91e-4
## 7     0 <tibble [19 x 10]> <lm>   qsec          0.0919    0.0983  0.935  3.65e-1
## 8     0 <tibble [19 x 10]> <lm>   gear          0.147     0.368   0.398  6.96e-1
## # ... with abbreviated variable name 1: statistic
```

What if you want not just the `tidy` output, but the `augment` and `glance` outputs as well, while still performing each regression only once? Since we're using list-columns, we can just fit the model once and use multiple list-columns to store the tidied, glanced and augmented outputs.


```r
regressions <- mtcars %>%
  nest(data = -am) %>%
  mutate(
    fit = map(data, ~ lm(wt ~ mpg + qsec + gear, data = .x)),
    tidied = map(fit, tidy),
    glanced = map(fit, glance),
    augmented = map(fit, augment)
  )

regressions %>%
  unnest(tidied)
```

```
## # A tibble: 8 x 10
##      am data     fit    term   estim~1 std.e~2 stati~3 p.value glanced  augmen~4
##   <dbl> <list>   <list> <chr>    <dbl>   <dbl>   <dbl>   <dbl> <list>   <list>  
## 1     1 <tibble> <lm>   (Inte~  4.28    3.46    1.24   2.47e-1 <tibble> <tibble>
## 2     1 <tibble> <lm>   mpg    -0.101   0.0294 -3.43   7.50e-3 <tibble> <tibble>
## 3     1 <tibble> <lm>   qsec    0.0398  0.151   0.264  7.98e-1 <tibble> <tibble>
## 4     1 <tibble> <lm>   gear   -0.0229  0.349  -0.0656 9.49e-1 <tibble> <tibble>
## 5     0 <tibble> <lm>   (Inte~  4.92    1.40    3.52   3.09e-3 <tibble> <tibble>
## 6     0 <tibble> <lm>   mpg    -0.192   0.0443 -4.33   5.91e-4 <tibble> <tibble>
## 7     0 <tibble> <lm>   qsec    0.0919  0.0983  0.935  3.65e-1 <tibble> <tibble>
## 8     0 <tibble> <lm>   gear    0.147   0.368   0.398  6.96e-1 <tibble> <tibble>
## # ... with abbreviated variable names 1: estimate, 2: std.error, 3: statistic,
## #   4: augmented
```

```r
regressions %>%
  unnest(glanced)
```

```
## # A tibble: 2 x 17
##      am data     fit    tidied   r.squared adj.r.s~1 sigma stati~2 p.value    df
##   <dbl> <list>   <list> <list>       <dbl>     <dbl> <dbl>   <dbl>   <dbl> <dbl>
## 1     1 <tibble> <lm>   <tibble>     0.833     0.778 0.291   15.0  7.59e-4     3
## 2     0 <tibble> <lm>   <tibble>     0.625     0.550 0.522    8.32 1.70e-3     3
## # ... with 7 more variables: logLik <dbl>, AIC <dbl>, BIC <dbl>,
## #   deviance <dbl>, df.residual <int>, nobs <int>, augmented <list>, and
## #   abbreviated variable names 1: adj.r.squared, 2: statistic
```

```r
regressions %>%
  unnest(augmented)
```

```
## # A tibble: 32 x 15
##       am data     fit    tidied   glanced     wt   mpg  qsec  gear .fitted
##    <dbl> <list>   <list> <list>   <list>   <dbl> <dbl> <dbl> <dbl>   <dbl>
##  1     1 <tibble> <lm>   <tibble> <tibble>  2.62  21    16.5     4    2.73
##  2     1 <tibble> <lm>   <tibble> <tibble>  2.88  21    17.0     4    2.75
##  3     1 <tibble> <lm>   <tibble> <tibble>  2.32  22.8  18.6     4    2.63
##  4     1 <tibble> <lm>   <tibble> <tibble>  2.2   32.4  19.5     4    1.70
##  5     1 <tibble> <lm>   <tibble> <tibble>  1.62  30.4  18.5     4    1.86
##  6     1 <tibble> <lm>   <tibble> <tibble>  1.84  33.9  19.9     4    1.56
##  7     1 <tibble> <lm>   <tibble> <tibble>  1.94  27.3  18.9     4    2.19
##  8     1 <tibble> <lm>   <tibble> <tibble>  2.14  26    16.7     5    2.21
##  9     1 <tibble> <lm>   <tibble> <tibble>  1.51  30.4  16.9     5    1.77
## 10     1 <tibble> <lm>   <tibble> <tibble>  3.17  15.8  14.5     5    3.15
## # ... with 22 more rows, and 5 more variables: .resid <dbl>, .hat <dbl>,
## #   .sigma <dbl>, .cooksd <dbl>, .std.resid <dbl>
```

By combining the estimates and p-values across all groups into the same tidy data frame (instead of a list of output model objects), a new class of analyses and visualizations becomes straightforward. This includes

- Sorting by p-value or estimate to find the most significant terms across all tests
- P-value histograms
- Volcano plots comparing p-values to effect size estimates

In each of these cases, we can easily filter, facet, or distinguish based on the `term` column. In short, this makes the tools of tidy data analysis available for the *results* of data analysis and models, not just the inputs.


## Tidy bootstrapping

Another place where combining model fits in a tidy way becomes useful is when performing bootstrapping or permutation tests. These approaches have been explored before, for instance by [Andrew MacDonald here](http://rstudio-pubs-static.s3.amazonaws.com/19698_a4c472606e3c43e4b94720506e49bb7b.html), and [Hadley has explored efficient support for bootstrapping](https://github.com/hadley/dplyr/issues/269) as a potential enhancement to dplyr. broom fits naturally with dplyr in performing these analyses.

Bootstrapping consists of randomly sampling a dataset with replacement, then performing the analysis individually on each bootstrapped replicate. The variation in the resulting estimate is then a reasonable approximation of the variance in our estimate.

Let's say we want to fit a nonlinear model to the weight/mileage relationship in the `mtcars` dataset.


```r
library(ggplot2)
theme_set(theme_minimal())
ggplot(mtcars, aes(mpg, wt)) + 
    geom_point()
```

![](05_broom_and_dplyr_files/figure-latex/unnamed-chunk-16-1.pdf)<!-- --> 

We might use the method of nonlinear least squares (via the `nls` function) to fit a model.


```r
nlsfit <- nls(mpg ~ k / wt + b, mtcars, start = list(k = 1, b = 0))
summary(nlsfit)
```

```
## 
## Formula: mpg ~ k/wt + b
## 
## Parameters:
##   Estimate Std. Error t value Pr(>|t|)    
## k   45.829      4.249  10.786 7.64e-12 ***
## b    4.386      1.536   2.855  0.00774 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.774 on 30 degrees of freedom
## 
## Number of iterations to convergence: 1 
## Achieved convergence tolerance: 2.877e-08
```

```r
ggplot(mtcars, aes(wt, mpg)) +
    geom_point() +
    geom_line(aes(y = predict(nlsfit)))
```

![](05_broom_and_dplyr_files/figure-latex/unnamed-chunk-17-1.pdf)<!-- --> 

While this does provide a p-value and confidence intervals for the parameters, these are based on model assumptions that may not hold in real data. Bootstrapping is a popular method for providing confidence intervals and predictions that are more robust to the nature of the data.

We can use the `bootstraps` function in the **rsample** package to sample bootstrap replications. First, we construct 100 bootstrap replications of the data, each of which has been randomly sampled with replacement. The resulting object is an `rset`, which is a dataframe with a column of `rsplit` objects.

An `rsplit` object has two main components: an analysis dataset and an assessment dataset, accessible via `analysis(rsplit)` and `assessment(rsplit)` respectively. For bootstrap samples, the analysis dataset is the bootstrap sample itself, and the assessment dataset consists of all the out of bag samples.


```r
library(dplyr)
library(rsample)
library(broom)
library(purrr)
library(tidyr)
set.seed(27)
boots <- bootstraps(mtcars, times = 100)
boots
```

```
## # Bootstrap sampling 
## # A tibble: 100 x 2
##    splits          id          
##    <list>          <chr>       
##  1 <split [32/13]> Bootstrap001
##  2 <split [32/10]> Bootstrap002
##  3 <split [32/13]> Bootstrap003
##  4 <split [32/11]> Bootstrap004
##  5 <split [32/9]>  Bootstrap005
##  6 <split [32/10]> Bootstrap006
##  7 <split [32/11]> Bootstrap007
##  8 <split [32/13]> Bootstrap008
##  9 <split [32/11]> Bootstrap009
## 10 <split [32/11]> Bootstrap010
## # ... with 90 more rows
```

We create a helper function to fit an `nls` model on each bootstrap sample, and then use `purrr::map` to apply this function to all the bootstrap samples at once. Similarly, we create a column of tidy coefficient information by unnesting.


```r
fit_nls_on_bootstrap <- function(split) {
    nls(mpg ~ k / wt + b, analysis(split), start = list(k = 1, b = 0))
}
boot_models <- boots %>% 
    mutate(model = map(splits, fit_nls_on_bootstrap),
           coef_info = map(model, tidy))
boot_models
```

```
## # Bootstrap sampling 
## # A tibble: 100 x 4
##    splits          id           model  coef_info       
##    <list>          <chr>        <list> <list>          
##  1 <split [32/13]> Bootstrap001 <nls>  <tibble [2 x 5]>
##  2 <split [32/10]> Bootstrap002 <nls>  <tibble [2 x 5]>
##  3 <split [32/13]> Bootstrap003 <nls>  <tibble [2 x 5]>
##  4 <split [32/11]> Bootstrap004 <nls>  <tibble [2 x 5]>
##  5 <split [32/9]>  Bootstrap005 <nls>  <tibble [2 x 5]>
##  6 <split [32/10]> Bootstrap006 <nls>  <tibble [2 x 5]>
##  7 <split [32/11]> Bootstrap007 <nls>  <tibble [2 x 5]>
##  8 <split [32/13]> Bootstrap008 <nls>  <tibble [2 x 5]>
##  9 <split [32/11]> Bootstrap009 <nls>  <tibble [2 x 5]>
## 10 <split [32/11]> Bootstrap010 <nls>  <tibble [2 x 5]>
## # ... with 90 more rows
```

```r
boot_coefs <- boot_models %>% 
    unnest(coef_info)
```

The unnested coefficient information contains a summary of each replication combined in a single data frame:


```r
boot_coefs
```

```
## # A tibble: 200 x 8
##    splits          id           model  term  estimate std.error stati~1  p.value
##    <list>          <chr>        <list> <chr>    <dbl>     <dbl>   <dbl>    <dbl>
##  1 <split [32/13]> Bootstrap001 <nls>  k        42.1       4.05   10.4  1.91e-11
##  2 <split [32/13]> Bootstrap001 <nls>  b         5.39      1.43    3.78 6.93e- 4
##  3 <split [32/10]> Bootstrap002 <nls>  k        49.9       5.66    8.82 7.82e-10
##  4 <split [32/10]> Bootstrap002 <nls>  b         3.73      1.92    1.94 6.13e- 2
##  5 <split [32/13]> Bootstrap003 <nls>  k        37.8       2.68   14.1  9.01e-15
##  6 <split [32/13]> Bootstrap003 <nls>  b         6.73      1.17    5.75 2.78e- 6
##  7 <split [32/11]> Bootstrap004 <nls>  k        45.6       4.45   10.2  2.70e-11
##  8 <split [32/11]> Bootstrap004 <nls>  b         4.75      1.62    2.93 6.38e- 3
##  9 <split [32/9]>  Bootstrap005 <nls>  k        43.6       4.63    9.41 1.85e-10
## 10 <split [32/9]>  Bootstrap005 <nls>  b         5.89      1.68    3.51 1.44e- 3
## # ... with 190 more rows, and abbreviated variable name 1: statistic
```

We can then calculate confidence intervals (using what is called the percentile method):


```r
alpha <- .05
boot_coefs %>% 
    group_by(term) %>%
    summarize(low = quantile(estimate, alpha / 2),
              high = quantile(estimate, 1 - alpha / 2))
```

```
## # A tibble: 2 x 3
##   term     low  high
##   <chr>  <dbl> <dbl>
## 1 b      0.283  6.74
## 2 k     38.5   57.6
```

Or we can use histograms to get a more detailed idea of the uncertainty in each estimate:


```r
ggplot(boot_coefs, aes(estimate)) + 
    geom_histogram(binwidth = 2) + 
    facet_wrap(~ term, scales = "free")
```

![](05_broom_and_dplyr_files/figure-latex/unnamed-chunk-22-1.pdf)<!-- --> 

Or we can use `augment` to visualize the uncertainty in the curve:


```r
boot_aug <- boot_models %>% 
    mutate(augmented = map(model, augment)) %>% 
    unnest(augmented)
boot_aug
```

```
## # A tibble: 3,200 x 8
##    splits          id           model  coef_info   mpg    wt .fitted .resid
##    <list>          <chr>        <list> <list>    <dbl> <dbl>   <dbl>  <dbl>
##  1 <split [32/13]> Bootstrap001 <nls>  <tibble>   18.7  3.44    17.6  1.08 
##  2 <split [32/13]> Bootstrap001 <nls>  <tibble>   32.4  2.2     24.5  7.89 
##  3 <split [32/13]> Bootstrap001 <nls>  <tibble>   15.5  3.52    17.3 -1.84 
##  4 <split [32/13]> Bootstrap001 <nls>  <tibble>   22.8  3.15    18.7  4.05 
##  5 <split [32/13]> Bootstrap001 <nls>  <tibble>   24.4  3.19    18.6  5.82 
##  6 <split [32/13]> Bootstrap001 <nls>  <tibble>   30.4  1.62    31.4 -1.04 
##  7 <split [32/13]> Bootstrap001 <nls>  <tibble>   10.4  5.42    13.1 -2.75 
##  8 <split [32/13]> Bootstrap001 <nls>  <tibble>   21    2.62    21.4 -0.448
##  9 <split [32/13]> Bootstrap001 <nls>  <tibble>   19.2  3.84    16.3  2.87 
## 10 <split [32/13]> Bootstrap001 <nls>  <tibble>   21    2.62    21.4 -0.448
## # ... with 3,190 more rows
```


```r
ggplot(boot_aug, aes(wt, mpg)) +
    geom_point() +
    geom_line(aes(y = .fitted, group = id), alpha=.2)
```

![](05_broom_and_dplyr_files/figure-latex/unnamed-chunk-24-1.pdf)<!-- --> 

With only a few small changes, we could easily perform bootstrapping with other kinds of predictive or hypothesis testing models, since the `tidy` and `augment` functions works for many statistical outputs. As another example, we could use `smooth.spline`, which fits a cubic smoothing spline to data:


```r
fit_spline_on_bootstrap <- function(split) {
    data <- analysis(split)
    smooth.spline(data$wt, data$mpg, df = 4)
}
boot_splines <- boots %>% 
    mutate(spline = map(splits, fit_spline_on_bootstrap),
           aug_train = map(spline, augment))
splines_aug <- boot_splines %>% 
    unnest(aug_train)
ggplot(splines_aug, aes(x, y)) +
    geom_point() +
    geom_line(aes(y = .fitted, group = id), alpha = 0.2)
```

![](05_broom_and_dplyr_files/figure-latex/unnamed-chunk-25-1.pdf)<!-- --> 

## links

https://www.youtube.com/watch?v=1bnhT8tlCJQ&list=PLBnFxG6owe1F-3y0_aphRZ5YHH06Qr1Kj

https://bookdown.org/bruno_lucian_costa/CursoIntermediarioR/tidyr.html

https://bookdown.org/Maxine/r4ds/nesting.html

https://livro.curso-r.com/7-3-tidyr.html

http://leg.ufpr.br/~walmes/cursoR/data-vis/slides/04-tidyr.pdf
