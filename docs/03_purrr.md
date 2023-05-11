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
##  [1]  0.6339724  2.7880373 -0.1984222  1.1442054  0.7124724  1.2276084
##  [7]  1.6474717  1.0929991 -0.6985577  1.4222392
## 
## [[2]]
##  [1] 2.133486 2.629332 1.065583 1.809737 2.560628 2.689161 1.176797 1.436152
##  [9] 1.966085 2.364917
## 
## [[3]]
##  [1] 1.100445 2.781537 2.299967 2.580975 2.761169 2.844217 2.702150 3.707112
##  [9] 2.153652 3.114340
## 
## [[4]]
##  [1] 3.198181 5.492960 3.899454 5.824106 4.974750 4.306030 4.521728 4.913784
##  [9] 6.094295 2.242841
## 
## [[5]]
##  [1] 5.930144 5.108468 6.096394 5.757209 4.764078 3.975482 7.795530 4.206314
##  [9] 4.938812 4.420335
## 
## [[6]]
##  [1] 6.463582 6.880319 6.300040 6.451217 8.143553 5.230962 5.460726 6.625071
##  [9] 7.612566 6.368246
## 
## [[7]]
##  [1] 8.293561 5.829829 6.865768 5.770135 7.291877 6.921683 5.799535 6.156567
##  [9] 6.752780 8.158770
## 
## [[8]]
##  [1] 7.038720 8.869194 8.518592 7.817315 6.673090 7.987795 8.056667 6.852389
##  [9] 9.615797 6.401303
## 
## [[9]]
##  [1] 9.620093 7.623280 9.325927 9.210802 9.204712 8.556545 8.931994 9.970865
##  [9] 7.602060 7.525370
## 
## [[10]]
##  [1] 10.142422  9.976939  9.624969  9.196717 11.881371  9.223565  7.831989
##  [8] 10.022924 10.202374  9.375127
```

```r
# You can also use an anonymous function
1:10 %>%
  map(function(x) rnorm(10, x))
```

```
## [[1]]
##  [1] 1.4242364 1.5871954 1.9318785 2.8434393 1.3602100 0.2791915 0.7598485
##  [8] 2.3902681 1.1827565 1.0402025
## 
## [[2]]
##  [1]  1.9331236  0.9889410  1.3633294  2.7567648  2.1605803 -0.1928810
##  [7]  1.5173740  4.1824461  0.3918547  2.6427651
## 
## [[3]]
##  [1] 1.833696 3.731865 2.035081 2.670794 2.587494 3.623561 3.397302 1.174053
##  [9] 2.521653 3.231178
## 
## [[4]]
##  [1] 3.322055 3.300941 2.778499 2.397965 3.612766 3.487014 4.929250 5.088878
##  [9] 3.720418 4.326392
## 
## [[5]]
##  [1] 6.992499 4.047125 4.791362 4.609602 6.009338 5.774938 5.733584 4.357322
##  [9] 4.755341 5.405129
## 
## [[6]]
##  [1] 4.442480 6.916796 6.436412 7.281615 6.459011 5.833078 5.170422 5.808270
##  [9] 6.636652 6.574892
## 
## [[7]]
##  [1] 4.631180 6.212651 5.914558 7.782220 6.864881 8.121306 7.271194 5.879798
##  [9] 6.159553 7.419076
## 
## [[8]]
##  [1] 6.124578 8.142177 7.150341 7.655491 7.855576 8.156402 6.106497 7.450578
##  [9] 6.997167 7.921414
## 
## [[9]]
##  [1]  8.456928  8.259565  8.654143 11.275767  8.942627 11.686104 10.876411
##  [8]  8.816997  7.241138  7.681535
## 
## [[10]]
##  [1] 10.420855  9.895795 10.549959 11.329046 10.400282 10.658443 12.107371
##  [8] 10.350309 11.412465 10.551717
```

```r
# Or a formula
1:10 %>%
  map(~ rnorm(10, .x))
```

```
## [[1]]
##  [1] 0.4318584 2.2339985 2.0797803 2.1416742 0.8599021 1.4803502 1.1686782
##  [8] 0.5791437 0.9945326 2.4618327
## 
## [[2]]
##  [1] 1.9993582 1.8140886 1.8983625 2.6912501 2.1727339 1.1397381 2.6211254
##  [8] 1.7293576 1.7228316 0.8387017
## 
## [[3]]
##  [1] 2.844852 4.423551 3.035988 2.884852 3.534187 3.276245 1.676339 4.923968
##  [9] 2.110770 2.774593
## 
## [[4]]
##  [1] 2.688604 3.988290 4.259162 3.272460 1.780160 2.152761 4.989109 3.847146
##  [9] 3.600067 4.622678
## 
## [[5]]
##  [1] 5.811755 4.345995 4.607296 3.982432 3.988317 4.977129 6.448051 5.059798
##  [9] 6.990571 5.790329
## 
## [[6]]
##  [1] 8.205088 4.223011 5.445546 5.696194 6.525025 3.447123 5.533415 5.879608
##  [9] 6.289842 5.726105
## 
## [[7]]
##  [1] 6.179094 8.639896 5.773333 6.809194 6.992966 6.422275 7.185122 4.389173
##  [9] 5.105871 6.047818
## 
## [[8]]
##  [1] 7.901826 7.428609 8.357170 8.413080 8.874393 6.759053 9.345543 8.335065
##  [9] 7.779112 7.638771
## 
## [[9]]
##  [1] 9.843409 7.628733 9.984434 9.465567 9.496891 9.675503 9.910865 8.918554
##  [9] 7.914487 8.901029
## 
## [[10]]
##  [1]  8.417983  9.905794 10.553245 10.411573  8.897385  8.332058 11.554748
##  [8] 11.098174 10.898944  8.815265
```

```r
# Simplify output to a vector instead of a list by computing the mean of the distributions
1:10 %>%
  map(rnorm, n = 10) %>%  # output a list
  map_dbl(mean)           # output an atomic vector
```

```
##  [1] 0.7318374 2.4697774 3.0166276 4.0095843 5.3207942 5.5948163 6.8471041
##  [8] 7.5384458 9.0545152 9.8331927
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
