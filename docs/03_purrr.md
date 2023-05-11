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
##  [1]  0.9568183  2.8650448  2.1981560  0.6089510  1.7053822  2.7599592
##  [7] -0.3607546  1.7759855  0.9550200  0.3174606
## 
## [[2]]
##  [1] 2.9032227 3.1294074 0.7215665 4.2226444 3.6253197 0.5059922 1.9556509
##  [8] 2.3894866 1.3962875 2.8812737
## 
## [[3]]
##  [1] 2.6215165 4.0190153 0.5779581 3.2260788 4.5997218 2.9682890 2.6249873
##  [8] 4.1123724 4.1227902 2.2409923
## 
## [[4]]
##  [1] 3.851299 3.994279 3.809529 3.868382 4.023621 4.455608 5.797307 3.939574
##  [9] 3.374947 4.063584
## 
## [[5]]
##  [1] 5.063482 4.335652 5.181203 4.245884 5.524943 5.036049 4.778888 6.607676
##  [9] 3.993926 6.329445
## 
## [[6]]
##  [1] 5.607152 8.214459 4.867169 5.473875 3.409760 5.257971 6.068497 5.468589
##  [9] 6.215634 4.716534
## 
## [[7]]
##  [1] 6.115351 6.158556 7.292740 7.597956 7.115716 6.030432 7.947264 7.489412
##  [9] 6.269428 7.422008
## 
## [[8]]
##  [1] 8.009514 6.864197 6.737559 8.251018 6.807435 9.607333 7.679211 7.573434
##  [9] 8.886922 8.435987
## 
## [[9]]
##  [1]  8.460095  9.831514 10.860349  9.077138  8.869532  7.159299  8.744495
##  [8]  8.397176  8.132245  7.870950
## 
## [[10]]
##  [1]  8.717056 10.859667 10.011716  9.905356 11.087304 11.418387  9.990313
##  [8] 10.474574  9.269086  9.775370
```

```r
# You can also use an anonymous function
1:10 %>%
  map(function(x) rnorm(10, x))
```

```
## [[1]]
##  [1]  1.11350197 -0.06022625 -1.37079226  2.62981165  1.39254641  0.21846169
##  [7]  0.84319189 -0.22633344  1.70246450  0.66166007
## 
## [[2]]
##  [1] 4.034264 1.672367 2.848997 1.446305 2.951599 2.465111 2.925400 1.413604
##  [9] 1.913360 2.443766
## 
## [[3]]
##  [1] 3.060641 1.616938 2.719261 2.628961 2.911265 2.554266 2.891745 3.417312
##  [9] 2.832603 3.201310
## 
## [[4]]
##  [1] 4.375858 1.801699 3.825798 5.083481 4.101677 5.554716 3.588879 3.732571
##  [9] 5.834065 3.327101
## 
## [[5]]
##  [1] 2.780941 4.891327 4.083682 6.717099 6.372229 4.037779 5.062755 6.111061
##  [9] 3.737440 6.420468
## 
## [[6]]
##  [1] 5.852848 6.311494 6.975048 4.105708 7.006056 7.232611 6.194221 5.953009
##  [9] 6.445343 5.417726
## 
## [[7]]
##  [1] 5.822074 6.906748 7.415821 7.715470 7.938982 7.617548 5.530839 7.069055
##  [9] 7.401223 7.661802
## 
## [[8]]
##  [1] 9.012350 8.780407 8.452637 8.774498 7.386004 5.690269 7.573500 8.074727
##  [9] 8.925675 6.218426
## 
## [[9]]
##  [1]  8.895530  7.573940  8.866533  9.492506  9.575101  8.606342 10.667647
##  [8]  9.474877  7.109665  8.111845
## 
## [[10]]
##  [1] 10.422390 10.075435 10.202715 10.381026  9.613512 10.388757 10.181952
##  [8]  9.176589  8.892918  8.338508
```

```r
# Or a formula
1:10 %>%
  map(~ rnorm(10, .x))
```

```
## [[1]]
##  [1]  1.4465027  2.0831450  0.2504933  0.8008118 -0.4229630  0.1836169
##  [7]  1.1335261  1.4128247  1.9174807  3.2592194
## 
## [[2]]
##  [1] 2.6581305 1.0796827 2.2973953 0.2123671 2.0140291 1.2321311 3.0060009
##  [8] 1.1230929 2.3737925 0.7013865
## 
## [[3]]
##  [1] 2.065874 2.291877 2.090610 4.549089 3.017146 3.143654 3.005866 3.312524
##  [9] 1.504188 3.621014
## 
## [[4]]
##  [1] 4.070333 3.365229 5.186513 6.412420 4.729520 4.558129 3.905438 5.298025
##  [9] 3.089909 4.242915
## 
## [[5]]
##  [1] 5.691716 4.762314 4.671307 4.541890 4.543598 5.504707 5.506233 4.004525
##  [9] 3.357210 6.113853
## 
## [[6]]
##  [1] 6.517114 7.186755 6.730101 6.861497 3.878155 5.343719 6.653394 8.227101
##  [9] 5.947019 5.501919
## 
## [[7]]
##  [1] 7.040736 8.962590 9.407680 8.149725 7.177082 8.247768 6.100168 8.440118
##  [9] 6.294496 7.563079
## 
## [[8]]
##  [1] 8.834375 7.246498 8.668342 8.664761 7.936759 7.248158 7.954025 8.546718
##  [9] 8.028588 6.864864
## 
## [[9]]
##  [1]  7.861197  8.984241  8.199782  8.797260 10.249191  9.618673  8.988000
##  [8]  8.637607  7.691781  8.233871
## 
## [[10]]
##  [1] 11.441084 10.701003 10.006795  8.124192  9.304147  8.395654 10.459165
##  [8] 10.067346  9.662966  9.511854
```

```r
# Simplify output to a vector instead of a list by computing the mean of the distributions
1:10 %>%
  map(rnorm, n = 10) %>%  # output a list
  map_dbl(mean)           # output an atomic vector
```

```
##  [1]  1.012407  2.057640  3.023085  3.785966  5.118181  5.593451  6.734778
##  [8]  8.387110  8.741657 10.456056
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
