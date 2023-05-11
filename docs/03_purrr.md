# purrr



```r
library(tidyverse)
ls("package:purrr")
```

```
##   [1] "%@%"                 "%||%"                "%>%"                
##   [4] "accumulate"          "accumulate_right"    "accumulate2"        
##   [7] "array_branch"        "array_tree"          "as_mapper"          
##  [10] "as_vector"           "assign_in"           "at_depth"           
##  [13] "attr_getter"         "auto_browse"         "chuck"              
##  [16] "compact"             "compose"             "cross"              
##  [19] "cross_d"             "cross_df"            "cross_n"            
##  [22] "cross2"              "cross3"              "detect"             
##  [25] "detect_index"        "discard"             "discard_at"         
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
##  [79] "is_null"             "is_rate"             "is_scalar_atomic"   
##  [82] "is_scalar_character" "is_scalar_double"    "is_scalar_integer"  
##  [85] "is_scalar_list"      "is_scalar_logical"   "is_scalar_vector"   
##  [88] "is_vector"           "iwalk"               "keep"               
##  [91] "keep_at"             "lift"                "lift_dl"            
##  [94] "lift_dv"             "lift_ld"             "lift_lv"            
##  [97] "lift_vd"             "lift_vl"             "list_along"         
## [100] "list_assign"         "list_c"              "list_cbind"         
## [103] "list_flatten"        "list_merge"          "list_modify"        
## [106] "list_rbind"          "list_simplify"       "list_transpose"     
## [109] "lmap"                "lmap_at"             "lmap_if"            
## [112] "map"                 "map_at"              "map_chr"            
## [115] "map_dbl"             "map_depth"           "map_df"             
## [118] "map_dfc"             "map_dfr"             "map_if"             
## [121] "map_int"             "map_lgl"             "map_raw"            
## [124] "map_vec"             "map2"                "map2_chr"           
## [127] "map2_dbl"            "map2_df"             "map2_dfc"           
## [130] "map2_dfr"            "map2_int"            "map2_lgl"           
## [133] "map2_raw"            "map2_vec"            "modify"             
## [136] "modify_at"           "modify_depth"        "modify_if"          
## [139] "modify_in"           "modify_tree"         "modify2"            
## [142] "negate"              "none"                "partial"            
## [145] "pluck"               "pluck_depth"         "pluck_exists"       
## [148] "pluck<-"             "pmap"                "pmap_chr"           
## [151] "pmap_dbl"            "pmap_df"             "pmap_dfc"           
## [154] "pmap_dfr"            "pmap_int"            "pmap_lgl"           
## [157] "pmap_raw"            "pmap_vec"            "possibly"           
## [160] "prepend"             "pwalk"               "quietly"            
## [163] "rate_backoff"        "rate_delay"          "rate_reset"         
## [166] "rate_sleep"          "rbernoulli"          "rdunif"             
## [169] "reduce"              "reduce_right"        "reduce2"            
## [172] "reduce2_right"       "rep_along"           "rerun"              
## [175] "safely"              "set_names"           "simplify"           
## [178] "simplify_all"        "slowly"              "some"               
## [181] "splice"              "tail_while"          "transpose"          
## [184] "update_list"         "vec_depth"           "walk"               
## [187] "walk2"               "when"                "zap"
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
##  [1]  0.9606835  0.2558870  1.3668572  1.1732371  0.2350102  2.6521804
##  [7]  0.8214040  1.9771913 -0.6155543 -0.9988018
## 
## [[2]]
##  [1] 0.9377754 0.9214704 2.8330264 3.0352480 0.5238653 2.5443251 2.2924893
##  [8] 2.7829582 2.4419409 0.3743757
## 
## [[3]]
##  [1] 1.879637 3.818392 2.890525 3.872515 3.140802 2.768388 2.584703 2.922238
##  [9] 2.616681 4.430572
## 
## [[4]]
##  [1] 3.567559 4.349109 3.615555 5.362684 2.652931 5.404148 3.194384 2.184058
##  [9] 3.673871 5.500680
## 
## [[5]]
##  [1] 5.036916 4.557861 3.973224 6.193965 4.280406 5.230274 5.726284 5.856957
##  [9] 5.082852 3.858142
## 
## [[6]]
##  [1] 4.656112 7.127457 7.521381 6.674265 6.095799 6.279487 6.779528 6.858636
##  [9] 6.285432 8.188434
## 
## [[7]]
##  [1] 7.006924 7.218360 9.174065 7.025654 5.990612 7.551149 8.043741 7.910483
##  [9] 8.426142 6.625891
## 
## [[8]]
##  [1]  8.002099  8.256513  6.616554  9.059241  7.288628  7.784038 10.333387
##  [8]  7.588237  8.447337  7.238239
## 
## [[9]]
##  [1]  7.935737  8.246600 10.050744  7.661163  8.686803  6.817224 10.071338
##  [8]  9.372334  7.385286  9.223673
## 
## [[10]]
##  [1]  9.330173  9.315890 11.722364 10.076962  8.569037 10.816171  9.890369
##  [8] 10.161575 12.456419  9.847980
```

```r
# You can also use an anonymous function
1:10 %>%
  map(function(x) rnorm(10, x))
```

```
## [[1]]
##  [1]  2.4355148  1.3161904 -0.3844751  0.2743863  0.6748907  1.7813773
##  [7]  2.4774534  0.9030152  2.0938987  0.7016563
## 
## [[2]]
##  [1] 4.134756 2.155219 2.612060 2.072941 2.212364 1.161664 3.075493 2.425679
##  [9] 2.495402 1.971725
## 
## [[3]]
##  [1] 3.486822 2.451827 1.297666 2.291035 4.398815 3.260149 3.528562 1.261258
##  [9] 4.190041 4.215717
## 
## [[4]]
##  [1] 5.009319 4.773983 4.106116 3.835846 3.576150 2.326984 5.184813 3.810730
##  [9] 2.117968 3.568571
## 
## [[5]]
##  [1] 3.414407 4.265428 5.562993 5.277723 5.809059 4.492726 6.804638 3.683302
##  [9] 6.856712 4.673499
## 
## [[6]]
##  [1] 6.501189 4.649399 3.941000 7.249335 6.460261 7.380625 6.432399 7.486122
##  [9] 5.982924 7.892294
## 
## [[7]]
##  [1] 5.963699 7.444964 6.695306 6.825283 7.318234 4.847514 7.119791 7.097203
##  [9] 7.174268 6.752557
## 
## [[8]]
##  [1] 7.624474 7.737118 7.432223 8.107198 6.294585 7.416914 8.153201 8.554924
##  [9] 7.239460 7.551029
## 
## [[9]]
##  [1]  9.714895  8.727867 10.330333 10.059361  7.865472  8.657559 11.955671
##  [8]  8.627070  9.512751  9.812425
## 
## [[10]]
##  [1]  9.475433 10.877835  9.728579 11.468436 11.286864  9.036773  9.359317
##  [8]  8.569081 11.450863  9.169504
```

```r
# Or a formula
1:10 %>%
  map(~ rnorm(10, .x))
```

```
## [[1]]
##  [1]  3.0076519  1.3518697  0.7273344  0.3630838  0.5742323  0.1116815
##  [7] -0.3309606  2.6579127 -0.6714084 -0.5835447
## 
## [[2]]
##  [1] 1.8177842 3.0601065 1.4946339 3.3512698 4.0715896 1.9266118 0.1991496
##  [8] 1.2843011 3.1614962 2.6488691
## 
## [[3]]
##  [1] 2.0150944 3.3520639 4.3315523 0.8447573 3.3162547 4.2627852 2.7075133
##  [8] 2.9769775 2.1460154 1.7398364
## 
## [[4]]
##  [1] 2.581916 4.808275 3.368369 4.329507 3.458508 4.789342 2.740603 3.141206
##  [9] 3.398164 5.694223
## 
## [[5]]
##  [1] 7.134640 4.254732 5.143223 4.848392 6.329070 4.984556 6.355305 4.787746
##  [9] 4.854475 3.621560
## 
## [[6]]
##  [1] 6.668563 6.849455 5.349847 6.371768 5.304878 8.179186 5.507334 4.445319
##  [9] 7.270777 7.253198
## 
## [[7]]
##  [1] 6.663330 9.986253 6.115810 6.244033 5.867123 7.941277 7.945039 8.765729
##  [9] 7.763603 7.744577
## 
## [[8]]
##  [1] 6.909167 7.448366 5.721610 8.387959 7.137138 7.407341 9.087489 8.648508
##  [9] 6.143680 8.053039
## 
## [[9]]
##  [1] 5.729458 9.210299 7.224728 9.911272 8.154753 9.608185 8.063629 7.206674
##  [9] 8.543258 9.196022
## 
## [[10]]
##  [1] 10.242230  9.523368  9.630883  9.686558  9.136119 11.507728  8.930809
##  [8]  9.987174 10.816272  9.131987
```

```r
# Simplify output to a vector instead of a list by computing the mean of the distributions
1:10 %>%
  map(rnorm, n = 10) %>%  # output a list
  map_dbl(mean)           # output an atomic vector
```

```
##  [1] 0.6709021 2.2725584 3.0476521 3.9502926 4.3599626 6.3029332 7.4379788
##  [8] 8.0346655 8.6689626 9.8911214
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
## # A tibble: 3 x 2
## # Groups:   cyl [3]
##     cyl data              
##   <dbl> <list>            
## 1     6 <tibble [7 x 10]> 
## 2     4 <tibble [11 x 10]>
## 3     8 <tibble [14 x 10]>
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
