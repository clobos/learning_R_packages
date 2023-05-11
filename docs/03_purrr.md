# purrr



```r
library(tidyverse)
ls("package:purrr")
```

```
##   [1] "%@%"                 "%||%"                "%>%"                
##   [4] "accumulate"          "accumulate_right"    "accumulate2"        
##   [7] "array_branch"        "array_tree"          "as_function"        
##  [10] "as_mapper"           "as_vector"           "assign_in"          
##  [13] "at_depth"            "attr_getter"         "auto_browse"        
##  [16] "chuck"               "compact"             "compose"            
##  [19] "cross"               "cross_d"             "cross_df"           
##  [22] "cross_n"             "cross2"              "cross3"             
##  [25] "detect"              "detect_index"        "discard"            
##  [28] "done"                "every"               "exec"               
##  [31] "flatten"             "flatten_chr"         "flatten_dbl"        
##  [34] "flatten_df"          "flatten_dfc"         "flatten_dfr"        
##  [37] "flatten_int"         "flatten_lgl"         "flatten_raw"        
##  [40] "has_element"         "head_while"          "imap"               
##  [43] "imap_chr"            "imap_dbl"            "imap_dfc"           
##  [46] "imap_dfr"            "imap_int"            "imap_lgl"           
##  [49] "imap_raw"            "imodify"             "insistently"        
##  [52] "invoke"              "invoke_map"          "invoke_map_chr"     
##  [55] "invoke_map_dbl"      "invoke_map_df"       "invoke_map_dfc"     
##  [58] "invoke_map_dfr"      "invoke_map_int"      "invoke_map_lgl"     
##  [61] "invoke_map_raw"      "is_atomic"           "is_bare_atomic"     
##  [64] "is_bare_character"   "is_bare_double"      "is_bare_integer"    
##  [67] "is_bare_list"        "is_bare_logical"     "is_bare_numeric"    
##  [70] "is_bare_vector"      "is_character"        "is_double"          
##  [73] "is_empty"            "is_formula"          "is_function"        
##  [76] "is_integer"          "is_list"             "is_logical"         
##  [79] "is_null"             "is_numeric"          "is_rate"            
##  [82] "is_scalar_atomic"    "is_scalar_character" "is_scalar_double"   
##  [85] "is_scalar_integer"   "is_scalar_list"      "is_scalar_logical"  
##  [88] "is_scalar_numeric"   "is_scalar_vector"    "is_vector"          
##  [91] "iwalk"               "keep"                "lift"               
##  [94] "lift_dl"             "lift_dv"             "lift_ld"            
##  [97] "lift_lv"             "lift_vd"             "lift_vl"            
## [100] "list_along"          "list_merge"          "list_modify"        
## [103] "lmap"                "lmap_at"             "lmap_if"            
## [106] "map"                 "map_at"              "map_call"           
## [109] "map_chr"             "map_dbl"             "map_depth"          
## [112] "map_df"              "map_dfc"             "map_dfr"            
## [115] "map_if"              "map_int"             "map_lgl"            
## [118] "map_raw"             "map2"                "map2_chr"           
## [121] "map2_dbl"            "map2_df"             "map2_dfc"           
## [124] "map2_dfr"            "map2_int"            "map2_lgl"           
## [127] "map2_raw"            "modify"              "modify_at"          
## [130] "modify_depth"        "modify_if"           "modify_in"          
## [133] "modify2"             "negate"              "none"               
## [136] "partial"             "pluck"               "pluck<-"            
## [139] "pmap"                "pmap_chr"            "pmap_dbl"           
## [142] "pmap_df"             "pmap_dfc"            "pmap_dfr"           
## [145] "pmap_int"            "pmap_lgl"            "pmap_raw"           
## [148] "possibly"            "prepend"             "pwalk"              
## [151] "quietly"             "rate_backoff"        "rate_delay"         
## [154] "rate_reset"          "rate_sleep"          "rbernoulli"         
## [157] "rdunif"              "reduce"              "reduce_right"       
## [160] "reduce2"             "reduce2_right"       "rep_along"          
## [163] "rerun"               "safely"              "set_names"          
## [166] "simplify"            "simplify_all"        "slowly"             
## [169] "some"                "splice"              "tail_while"         
## [172] "transpose"           "update_list"         "vec_depth"          
## [175] "walk"                "walk2"               "when"               
## [178] "zap"
```

## Apply a function to each element of a list or atomic vector

> The map functions transform their input by applying a function to each element of a list or atomic vector and returning an object of the same length as the input.

  - map() always returns a list. See the modify() family for versions that return an object of the same type as the input.

  - map_lgl(), map_int(), map_dbl() and map_chr() return an atomic vector of the indicated type (or die trying).

  - map_dfr() and map_dfc() return a data frame created by row-binding and column-binding respectively. They require dplyr to be installed.

  - The returned values of .f must be of length one for each element of .x. If .f uses an extractor function shortcut, .default can be specified to handle values that are absent or empty. See as_mapper() for more on .default.

  - walk() calls .f for its side-effect and returns the input .x.

### Usage

  - map(.x, .f, ...)

  - map_lgl(.x, .f, ...)

  - map_chr(.x, .f, ...)

  - map_int(.x, .f, ...)

  - map_dbl(.x, .f, ...)

  - map_raw(.x, .f, ...)

  - map_dfr(.x, .f, ..., .id = NULL)

  - map_dfc(.x, .f, ...)

  - walk(.x, .f, ...)
  

### Arguments

  - .x	A list or atomic vector.

  - .f	A function, formula, or vector (not necessarily atomic).

  If a function, it is used as is.

  If a formula, e.g. ~ .x + 2, it is converted to a function. There are three ways to refer to the arguments:

  - For a single argument function, use .

  - For a two argument function, use .x and .y

  - For more arguments, use ..1, ..2, ..3 etc

This syntax allows you to create very compact anonymous functions.

If character vector, numeric vector, or list, it is converted to an extractor function. Character vectors index by name and numeric vectors index by position; use a list to index by position and name at different levels. If a component is not present, the value of .default will be returned.

  - ...	Additional arguments passed on to the mapped function.

  - .id	Either a string or NULL. If a string, the output will contain a variable with that name, storing either the name (if .x is named) or the index (if .x is unnamed) of the input. If NULL, the default, no variable will be created.

Only applies to ⁠_dfr⁠ variant.

### Value

  - map() Returns a list the same length as .x.

  - map_lgl() returns a logical vector, map_int() an integer vector, map_dbl() a double vector, and map_chr() a character vector.

  - map_df(), map_dfc(), map_dfr() all return a data frame.

  - If .x has names(), the return value preserves those names.

  - The output of .f will be automatically typed upwards, e.g. logical -> integer -> double -> character.

  - walk() returns the input .x (invisibly). This makes it easy to use in pipe.

### See Also

map_if() for applying a function to only those elements of .x that meet a specified condition.

Other map variants: imap(), invoke(), lmap(), map2(), map_if(), modify()

## Examples


```r
# Compute normal distributions from an atomic vector
1:10 %>%
  map(rnorm, n = 10)
```

```
## [[1]]
##  [1] -0.2510138  1.3663339  2.0318617  1.0593567 -1.2458543  2.3792939
##  [7] -0.1092037  0.7742969  0.4561258  0.9740845
## 
## [[2]]
##  [1] 2.7321088 3.0131228 1.7510829 1.5556041 2.6261215 0.5609750 0.5542788
##  [8] 3.0671672 2.6869903 1.3212537
## 
## [[3]]
##  [1] 1.499728 3.283436 2.152757 4.372840 2.196048 4.004197 2.027555 3.741894
##  [9] 2.975770 2.745027
## 
## [[4]]
##  [1] 2.463953 3.463641 2.436951 4.530203 3.555637 5.856303 3.682413 4.633489
##  [9] 4.759220 3.713782
## 
## [[5]]
##  [1] 4.560828 6.308513 4.131827 6.146918 4.753959 5.112233 3.301413 4.661634
##  [9] 6.773473 5.444516
## 
## [[6]]
##  [1] 6.412483 4.585445 6.277479 5.865835 4.473993 6.953530 4.917721 5.749378
##  [9] 6.971929 6.954632
## 
## [[7]]
##  [1]  4.834337  4.835345  7.317229  7.417334  7.222412  8.800412 10.106278
##  [8]  7.106012  5.163386  6.654080
## 
## [[8]]
##  [1] 7.961968 8.906424 6.956105 7.037046 7.785741 6.649681 6.920087 9.943176
##  [9] 7.697148 9.717656
## 
## [[9]]
##  [1]  8.152028  8.837104  9.798076 10.128146  9.661960  9.254124  9.148925
##  [8]  9.265534  9.519678  9.989440
## 
## [[10]]
##  [1] 10.061106 10.331340  9.871614  9.675968  9.550376  9.576885  9.525325
##  [8] 11.558782 10.431002 10.319936
```

```r
# You can also use an anonymous function
1:10 %>%
  map(function(x) rnorm(10, x))
```

```
## [[1]]
##  [1] 1.7123682 0.1065350 1.1595637 0.4087544 2.0348150 1.3389534 1.5407350
##  [8] 1.3270952 1.3982069 2.2468535
## 
## [[2]]
##  [1] 3.2042425 2.2652217 3.5720600 1.1629091 2.4030318 0.8276631 2.8985715
##  [8] 2.5569807 2.4387543 2.9815737
## 
## [[3]]
##  [1] 3.217796 4.462182 3.716869 2.510780 4.074443 1.013212 2.501519 2.106531
##  [9] 3.012910 2.934018
## 
## [[4]]
##  [1] 4.345001 2.945592 2.908201 4.461540 3.598564 4.911316 3.205682 5.043623
##  [9] 2.078730 4.074328
## 
## [[5]]
##  [1] 5.845767 5.332237 4.754968 5.249908 3.715502 3.017545 5.896302 5.507187
##  [9] 5.930520 6.294953
## 
## [[6]]
##  [1] 6.655671 6.075838 7.324229 7.768052 4.943801 4.462991 6.527679 7.133577
##  [9] 6.262455 6.248165
## 
## [[7]]
##  [1] 6.156965 7.300594 7.295088 5.821933 7.640714 6.787728 6.298466 8.942717
##  [9] 7.496085 8.685640
## 
## [[8]]
##  [1] 7.813504 9.456155 7.070994 8.159051 7.067865 7.209916 7.429870 8.053365
##  [9] 7.829721 7.385483
## 
## [[9]]
##  [1] 9.634875 8.795818 9.939768 9.531589 9.084126 8.697142 8.810479 9.329292
##  [9] 7.009637 8.639508
## 
## [[10]]
##  [1] 11.997331 10.971319  9.427018 10.034238 11.038642 10.627948  9.026038
##  [8] 10.442100  8.877136  7.431340
```

```r
# Or a formula
1:10 %>%
  map(~ rnorm(10, .x))
```

```
## [[1]]
##  [1] 1.025849 2.558404 1.755915 2.193397 1.939269 1.309701 0.503807 2.063347
##  [9] 0.723649 0.985416
## 
## [[2]]
##  [1] 0.6616534 3.4561801 1.2389847 0.8602657 1.1741932 0.8186269 3.9167699
##  [8] 1.9265003 1.4348158 1.8587782
## 
## [[3]]
##  [1] 2.8309069 2.8045459 4.3017234 3.9404368 0.1490951 3.1553623 4.3528047
##  [8] 4.7535664 0.9901394 3.3861543
## 
## [[4]]
##  [1] 4.122743 3.938384 3.886257 3.429402 4.826656 4.192718 2.349485 3.376756
##  [9] 3.995786 2.030279
## 
## [[5]]
##  [1] 5.027699 6.376929 5.662397 5.561232 5.195049 4.742073 6.040439 4.684147
##  [9] 5.259453 4.522845
## 
## [[6]]
##  [1] 7.362841 6.254675 6.563096 6.159555 7.081791 5.104524 6.256664 5.940224
##  [9] 5.855206 7.485169
## 
## [[7]]
##  [1] 6.666676 5.727916 7.740839 5.716449 4.749752 7.348871 8.843951 6.014475
##  [9] 8.409513 6.692473
## 
## [[8]]
##  [1] 7.741570 7.847221 7.559155 5.722923 8.734690 9.823998 7.971953 8.260075
##  [9] 8.195070 6.708963
## 
## [[9]]
##  [1] 9.573037 9.698491 9.586603 8.266991 7.571689 8.733963 7.098599 8.270489
##  [9] 7.811114 8.546880
## 
## [[10]]
##  [1]  9.731846 12.090286  9.943375 10.519790 10.159346 11.087403 11.200392
##  [8]  9.028120 10.590912  9.121794
```

```r
# Simplify output to a vector instead of a list by computing the mean of the distributions
1:10 %>%
  map(rnorm, n = 10) %>%  # output a list
  map_dbl(mean)           # output an atomic vector
```

```
##  [1] 1.239432 1.855887 2.593301 4.045615 4.909148 5.964231 6.936763 7.985706
##  [9] 8.480859 9.886918
```

```r
# Using set_names() with character vectors is handy to keep track
# of the original inputs:
set_names(c("foo", "bar")) %>% 
  map_chr(paste0, ":suffix")
```

```
##          foo          bar 
## "foo:suffix" "bar:suffix"
```

```r
# Working with lists
favorite_desserts <- list(Sophia = "banana bread", Eliott = "pancakes", Karina = "chocolate cake")
favorite_desserts
```

```
## $Sophia
## [1] "banana bread"
## 
## $Eliott
## [1] "pancakes"
## 
## $Karina
## [1] "chocolate cake"
```

```r
favorite_desserts %>% 
  map_chr(~ paste(.x, "rocks!"))
```

```
##                  Sophia                  Eliott                  Karina 
##   "banana bread rocks!"       "pancakes rocks!" "chocolate cake rocks!"
```

```r
# Extract by name or position
# .default specifies value for elements that are missing or NULL
l1 <- list(list(a = 1L), 
           list(a = NULL, b = 2L), 
           list(b = 3L))
l1
```

```
## [[1]]
## [[1]]$a
## [1] 1
## 
## 
## [[2]]
## [[2]]$a
## NULL
## 
## [[2]]$b
## [1] 2
## 
## 
## [[3]]
## [[3]]$b
## [1] 3
```

```r
l1 %>% 
  map("a", .default = "???")
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] "???"
## 
## [[3]]
## [1] "???"
```

```r
l1 %>% 
  map_int("b", .default = NA)
```

```
## [1] NA  2  3
```

```r
l1 %>% 
  map_int(2, .default = NA)
```

```
## [1] NA  2 NA
```

```r
# Supply multiple values to index deeply into a list
l2 <- list(
  list(num = 1:3,     letters[1:3]),
  list(num = 101:103, letters[4:6]),
  list())
l2
```

```
## [[1]]
## [[1]]$num
## [1] 1 2 3
## 
## [[1]][[2]]
## [1] "a" "b" "c"
## 
## 
## [[2]]
## [[2]]$num
## [1] 101 102 103
## 
## [[2]][[2]]
## [1] "d" "e" "f"
## 
## 
## [[3]]
## list()
```

```r
l2 %>% 
  map(c(2, 2))
```

```
## [[1]]
## [1] "b"
## 
## [[2]]
## [1] "e"
## 
## [[3]]
## NULL
```

```r
# Use a list to build an extractor that mixes numeric indices and names,
# and .default to provide a default value if the element does not exist
l2 %>% 
  map(list("num", 3))
```

```
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 103
## 
## [[3]]
## NULL
```

```r
l2 %>% 
  map_int(list("num", 3), .default = NA)
```

```
## [1]   3 103  NA
```

```r
# Working with data frames
# Use map_lgl(), map_dbl(), etc to return a vector instead of a list:
mtcars %>% 
  map_dbl(sum)
```

```
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  642.900  198.000 7383.100 4694.000  115.090  102.952  571.160   14.000 
##       am     gear     carb 
##   13.000  118.000   90.000
```

```r
# A more realistic example: split a data frame into pieces, fit a
# model to each piece, summarise and extract R^2

mtcars %>% 
  select(cyl) 
```

```
##                     cyl
## Mazda RX4             6
## Mazda RX4 Wag         6
## Datsun 710            4
## Hornet 4 Drive        6
## Hornet Sportabout     8
## Valiant               6
## Duster 360            8
## Merc 240D             4
## Merc 230              4
## Merc 280              6
## Merc 280C             6
## Merc 450SE            8
## Merc 450SL            8
## Merc 450SLC           8
## Cadillac Fleetwood    8
## Lincoln Continental   8
## Chrysler Imperial     8
## Fiat 128              4
## Honda Civic           4
## Toyota Corolla        4
## Toyota Corona         4
## Dodge Challenger      8
## AMC Javelin           8
## Camaro Z28            8
## Pontiac Firebird      8
## Fiat X1-9             4
## Porsche 914-2         4
## Lotus Europa          4
## Ford Pantera L        8
## Ferrari Dino          6
## Maserati Bora         8
## Volvo 142E            4
```

```r
mtcars %>%
  split(.$cyl) 
```

```
## $`4`
##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
## 
## $`6`
##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Valiant        18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
## Merc 280       19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
## Merc 280C      17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
## Ferrari Dino   19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
## 
## $`8`
##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
```

```r
mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .x)) 
```

```
## $`4`
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Coefficients:
## (Intercept)           wt  
##      39.571       -5.647  
## 
## 
## $`6`
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Coefficients:
## (Intercept)           wt  
##       28.41        -2.78  
## 
## 
## $`8`
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Coefficients:
## (Intercept)           wt  
##      23.868       -2.192
```

```r
mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .x)) %>%
  map(summary) 
```

```
## $`4`
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.1513 -1.9795 -0.6272  1.9299  5.2523 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   39.571      4.347   9.104 7.77e-06 ***
## wt            -5.647      1.850  -3.052   0.0137 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.332 on 9 degrees of freedom
## Multiple R-squared:  0.5086,	Adjusted R-squared:  0.454 
## F-statistic: 9.316 on 1 and 9 DF,  p-value: 0.01374
## 
## 
## $`6`
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Residuals:
##      Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
##        -0.1250         0.5840         1.9292        -0.6897         0.3547 
##      Merc 280C   Ferrari Dino 
##        -1.0453        -1.0080 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)   
## (Intercept)   28.409      4.184   6.789  0.00105 **
## wt            -2.780      1.335  -2.083  0.09176 . 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.165 on 5 degrees of freedom
## Multiple R-squared:  0.4645,	Adjusted R-squared:  0.3574 
## F-statistic: 4.337 on 1 and 5 DF,  p-value: 0.09176
## 
## 
## $`8`
## 
## Call:
## lm(formula = mpg ~ wt, data = .x)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.1491 -1.4664 -0.8458  1.5711  3.7619 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  23.8680     3.0055   7.942 4.05e-06 ***
## wt           -2.1924     0.7392  -2.966   0.0118 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.024 on 12 degrees of freedom
## Multiple R-squared:  0.423,	Adjusted R-squared:  0.3749 
## F-statistic: 8.796 on 1 and 12 DF,  p-value: 0.01179
```

```r
# original  
  mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .x)) %>%
  map(summary) %>%
  map_dbl("r.squared")
```

```
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
# If each element of the output is a data frame, use
# map_dfr to row-bind them together:
mtcars %>%
  split(.$cyl) %>%
  map(~ lm(mpg ~ wt, data = .x)) %>%
  map_dfr(~ as.data.frame(t(as.matrix(coef(.)))))
```

```
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
```

```r
# (if you also want to preserve the variable names see
# the broom package)


#nest, unest() estudar!


mtcars %>% 
  group_by(cyl) %>% 
  nest()
```

```
## # A tibble: 3 × 2
## # Groups:   cyl [3]
##     cyl data              
##   <dbl> <list>            
## 1     6 <tibble [7 × 10]> 
## 2     4 <tibble [11 × 10]>
## 3     8 <tibble [14 × 10]>
```

```r
#mtcars %>% 
#  group_by(cyl) %>% 
#  nest() %>%
#  map(~ lm(mpg ~ wt, data = .x)) 
```

## map functions


```r
example("map")
example("map_at")
example("map_chr")
example("map_dbl")
example("map_df")
example("map_dfc")
example("map_dfr")
example("map_int")
example("map_lgl")
example("map_vec")
```

## map2 functions


```r
example("map2")
example("map2_chr")
example("map2_dbl")
example("map2_df")
example("map2_dfc")
example("map2_dfr")
example("map2_int")
example("map2_lgl")
example("map2_raw")
example("map2_vec")
```
