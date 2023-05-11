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
##  [1]  0.9847728  1.2134403  1.5271987  0.8699412  1.0326853  1.4969394
##  [7]  0.1967036  1.2745323  0.8581227 -0.4807560
## 
## [[2]]
##  [1] 1.954105 2.671244 2.618976 1.374043 2.967331 2.569375 1.547439 0.662164
##  [9] 3.292868 1.207899
## 
## [[3]]
##  [1] 3.982925 2.989138 3.476106 1.916062 3.991005 4.380055 3.181884 1.457460
##  [9] 2.561777 3.816446
## 
## [[4]]
##  [1] 3.910191 4.188865 4.890194 2.935118 3.096332 2.446448 4.216989 5.179807
##  [9] 2.974192 4.166398
## 
## [[5]]
##  [1] 3.846474 6.561309 5.894584 4.956071 3.497541 6.381848 5.840936 4.128073
##  [9] 4.713686 6.431420
## 
## [[6]]
##  [1] 6.070228 7.870752 6.489020 6.973389 6.849549 6.519332 5.967410 4.984029
##  [9] 6.683463 3.830970
## 
## [[7]]
##  [1] 6.481638 5.649669 8.548069 6.921537 6.464654 7.726066 6.475998 6.799747
##  [9] 6.169225 5.658955
## 
## [[8]]
##  [1] 9.359796 7.547208 7.810476 6.848805 8.074348 6.659517 9.657212 6.938745
##  [9] 9.529321 8.374576
## 
## [[9]]
##  [1] 10.410501  9.689270  8.524527  8.998205  9.130904 10.373338  8.687423
##  [8]  9.166574  8.966455 10.360590
## 
## [[10]]
##  [1] 10.435808  9.288908  9.668146  7.498355  9.559159 10.678296 10.324279
##  [8] 10.079509  9.707040  9.433631
```

```r
# You can also use an anonymous function
1:10 %>%
  map(function(x) rnorm(10, x))
```

```
## [[1]]
##  [1] 2.312638896 1.488565671 2.081679763 4.250274556 1.093323096 0.006087448
##  [7] 2.243602914 0.643165504 1.583823914 0.902019184
## 
## [[2]]
##  [1] 0.7957497 3.3901558 1.3464790 2.1211533 1.1673932 1.8070767 1.1887438
##  [8] 2.3783743 3.2269434 1.7585739
## 
## [[3]]
##  [1] 2.4897358 2.2679499 2.4456183 3.8764642 2.4859849 2.2495324 0.9942630
##  [8] 0.8884974 3.4104583 3.5567135
## 
## [[4]]
##  [1] 3.091265 2.420960 3.126583 4.907215 4.629660 4.338844 3.589269 3.654768
##  [9] 4.959958 5.116713
## 
## [[5]]
##  [1] 3.948429 4.559113 4.068604 7.650255 4.956375 6.943495 3.870947 4.772121
##  [9] 3.631234 4.917196
## 
## [[6]]
##  [1] 4.790284 6.635208 7.222999 5.474786 5.266004 6.712271 6.455652 5.591993
##  [9] 5.264093 5.841003
## 
## [[7]]
##  [1] 6.260580 6.829698 6.654538 6.641228 5.238742 5.029787 6.815748 8.586614
##  [9] 6.786050 8.854479
## 
## [[8]]
##  [1] 7.861666 8.658158 7.742446 7.541804 7.291984 8.218550 8.246015 8.691571
##  [9] 6.119368 7.908149
## 
## [[9]]
##  [1] 6.831690 8.127748 9.961263 9.622232 9.182766 9.195515 9.042667 7.781265
##  [9] 8.793952 9.323953
## 
## [[10]]
##  [1] 10.071423  9.653958  9.676890 11.486766  9.848096 10.973562 10.593814
##  [8] 10.007440  9.262317 10.386423
```

```r
# Or a formula
1:10 %>%
  map(~ rnorm(10, .x))
```

```
## [[1]]
##  [1]  1.46809093  0.81892040 -0.27699258  1.15883547 -1.27560745  0.66559751
##  [7]  1.79466289  0.37143972  1.83715287  0.02653139
## 
## [[2]]
##  [1] 1.5005557 2.5132414 3.1335278 3.2841949 0.2942115 2.0017352 2.1382454
##  [8] 2.7290475 1.5498179 3.0207087
## 
## [[3]]
##  [1] 2.239764 2.655934 3.271333 2.074757 0.579506 3.292257 4.267165 3.338194
##  [9] 5.046800 3.526191
## 
## [[4]]
##  [1] 4.741782 5.018736 3.931588 4.270694 3.530900 5.256601 3.266689 2.898363
##  [9] 5.059677 4.036169
## 
## [[5]]
##  [1] 4.907031 6.329643 4.765220 5.938428 3.772819 4.603587 4.584548 4.182335
##  [9] 5.338289 6.187431
## 
## [[6]]
##  [1] 6.215457 5.511327 6.836773 6.860728 6.350363 6.572379 4.649024 7.065337
##  [9] 6.634251 6.566702
## 
## [[7]]
##  [1] 5.706115 7.068877 6.786134 7.035794 5.322005 6.772898 5.443521 8.518114
##  [9] 7.239341 6.678006
## 
## [[8]]
##  [1] 8.040349 6.472084 7.818219 8.324279 9.113741 7.244781 7.874199 7.807287
##  [9] 5.498444 7.880996
## 
## [[9]]
##  [1]  7.247952  9.014034 10.576555  8.028198  9.808327  9.428881  9.242694
##  [8] 10.694102  8.700257  9.616496
## 
## [[10]]
##  [1]  9.349160  9.500872  9.292084  8.530186 10.761558  8.570281 10.077362
##  [8] 10.161438  8.972482  9.140428
```

```r
# Simplify output to a vector instead of a list by computing the mean of the distributions
1:10 %>%
  map(rnorm, n = 10) %>%  # output a list
  map_dbl(mean)           # output an atomic vector
```

```
##  [1] 1.320401 1.779741 2.756991 4.020664 4.460907 5.857932 6.608631 7.997054
##  [9] 8.822456 9.815341
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
