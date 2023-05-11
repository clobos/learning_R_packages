# tidyr

More details in [https://tidyr.tidyverse.org/articles/nest.html](https://tidyr.tidyverse.org/articles/nest.html)

## nest()


```r
library(tidyverse)

mtcars %>% 
  group_by(cyl) %>% 
  nest() 
```

```
## # A tibble: 3 x 2
## # Groups:   cyl [3]
##     cyl data              
##   <dbl> <list>            
## 1     6 <tibble [7 x 10]> 
## 2     4 <tibble [11 x 10]>
## 3     8 <tibble [14 x 10]>
```

```r
um<- mtcars %>% 
  group_by(cyl) %>% 
  nest() %>%
  mutate(
    linMod = map(data, ~lm(mpg ~ wt, data = .)),
    coeffs = map(linMod, coefficients),
    slope = map_dbl(coeffs, 2))
um
```

```
## # A tibble: 3 x 5
## # Groups:   cyl [3]
##     cyl data               linMod coeffs    slope
##   <dbl> <list>             <list> <list>    <dbl>
## 1     6 <tibble [7 x 10]>  <lm>   <dbl [2]> -2.78
## 2     4 <tibble [11 x 10]> <lm>   <dbl [2]> -5.65
## 3     8 <tibble [14 x 10]> <lm>   <dbl [2]> -2.19
```

```r
um$linMod
```

```
## [[1]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## [[2]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## [[3]]
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

```r
um$coeffs
```

```
## [[1]]
## (Intercept)          wt 
##   28.408845   -2.780106 
## 
## [[2]]
## (Intercept)          wt 
##   39.571196   -5.647025 
## 
## [[3]]
## (Intercept)          wt 
##   23.868029   -2.192438
```

```r
um$slope
```

```
## [1] -2.780106 -5.647025 -2.192438
```

```r
um$linMod[[1]]
```

```
## 
## Call:
## lm(formula = mpg ~ wt, data = .)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78
```

```r
dois<- mtcars %>% 
  group_by(cyl) %>% 
  nest() %>% 
  mutate(model = map(data, function(df) lm(mpg ~ wt, data = df)))
dois
```

```
## # A tibble: 3 x 3
## # Groups:   cyl [3]
##     cyl data               model 
##   <dbl> <list>             <list>
## 1     6 <tibble [7 x 10]>  <lm>  
## 2     4 <tibble [11 x 10]> <lm>  
## 3     8 <tibble [14 x 10]> <lm>
```

```r
dois$cyl
```

```
## [1] 6 4 8
```

```r
dois$data
```

```
## [[1]]
## # A tibble: 7 x 10
##     mpg  disp    hp  drat    wt  qsec    vs    am  gear  carb
##   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
## 1  21    160    110  3.9   2.62  16.5     0     1     4     4
## 2  21    160    110  3.9   2.88  17.0     0     1     4     4
## 3  21.4  258    110  3.08  3.22  19.4     1     0     3     1
## 4  18.1  225    105  2.76  3.46  20.2     1     0     3     1
## 5  19.2  168.   123  3.92  3.44  18.3     1     0     4     4
## 6  17.8  168.   123  3.92  3.44  18.9     1     0     4     4
## 7  19.7  145    175  3.62  2.77  15.5     0     1     5     6
## 
## [[2]]
## # A tibble: 11 x 10
##      mpg  disp    hp  drat    wt  qsec    vs    am  gear  carb
##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
##  1  22.8 108      93  3.85  2.32  18.6     1     1     4     1
##  2  24.4 147.     62  3.69  3.19  20       1     0     4     2
##  3  22.8 141.     95  3.92  3.15  22.9     1     0     4     2
##  4  32.4  78.7    66  4.08  2.2   19.5     1     1     4     1
##  5  30.4  75.7    52  4.93  1.62  18.5     1     1     4     2
##  6  33.9  71.1    65  4.22  1.84  19.9     1     1     4     1
##  7  21.5 120.     97  3.7   2.46  20.0     1     0     3     1
##  8  27.3  79      66  4.08  1.94  18.9     1     1     4     1
##  9  26   120.     91  4.43  2.14  16.7     0     1     5     2
## 10  30.4  95.1   113  3.77  1.51  16.9     1     1     5     2
## 11  21.4 121     109  4.11  2.78  18.6     1     1     4     2
## 
## [[3]]
## # A tibble: 14 x 10
##      mpg  disp    hp  drat    wt  qsec    vs    am  gear  carb
##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
##  1  18.7  360    175  3.15  3.44  17.0     0     0     3     2
##  2  14.3  360    245  3.21  3.57  15.8     0     0     3     4
##  3  16.4  276.   180  3.07  4.07  17.4     0     0     3     3
##  4  17.3  276.   180  3.07  3.73  17.6     0     0     3     3
##  5  15.2  276.   180  3.07  3.78  18       0     0     3     3
##  6  10.4  472    205  2.93  5.25  18.0     0     0     3     4
##  7  10.4  460    215  3     5.42  17.8     0     0     3     4
##  8  14.7  440    230  3.23  5.34  17.4     0     0     3     4
##  9  15.5  318    150  2.76  3.52  16.9     0     0     3     2
## 10  15.2  304    150  3.15  3.44  17.3     0     0     3     2
## 11  13.3  350    245  3.73  3.84  15.4     0     0     3     4
## 12  19.2  400    175  3.08  3.84  17.0     0     0     3     2
## 13  15.8  351    264  4.22  3.17  14.5     0     1     5     4
## 14  15    301    335  3.54  3.57  14.6     0     1     5     8
```

```r
dois$model
```

```
## [[1]]
## 
## Call:
## lm(formula = mpg ~ wt, data = df)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## [[2]]
## 
## Call:
## lm(formula = mpg ~ wt, data = df)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## [[3]]
## 
## Call:
## lm(formula = mpg ~ wt, data = df)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

```r
dois$model[[3]]
```

```
## 
## Call:
## lm(formula = mpg ~ wt, data = df)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

```r
tres<- dois %>% 
  mutate(model = map(model, predict))

tres
```

```
## # A tibble: 3 x 3
## # Groups:   cyl [3]
##     cyl data               model     
##   <dbl> <list>             <list>    
## 1     6 <tibble [7 x 10]>  <dbl [7]> 
## 2     4 <tibble [11 x 10]> <dbl [11]>
## 3     8 <tibble [14 x 10]> <dbl [14]>
```

```r
tres$model
```

```
## [[1]]
##        1        2        3        4        5        6        7 
## 21.12497 20.41604 19.47080 18.78968 18.84528 18.84528 20.70795 
## 
## [[2]]
##        1        2        3        4        5        6        7        8 
## 26.47010 21.55719 21.78307 27.14774 30.45125 29.20890 25.65128 28.64420 
##        9       10       11 
## 27.48656 31.02725 23.87247 
## 
## [[3]]
##        1        2        3        4        5        6        7        8 
## 16.32604 16.04103 14.94481 15.69024 15.58061 12.35773 11.97625 12.14945 
##        9       10       11       12       13       14 
## 16.15065 16.33700 15.44907 15.43811 16.91800 16.04103
```

```r
tres$model[[3]]
```

```
##        1        2        3        4        5        6        7        8 
## 16.32604 16.04103 14.94481 15.69024 15.58061 12.35773 11.97625 12.14945 
##        9       10       11       12       13       14 
## 16.15065 16.33700 15.44907 15.43811 16.91800 16.04103
```

## unnest()


```r
um %>% 
  unnest(data)
```

```
## # A tibble: 32 x 14
## # Groups:   cyl [3]
##      cyl   mpg  disp    hp  drat    wt  qsec    vs    am  gear  carb linMod
##    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <list>
##  1     6  21    160    110  3.9   2.62  16.5     0     1     4     4 <lm>  
##  2     6  21    160    110  3.9   2.88  17.0     0     1     4     4 <lm>  
##  3     6  21.4  258    110  3.08  3.22  19.4     1     0     3     1 <lm>  
##  4     6  18.1  225    105  2.76  3.46  20.2     1     0     3     1 <lm>  
##  5     6  19.2  168.   123  3.92  3.44  18.3     1     0     4     4 <lm>  
##  6     6  17.8  168.   123  3.92  3.44  18.9     1     0     4     4 <lm>  
##  7     6  19.7  145    175  3.62  2.77  15.5     0     1     5     6 <lm>  
##  8     4  22.8  108     93  3.85  2.32  18.6     1     1     4     1 <lm>  
##  9     4  24.4  147.    62  3.69  3.19  20       1     0     4     2 <lm>  
## 10     4  22.8  141.    95  3.92  3.15  22.9     1     0     4     2 <lm>  
## # ... with 22 more rows, and 2 more variables: coeffs <list>, slope <dbl>
```


## Exemplos da ajuda do R


```r
df <- tibble(x = c(1, 1, 1, 2, 2, 3), 
             y = 1:6, 
             z = 6:1)
df
```

```
## # A tibble: 6 x 3
##       x     y     z
##   <dbl> <int> <int>
## 1     1     1     6
## 2     1     2     5
## 3     1     3     4
## 4     2     4     3
## 5     2     5     2
## 6     3     6     1
```

```r
# Note that we get one row of output for each unique combination of
# non-nested variables
df %>% 
  nest(data = c(y, z))
```

```
## # A tibble: 3 x 2
##       x data            
##   <dbl> <list>          
## 1     1 <tibble [3 x 2]>
## 2     2 <tibble [2 x 2]>
## 3     3 <tibble [1 x 2]>
```

```r
# chop does something similar, but retains individual columns
df %>% 
  chop(c(y, z))
```

```
## # A tibble: 3 x 3
##       x           y           z
##   <dbl> <list<int>> <list<int>>
## 1     1         [3]         [3]
## 2     2         [2]         [2]
## 3     3         [1]         [1]
```

```r
# use tidyselect syntax and helpers, just like in dplyr::select()
df %>% 
  nest(data = any_of(c("y", "z")))
```

```
## # A tibble: 3 x 2
##       x data            
##   <dbl> <list>          
## 1     1 <tibble [3 x 2]>
## 2     2 <tibble [2 x 2]>
## 3     3 <tibble [1 x 2]>
```

```r
iris %>% 
  nest(data = !Species)
```

```
## # A tibble: 3 x 2
##   Species    data             
##   <fct>      <list>           
## 1 setosa     <tibble [50 x 4]>
## 2 versicolor <tibble [50 x 4]>
## 3 virginica  <tibble [50 x 4]>
```

```r
nest_vars <- names(iris)[1:4]
iris %>% 
  nest(data = any_of(nest_vars))
```

```
## # A tibble: 3 x 2
##   Species    data             
##   <fct>      <list>           
## 1 setosa     <tibble [50 x 4]>
## 2 versicolor <tibble [50 x 4]>
## 3 virginica  <tibble [50 x 4]>
```

```r
iris %>%
  nest(petal = starts_with("Petal"), sepal = starts_with("Sepal"))
```

```
## # A tibble: 3 x 3
##   Species    petal             sepal            
##   <fct>      <list>            <list>           
## 1 setosa     <tibble [50 x 2]> <tibble [50 x 2]>
## 2 versicolor <tibble [50 x 2]> <tibble [50 x 2]>
## 3 virginica  <tibble [50 x 2]> <tibble [50 x 2]>
```

```r
iris %>%
  nest(width = contains("Width"), length = contains("Length"))
```

```
## # A tibble: 3 x 3
##   Species    width             length           
##   <fct>      <list>            <list>           
## 1 setosa     <tibble [50 x 2]> <tibble [50 x 2]>
## 2 versicolor <tibble [50 x 2]> <tibble [50 x 2]>
## 3 virginica  <tibble [50 x 2]> <tibble [50 x 2]>
```

```r
# Nesting a grouped data frame nests all variables apart from the group vars
fish_encounters %>%
  group_by(fish) %>%
  nest()
```

```
## # A tibble: 19 x 2
## # Groups:   fish [19]
##    fish  data             
##    <fct> <list>           
##  1 4842  <tibble [11 x 2]>
##  2 4843  <tibble [11 x 2]>
##  3 4844  <tibble [11 x 2]>
##  4 4845  <tibble [5 x 2]> 
##  5 4847  <tibble [3 x 2]> 
##  6 4848  <tibble [4 x 2]> 
##  7 4849  <tibble [2 x 2]> 
##  8 4850  <tibble [6 x 2]> 
##  9 4851  <tibble [2 x 2]> 
## 10 4854  <tibble [2 x 2]> 
## 11 4855  <tibble [5 x 2]> 
## 12 4857  <tibble [9 x 2]> 
## 13 4858  <tibble [11 x 2]>
## 14 4859  <tibble [5 x 2]> 
## 15 4861  <tibble [11 x 2]>
## 16 4862  <tibble [9 x 2]> 
## 17 4863  <tibble [2 x 2]> 
## 18 4864  <tibble [2 x 2]> 
## 19 4865  <tibble [3 x 2]>
```

```r
# Nesting is often useful for creating per group models
mtcars %>%
  group_by(cyl) %>%
  nest() %>%
  mutate(models = lapply(data, function(df) lm(mpg ~ wt, data = df)))
```

```
## # A tibble: 3 x 3
## # Groups:   cyl [3]
##     cyl data               models
##   <dbl> <list>             <list>
## 1     6 <tibble [7 x 10]>  <lm>  
## 2     4 <tibble [11 x 10]> <lm>  
## 3     8 <tibble [14 x 10]> <lm>
```

```r
# unnest() is primarily designed to work with lists of data frames
df <- tibble(
  x = 1:3,
  y = list(
    NULL,
    tibble(a = 1, b = 2),
    tibble(a = 1:3, b = 3:1)
  )
)

df %>% 
  unnest(y)
```

```
## # A tibble: 4 x 3
##       x     a     b
##   <int> <dbl> <dbl>
## 1     2     1     2
## 2     3     1     3
## 3     3     2     2
## 4     3     3     1
```

```r
df %>% 
  unnest(y, keep_empty = TRUE)
```

```
## # A tibble: 5 x 3
##       x     a     b
##   <int> <dbl> <dbl>
## 1     1    NA    NA
## 2     2     1     2
## 3     3     1     3
## 4     3     2     2
## 5     3     3     1
```

```r
# If you have lists of lists, or lists of atomic vectors, instead
# see hoist(), unnest_wider(), and unnest_longer()

#' # You can unnest multiple columns simultaneously
df <- tibble(
 a = list(c("a", "b"), "c"),
 b = list(1:2, 3),
 c = c(11, 22)
)
df
```

```
## # A tibble: 2 x 3
##   a         b             c
##   <list>    <list>    <dbl>
## 1 <chr [2]> <int [2]>    11
## 2 <chr [1]> <dbl [1]>    22
```

```r
df %>% 
  unnest(c(a, b))
```

```
## # A tibble: 3 x 3
##   a         b     c
##   <chr> <dbl> <dbl>
## 1 a         1    11
## 2 b         2    11
## 3 c         3    22
```

```r
# Compare with unnesting one column at a time, which generates
# the Cartesian product
df %>% 
  unnest(a) %>% 
  unnest(b)
```

```
## # A tibble: 5 x 3
##   a         b     c
##   <chr> <dbl> <dbl>
## 1 a         1    11
## 2 a         2    11
## 3 b         1    11
## 4 b         2    11
## 5 c         3    22
```

## Mais detalhes

[https://github.com/tidymodels/broom/blob/main/vignettes/broom_and_dplyr.Rmd](https://github.com/tidymodels/broom/blob/main/vignettes/broom_and_dplyr.Rmd)

