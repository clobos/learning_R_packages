--- 
title: "Learning R packages"
author: "Cristian Villegas"
date: "2023-05-09"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  This is a minimal example of using the bookdown package to write a book.
  The HTML output format for this example is bookdown::gitbook,
  set in the _output.yml file.
link-citations: yes
github-repo: rstudio/bookdown-demo
---

# Intro

## Carrega pacotes a serem usados


```r
#install.packages("tidyverse") 
#install.packages("dplyr")     
#install.packages("tidyr")     
#install.packages("ggplot2")   

library(tidyverse)
# Manipulação de dados
#library(dplyr)

# Visualização de gráficos
library(ggplot2)
library(gridExtra)
library(patchwork)
library(plotly)
library(esquisse)

# Para dados gráfico de perfis
library(nlme)
```



Ver como citar referências @tidyverse2019, @R-tidyverse, @R-tidyr, @R-ggplot2, @R-purrr, @R-dplyr, @R-knitr, @R-bookdown


## Alguns atalhos no Rstudio

Para considerar

  > Operador Pipe (%>%): Ctrl + Shift + M (Windows) ou Cmd + Shift + M (Mac).

  > Criar novos chunks: Ctrl + Alt + I (Windows) ou Cmd + Option + I (Mac).
  



## Descrição dos dados `mpg` 

Dados de economia de combustível de 1999 a 2008 para *38 modelos populares de carros*. Este conjunto de dados contém um subconjunto dos dados de economia de combustível que a EPA disponibiliza em *https://fueleconomy.gov/*. Ele contém apenas modelos que tiveram um novo lançamento a cada ano entre 1999 e 2008 - isso foi usado como um substituto para a popularidade do carro. Um *data frame* com 234 linhas e 11 variáveis:

  - *manufacturer* nome do fabricante 

  - *model* nome do modelo

  - *displ* cilindrada do motor, em litros

  - *year* ano de fabricação

  - *cyl* número de cilindros

  - *trans* tipo de transmissão

  - *drv* o tipo de trem de força, onde **f = tração dianteira**, **r = tração traseira** e **4 = 4wd**

  - *cty* milhas urbanas  por galão

  - *hwy* milhas rodoviárias por galão

  - *fl* tipo de combustível

  - *class* "tipo" de carro
  

```r
#help("mpg")
dados <- mpg
glimpse(dados)
```

```
## Rows: 234
## Columns: 11
## $ manufacturer <chr> "audi", "audi", "audi", "audi", "audi", "audi", "audi", "~
## $ model        <chr> "a4", "a4", "a4", "a4", "a4", "a4", "a4", "a4 quattro", "~
## $ displ        <dbl> 1.8, 1.8, 2.0, 2.0, 2.8, 2.8, 3.1, 1.8, 1.8, 2.0, 2.0, 2.~
## $ year         <int> 1999, 1999, 2008, 2008, 1999, 1999, 2008, 1999, 1999, 200~
## $ cyl          <int> 4, 4, 4, 4, 6, 6, 6, 4, 4, 4, 4, 6, 6, 6, 6, 6, 6, 8, 8, ~
## $ trans        <chr> "auto(l5)", "manual(m5)", "manual(m6)", "auto(av)", "auto~
## $ drv          <chr> "f", "f", "f", "f", "f", "f", "f", "4", "4", "4", "4", "4~
## $ cty          <int> 18, 21, 20, 21, 16, 18, 18, 18, 16, 20, 19, 15, 17, 17, 1~
## $ hwy          <int> 29, 29, 31, 30, 26, 26, 27, 26, 25, 28, 27, 25, 25, 25, 2~
## $ fl           <chr> "p", "p", "p", "p", "p", "p", "p", "p", "p", "p", "p", "p~
## $ class        <chr> "compact", "compact", "compact", "compact", "compact", "c~
```

```r
dados <- mutate(.data = dados, 
                across(where(is.character), 
                as.factor))
#View(df) 
glimpse(dados)
```

```
## Rows: 234
## Columns: 11
## $ manufacturer <fct> audi, audi, audi, audi, audi, audi, audi, audi, audi, aud~
## $ model        <fct> a4, a4, a4, a4, a4, a4, a4, a4 quattro, a4 quattro, a4 qu~
## $ displ        <dbl> 1.8, 1.8, 2.0, 2.0, 2.8, 2.8, 3.1, 1.8, 1.8, 2.0, 2.0, 2.~
## $ year         <int> 1999, 1999, 2008, 2008, 1999, 1999, 2008, 1999, 1999, 200~
## $ cyl          <int> 4, 4, 4, 4, 6, 6, 6, 4, 4, 4, 4, 6, 6, 6, 6, 6, 6, 8, 8, ~
## $ trans        <fct> auto(l5), manual(m5), manual(m6), auto(av), auto(l5), man~
## $ drv          <fct> f, f, f, f, f, f, f, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, r, ~
## $ cty          <int> 18, 21, 20, 21, 16, 18, 18, 18, 16, 20, 19, 15, 17, 17, 1~
## $ hwy          <int> 29, 29, 31, 30, 26, 26, 27, 26, 25, 28, 27, 25, 25, 25, 2~
## $ fl           <fct> p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, r, ~
## $ class        <fct> compact, compact, compact, compact, compact, compact, com~
```

<!--chapter:end:index.Rmd-->

# dplyr (60 minutos)

## Carrega pacotes a serem usados


```r
#install.packages("tidyverse") 
#install.packages("dplyr")     
#install.packages("tidyr")     
#install.packages("ggplot2")   

library(tidyverse)
# Manipulação de dados
#library(dplyr)

# Visualização de gráficos
library(ggplot2)
library(gridExtra)
library(patchwork)
library(plotly)
library(esquisse)

# Para dados gráfico de perfis
library(nlme)
```

## Descrição dos dados `mpg` 

Dados de economia de combustível de 1999 a 2008 para *38 modelos populares de carros*. Este conjunto de dados contém um subconjunto dos dados de economia de combustível que a EPA disponibiliza em *https://fueleconomy.gov/*. Ele contém apenas modelos que tiveram um novo lançamento a cada ano entre 1999 e 2008 - isso foi usado como um substituto para a popularidade do carro. Um *data frame* com 234 linhas e 11 variáveis:

  - *manufacturer* nome do fabricante 

  - *model* nome do modelo

  - *displ* cilindrada do motor, em litros

  - *year* ano de fabricação

  - *cyl* número de cilindros

  - *trans* tipo de transmissão

  - *drv* o tipo de trem de força, onde **f = tração dianteira**, **r = tração traseira** e **4 = 4wd**

  - *cty* milhas urbanas  por galão

  - *hwy* milhas rodoviárias por galão

  - *fl* tipo de combustível

  - *class* "tipo" de carro
  

```r
#help("mpg")
library(tidyverse)
dados <- mpg
glimpse(dados)
```

```
## Rows: 234
## Columns: 11
## $ manufacturer <chr> "audi", "audi", "audi", "audi", "audi", "audi", "audi", "~
## $ model        <chr> "a4", "a4", "a4", "a4", "a4", "a4", "a4", "a4 quattro", "~
## $ displ        <dbl> 1.8, 1.8, 2.0, 2.0, 2.8, 2.8, 3.1, 1.8, 1.8, 2.0, 2.0, 2.~
## $ year         <int> 1999, 1999, 2008, 2008, 1999, 1999, 2008, 1999, 1999, 200~
## $ cyl          <int> 4, 4, 4, 4, 6, 6, 6, 4, 4, 4, 4, 6, 6, 6, 6, 6, 6, 8, 8, ~
## $ trans        <chr> "auto(l5)", "manual(m5)", "manual(m6)", "auto(av)", "auto~
## $ drv          <chr> "f", "f", "f", "f", "f", "f", "f", "4", "4", "4", "4", "4~
## $ cty          <int> 18, 21, 20, 21, 16, 18, 18, 18, 16, 20, 19, 15, 17, 17, 1~
## $ hwy          <int> 29, 29, 31, 30, 26, 26, 27, 26, 25, 28, 27, 25, 25, 25, 2~
## $ fl           <chr> "p", "p", "p", "p", "p", "p", "p", "p", "p", "p", "p", "p~
## $ class        <chr> "compact", "compact", "compact", "compact", "compact", "c~
```

```r
dados <- mutate(.data = dados, 
                across(where(is.character), 
                as.factor))
#View(df) 
glimpse(dados)
```

```
## Rows: 234
## Columns: 11
## $ manufacturer <fct> audi, audi, audi, audi, audi, audi, audi, audi, audi, aud~
## $ model        <fct> a4, a4, a4, a4, a4, a4, a4, a4 quattro, a4 quattro, a4 qu~
## $ displ        <dbl> 1.8, 1.8, 2.0, 2.0, 2.8, 2.8, 3.1, 1.8, 1.8, 2.0, 2.0, 2.~
## $ year         <int> 1999, 1999, 2008, 2008, 1999, 1999, 2008, 1999, 1999, 200~
## $ cyl          <int> 4, 4, 4, 4, 6, 6, 6, 4, 4, 4, 4, 6, 6, 6, 6, 6, 6, 8, 8, ~
## $ trans        <fct> auto(l5), manual(m5), manual(m6), auto(av), auto(l5), man~
## $ drv          <fct> f, f, f, f, f, f, f, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, r, ~
## $ cty          <int> 18, 21, 20, 21, 16, 18, 18, 18, 16, 20, 19, 15, 17, 17, 1~
## $ hwy          <int> 29, 29, 31, 30, 26, 26, 27, 26, 25, 28, 27, 25, 25, 25, 2~
## $ fl           <fct> p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, p, r, ~
## $ class        <fct> compact, compact, compact, compact, compact, compact, com~
```


## Lista de funções do pacote dplyr


```r
ls("package:dplyr")
```

```
##   [1] "%>%"                   "across"                "add_count"            
##   [4] "add_count_"            "add_row"               "add_rownames"         
##   [7] "add_tally"             "add_tally_"            "all_equal"            
##  [10] "all_of"                "all_vars"              "anti_join"            
##  [13] "any_of"                "any_vars"              "arrange"              
##  [16] "arrange_"              "arrange_all"           "arrange_at"           
##  [19] "arrange_if"            "as.tbl"                "as_data_frame"        
##  [22] "as_label"              "as_tibble"             "auto_copy"            
##  [25] "band_instruments"      "band_instruments2"     "band_members"         
##  [28] "bench_tbls"            "between"               "bind_cols"            
##  [31] "bind_rows"             "c_across"              "case_match"           
##  [34] "case_when"             "changes"               "check_dbplyr"         
##  [37] "coalesce"              "collapse"              "collect"              
##  [40] "combine"               "common_by"             "compare_tbls"         
##  [43] "compare_tbls2"         "compute"               "consecutive_id"       
##  [46] "contains"              "copy_to"               "count"                
##  [49] "count_"                "cross_join"            "cumall"               
##  [52] "cumany"                "cume_dist"             "cummean"              
##  [55] "cur_column"            "cur_data"              "cur_data_all"         
##  [58] "cur_group"             "cur_group_id"          "cur_group_rows"       
##  [61] "current_vars"          "data_frame"            "db_analyze"           
##  [64] "db_begin"              "db_commit"             "db_create_index"      
##  [67] "db_create_indexes"     "db_create_table"       "db_data_type"         
##  [70] "db_desc"               "db_drop_table"         "db_explain"           
##  [73] "db_has_table"          "db_insert_into"        "db_list_tables"       
##  [76] "db_query_fields"       "db_query_rows"         "db_rollback"          
##  [79] "db_save_query"         "db_write_table"        "dense_rank"           
##  [82] "desc"                  "dim_desc"              "distinct"             
##  [85] "distinct_"             "distinct_all"          "distinct_at"          
##  [88] "distinct_if"           "distinct_prepare"      "do"                   
##  [91] "do_"                   "dplyr_col_modify"      "dplyr_reconstruct"    
##  [94] "dplyr_row_slice"       "ends_with"             "enexpr"               
##  [97] "enexprs"               "enquo"                 "enquos"               
## [100] "ensym"                 "ensyms"                "eval_tbls"            
## [103] "eval_tbls2"            "everything"            "explain"              
## [106] "expr"                  "failwith"              "filter"               
## [109] "filter_"               "filter_all"            "filter_at"            
## [112] "filter_if"             "first"                 "full_join"            
## [115] "funs"                  "funs_"                 "glimpse"              
## [118] "group_by"              "group_by_"             "group_by_all"         
## [121] "group_by_at"           "group_by_drop_default" "group_by_if"          
## [124] "group_by_prepare"      "group_cols"            "group_data"           
## [127] "group_indices"         "group_indices_"        "group_keys"           
## [130] "group_map"             "group_modify"          "group_nest"           
## [133] "group_rows"            "group_size"            "group_split"          
## [136] "group_trim"            "group_vars"            "group_walk"           
## [139] "grouped_df"            "groups"                "id"                   
## [142] "ident"                 "if_all"                "if_any"               
## [145] "if_else"               "inner_join"            "intersect"            
## [148] "is.grouped_df"         "is.src"                "is.tbl"               
## [151] "is_grouped_df"         "join_by"               "lag"                  
## [154] "last"                  "last_col"              "last_dplyr_warnings"  
## [157] "lead"                  "left_join"             "location"             
## [160] "lst"                   "make_tbl"              "matches"              
## [163] "min_rank"              "mutate"                "mutate_"              
## [166] "mutate_all"            "mutate_at"             "mutate_each"          
## [169] "mutate_each_"          "mutate_if"             "n"                    
## [172] "n_distinct"            "n_groups"              "na_if"                
## [175] "near"                  "nest_by"               "nest_join"            
## [178] "new_grouped_df"        "new_rowwise_df"        "nth"                  
## [181] "ntile"                 "num_range"             "one_of"               
## [184] "order_by"              "percent_rank"          "pick"                 
## [187] "progress_estimated"    "pull"                  "quo"                  
## [190] "quo_name"              "quos"                  "recode"               
## [193] "recode_factor"         "reframe"               "relocate"             
## [196] "rename"                "rename_"               "rename_all"           
## [199] "rename_at"             "rename_if"             "rename_vars"          
## [202] "rename_vars_"          "rename_with"           "right_join"           
## [205] "row_number"            "rows_append"           "rows_delete"          
## [208] "rows_insert"           "rows_patch"            "rows_update"          
## [211] "rows_upsert"           "rowwise"               "same_src"             
## [214] "sample_frac"           "sample_n"              "select"               
## [217] "select_"               "select_all"            "select_at"            
## [220] "select_if"             "select_var"            "select_vars"          
## [223] "select_vars_"          "semi_join"             "setdiff"              
## [226] "setequal"              "show_query"            "slice"                
## [229] "slice_"                "slice_head"            "slice_max"            
## [232] "slice_min"             "slice_sample"          "slice_tail"           
## [235] "sql"                   "sql_escape_ident"      "sql_escape_string"    
## [238] "sql_join"              "sql_select"            "sql_semi_join"        
## [241] "sql_set_op"            "sql_subquery"          "sql_translate_env"    
## [244] "src"                   "src_df"                "src_local"            
## [247] "src_mysql"             "src_postgres"          "src_sqlite"           
## [250] "src_tbls"              "starts_with"           "starwars"             
## [253] "storms"                "summarise"             "summarise_"           
## [256] "summarise_all"         "summarise_at"          "summarise_each"       
## [259] "summarise_each_"       "summarise_if"          "summarize"            
## [262] "summarize_"            "summarize_all"         "summarize_at"         
## [265] "summarize_each"        "summarize_each_"       "summarize_if"         
## [268] "sym"                   "symdiff"               "syms"                 
## [271] "tally"                 "tally_"                "tbl"                  
## [274] "tbl_df"                "tbl_nongroup_vars"     "tbl_ptype"            
## [277] "tbl_vars"              "tibble"                "top_frac"             
## [280] "top_n"                 "transmute"             "transmute_"           
## [283] "transmute_all"         "transmute_at"          "transmute_if"         
## [286] "tribble"               "type_sum"              "ungroup"              
## [289] "union"                 "union_all"             "validate_grouped_df"  
## [292] "validate_rowwise_df"   "vars"                  "where"                
## [295] "with_groups"           "with_order"            "wrap_dbplyr_obj"
```


## Operador Pipe


```r
sqrt(log(44))
```

```
## [1] 1.945299
```

```r
44 %>% log %>% sqrt
```

```
## [1] 1.945299
```

## select() para colunas


```r
select(dados, manufacturer, model, year)
```

```
## # A tibble: 234 x 3
##    manufacturer model       year
##    <fct>        <fct>      <int>
##  1 audi         a4          1999
##  2 audi         a4          1999
##  3 audi         a4          2008
##  4 audi         a4          2008
##  5 audi         a4          1999
##  6 audi         a4          1999
##  7 audi         a4          2008
##  8 audi         a4 quattro  1999
##  9 audi         a4 quattro  1999
## 10 audi         a4 quattro  2008
## # ... with 224 more rows
```

```r
select(dados, starts_with("m"))
```

```
## # A tibble: 234 x 2
##    manufacturer model     
##    <fct>        <fct>     
##  1 audi         a4        
##  2 audi         a4        
##  3 audi         a4        
##  4 audi         a4        
##  5 audi         a4        
##  6 audi         a4        
##  7 audi         a4        
##  8 audi         a4 quattro
##  9 audi         a4 quattro
## 10 audi         a4 quattro
## # ... with 224 more rows
```

```r
select(dados, contains("r"))
```

```
## # A tibble: 234 x 4
##    manufacturer  year trans      drv  
##    <fct>        <int> <fct>      <fct>
##  1 audi          1999 auto(l5)   f    
##  2 audi          1999 manual(m5) f    
##  3 audi          2008 manual(m6) f    
##  4 audi          2008 auto(av)   f    
##  5 audi          1999 auto(l5)   f    
##  6 audi          1999 manual(m5) f    
##  7 audi          2008 auto(av)   f    
##  8 audi          1999 manual(m5) 4    
##  9 audi          1999 auto(l5)   4    
## 10 audi          2008 manual(m6) 4    
## # ... with 224 more rows
```

```r
select(dados, ends_with("y"))
```

```
## # A tibble: 234 x 2
##      cty   hwy
##    <int> <int>
##  1    18    29
##  2    21    29
##  3    20    31
##  4    21    30
##  5    16    26
##  6    18    26
##  7    18    27
##  8    18    26
##  9    16    25
## 10    20    28
## # ... with 224 more rows
```

```r
select(dados, matches("[abc]"))
```

```
## # A tibble: 234 x 6
##    manufacturer  year   cyl trans        cty class  
##    <fct>        <int> <int> <fct>      <int> <fct>  
##  1 audi          1999     4 auto(l5)      18 compact
##  2 audi          1999     4 manual(m5)    21 compact
##  3 audi          2008     4 manual(m6)    20 compact
##  4 audi          2008     4 auto(av)      21 compact
##  5 audi          1999     6 auto(l5)      16 compact
##  6 audi          1999     6 manual(m5)    18 compact
##  7 audi          2008     6 auto(av)      18 compact
##  8 audi          1999     4 manual(m5)    18 compact
##  9 audi          1999     4 auto(l5)      16 compact
## 10 audi          2008     4 manual(m6)    20 compact
## # ... with 224 more rows
```

```r
select(dados, starts_with("m"), starts_with("c")) 
```

```
## # A tibble: 234 x 5
##    manufacturer model        cyl   cty class  
##    <fct>        <fct>      <int> <int> <fct>  
##  1 audi         a4             4    18 compact
##  2 audi         a4             4    21 compact
##  3 audi         a4             4    20 compact
##  4 audi         a4             4    21 compact
##  5 audi         a4             6    16 compact
##  6 audi         a4             6    18 compact
##  7 audi         a4             6    18 compact
##  8 audi         a4 quattro     4    18 compact
##  9 audi         a4 quattro     4    16 compact
## 10 audi         a4 quattro     4    20 compact
## # ... with 224 more rows
```

```r
select(dados, ends_with("l"), ends_with("s")) 
```

```
## # A tibble: 234 x 6
##    model      displ   cyl fl    trans      class  
##    <fct>      <dbl> <int> <fct> <fct>      <fct>  
##  1 a4           1.8     4 p     auto(l5)   compact
##  2 a4           1.8     4 p     manual(m5) compact
##  3 a4           2       4 p     manual(m6) compact
##  4 a4           2       4 p     auto(av)   compact
##  5 a4           2.8     6 p     auto(l5)   compact
##  6 a4           2.8     6 p     manual(m5) compact
##  7 a4           3.1     6 p     auto(av)   compact
##  8 a4 quattro   1.8     4 p     manual(m5) compact
##  9 a4 quattro   1.8     4 p     auto(l5)   compact
## 10 a4 quattro   2       4 p     manual(m6) compact
## # ... with 224 more rows
```

```r
select(dados, 1:3)      
```

```
## # A tibble: 234 x 3
##    manufacturer model      displ
##    <fct>        <fct>      <dbl>
##  1 audi         a4           1.8
##  2 audi         a4           1.8
##  3 audi         a4           2  
##  4 audi         a4           2  
##  5 audi         a4           2.8
##  6 audi         a4           2.8
##  7 audi         a4           3.1
##  8 audi         a4 quattro   1.8
##  9 audi         a4 quattro   1.8
## 10 audi         a4 quattro   2  
## # ... with 224 more rows
```

```r
select(dados, c(2,5,7)) 
```

```
## # A tibble: 234 x 3
##    model        cyl drv  
##    <fct>      <int> <fct>
##  1 a4             4 f    
##  2 a4             4 f    
##  3 a4             4 f    
##  4 a4             4 f    
##  5 a4             6 f    
##  6 a4             6 f    
##  7 a4             6 f    
##  8 a4 quattro     4 4    
##  9 a4 quattro     4 4    
## 10 a4 quattro     4 4    
## # ... with 224 more rows
```

```r
select(dados, manufacturer:cyl) 
```

```
## # A tibble: 234 x 5
##    manufacturer model      displ  year   cyl
##    <fct>        <fct>      <dbl> <int> <int>
##  1 audi         a4           1.8  1999     4
##  2 audi         a4           1.8  1999     4
##  3 audi         a4           2    2008     4
##  4 audi         a4           2    2008     4
##  5 audi         a4           2.8  1999     6
##  6 audi         a4           2.8  1999     6
##  7 audi         a4           3.1  2008     6
##  8 audi         a4 quattro   1.8  1999     4
##  9 audi         a4 quattro   1.8  1999     4
## 10 audi         a4 quattro   2    2008     4
## # ... with 224 more rows
```

```r
select(dados,-(manufacturer:cyl))
```

```
## # A tibble: 234 x 6
##    trans      drv     cty   hwy fl    class  
##    <fct>      <fct> <int> <int> <fct> <fct>  
##  1 auto(l5)   f        18    29 p     compact
##  2 manual(m5) f        21    29 p     compact
##  3 manual(m6) f        20    31 p     compact
##  4 auto(av)   f        21    30 p     compact
##  5 auto(l5)   f        16    26 p     compact
##  6 manual(m5) f        18    26 p     compact
##  7 auto(av)   f        18    27 p     compact
##  8 manual(m5) 4        18    26 p     compact
##  9 auto(l5)   4        16    25 p     compact
## 10 manual(m6) 4        20    28 p     compact
## # ... with 224 more rows
```


##   rename()


```r
dados1 <- rename(dados, 
              mnfc = manufacturer,
              mod = model)
dados1
```

```
## # A tibble: 234 x 11
##    mnfc  mod        displ  year   cyl trans      drv     cty   hwy fl    class  
##    <fct> <fct>      <dbl> <int> <int> <fct>      <fct> <int> <int> <fct> <fct>  
##  1 audi  a4           1.8  1999     4 auto(l5)   f        18    29 p     compact
##  2 audi  a4           1.8  1999     4 manual(m5) f        21    29 p     compact
##  3 audi  a4           2    2008     4 manual(m6) f        20    31 p     compact
##  4 audi  a4           2    2008     4 auto(av)   f        21    30 p     compact
##  5 audi  a4           2.8  1999     6 auto(l5)   f        16    26 p     compact
##  6 audi  a4           2.8  1999     6 manual(m5) f        18    26 p     compact
##  7 audi  a4           3.1  2008     6 auto(av)   f        18    27 p     compact
##  8 audi  a4 quattro   1.8  1999     4 manual(m5) 4        18    26 p     compact
##  9 audi  a4 quattro   1.8  1999     4 auto(l5)   4        16    25 p     compact
## 10 audi  a4 quattro   2    2008     4 manual(m6) 4        20    28 p     compact
## # ... with 224 more rows
```

```r
select(dados,
       mnfc = manufacturer,
       mod = model)
```

```
## # A tibble: 234 x 2
##    mnfc  mod       
##    <fct> <fct>     
##  1 audi  a4        
##  2 audi  a4        
##  3 audi  a4        
##  4 audi  a4        
##  5 audi  a4        
##  6 audi  a4        
##  7 audi  a4        
##  8 audi  a4 quattro
##  9 audi  a4 quattro
## 10 audi  a4 quattro
## # ... with 224 more rows
```

```r
select(dados,
       mnfc = manufacturer,
       mod = model,
       everything())
```

```
## # A tibble: 234 x 11
##    mnfc  mod        displ  year   cyl trans      drv     cty   hwy fl    class  
##    <fct> <fct>      <dbl> <int> <int> <fct>      <fct> <int> <int> <fct> <fct>  
##  1 audi  a4           1.8  1999     4 auto(l5)   f        18    29 p     compact
##  2 audi  a4           1.8  1999     4 manual(m5) f        21    29 p     compact
##  3 audi  a4           2    2008     4 manual(m6) f        20    31 p     compact
##  4 audi  a4           2    2008     4 auto(av)   f        21    30 p     compact
##  5 audi  a4           2.8  1999     6 auto(l5)   f        16    26 p     compact
##  6 audi  a4           2.8  1999     6 manual(m5) f        18    26 p     compact
##  7 audi  a4           3.1  2008     6 auto(av)   f        18    27 p     compact
##  8 audi  a4 quattro   1.8  1999     4 manual(m5) 4        18    26 p     compact
##  9 audi  a4 quattro   1.8  1999     4 auto(l5)   4        16    25 p     compact
## 10 audi  a4 quattro   2    2008     4 manual(m6) 4        20    28 p     compact
## # ... with 224 more rows
```

## mutate() para colunas


```r
mutate(dados, sqrt_cty = sqrt(cty))
```

```
## # A tibble: 234 x 12
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  2 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  3 audi      a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
##  4 audi      a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
##  5 audi      a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
##  6 audi      a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
##  7 audi      a4      3.1  2008     6 auto~ f        18    27 p     comp~    4.24
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 audi      a4 q~   2    2008     4 manu~ 4        20    28 p     comp~    4.47
## # ... with 224 more rows, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

```r
names(dados)
```

```
##  [1] "manufacturer" "model"        "displ"        "year"         "cyl"         
##  [6] "trans"        "drv"          "cty"          "hwy"          "fl"          
## [11] "class"
```

```r
dados<- mutate(dados, sqrt_cty = sqrt(cty))
names(dados)
```

```
##  [1] "manufacturer" "model"        "displ"        "year"         "cyl"         
##  [6] "trans"        "drv"          "cty"          "hwy"          "fl"          
## [11] "class"        "sqrt_cty"
```

```r
dados <- mutate(dados,
`soma de variáveis` = (cty + hwy) / 2)
names(dados)
```

```
##  [1] "manufacturer"      "model"             "displ"            
##  [4] "year"              "cyl"               "trans"            
##  [7] "drv"               "cty"               "hwy"              
## [10] "fl"                "class"             "sqrt_cty"         
## [13] "soma de variáveis"
```

```r
dados <- mutate(dados,
             car = paste(manufacturer, model, sep = " "),
             `cyl / trans` = paste(cyl, " cylinders", " / ", trans, " transmission", sep = ""))
dados
```

```
## # A tibble: 234 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  2 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  3 audi      a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
##  4 audi      a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
##  5 audi      a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
##  6 audi      a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
##  7 audi      a4      3.1  2008     6 auto~ f        18    27 p     comp~    4.24
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 audi      a4 q~   2    2008     4 manu~ 4        20    28 p     comp~    4.47
## # ... with 224 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

## transmute() 


```r
transmute(dados,
          `avg miles per gallon` = (cty + hwy) / 2)
```

```
## # A tibble: 234 x 1
##    `avg miles per gallon`
##                     <dbl>
##  1                   23.5
##  2                   25  
##  3                   25.5
##  4                   25.5
##  5                   21  
##  6                   22  
##  7                   22.5
##  8                   22  
##  9                   20.5
## 10                   24  
## # ... with 224 more rows
```

```r
transmute(dados,
          car = paste(manufacturer, model, sep = " "),
          `cyl / trans` = paste(cyl, " cylinders", " / ", trans, " transmission", sep = ""))
```

```
## # A tibble: 234 x 2
##    car             `cyl / trans`                        
##    <chr>           <chr>                                
##  1 audi a4         4 cylinders / auto(l5) transmission  
##  2 audi a4         4 cylinders / manual(m5) transmission
##  3 audi a4         4 cylinders / manual(m6) transmission
##  4 audi a4         4 cylinders / auto(av) transmission  
##  5 audi a4         6 cylinders / auto(l5) transmission  
##  6 audi a4         6 cylinders / manual(m5) transmission
##  7 audi a4         6 cylinders / auto(av) transmission  
##  8 audi a4 quattro 4 cylinders / manual(m5) transmission
##  9 audi a4 quattro 4 cylinders / auto(l5) transmission  
## 10 audi a4 quattro 4 cylinders / manual(m6) transmission
## # ... with 224 more rows
```


## filter()  para linhas


```r
filter(dados, manufacturer == "audi") 
```

```
## # A tibble: 18 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  2 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  3 audi      a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
##  4 audi      a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
##  5 audi      a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
##  6 audi      a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
##  7 audi      a4      3.1  2008     6 auto~ f        18    27 p     comp~    4.24
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 audi      a4 q~   2    2008     4 manu~ 4        20    28 p     comp~    4.47
## 11 audi      a4 q~   2    2008     4 auto~ 4        19    27 p     comp~    4.36
## 12 audi      a4 q~   2.8  1999     6 auto~ 4        15    25 p     comp~    3.87
## 13 audi      a4 q~   2.8  1999     6 manu~ 4        17    25 p     comp~    4.12
## 14 audi      a4 q~   3.1  2008     6 auto~ 4        17    25 p     comp~    4.12
## 15 audi      a4 q~   3.1  2008     6 manu~ 4        15    25 p     comp~    3.87
## 16 audi      a6 q~   2.8  1999     6 auto~ 4        15    24 p     mids~    3.87
## 17 audi      a6 q~   3.1  2008     6 auto~ 4        17    25 p     mids~    4.12
## 18 audi      a6 q~   4.2  2008     8 auto~ 4        16    23 p     mids~    4   
## # ... with 3 more variables: `soma de variáveis` <dbl>, car <chr>,
## #   `cyl / trans` <chr>, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

```r
filter(dados, manufacturer == "audi" & year == "1999") 
```

```
## # A tibble: 9 x 15
##   manufact~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##   <fct>      <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
## 1 audi       a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
## 2 audi       a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
## 3 audi       a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
## 4 audi       a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
## 5 audi       a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
## 6 audi       a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 7 audi       a4 q~   2.8  1999     6 auto~ 4        15    25 p     comp~    3.87
## 8 audi       a4 q~   2.8  1999     6 manu~ 4        17    25 p     comp~    4.12
## 9 audi       a6 q~   2.8  1999     6 auto~ 4        15    24 p     mids~    3.87
## # ... with 3 more variables: `soma de variáveis` <dbl>, car <chr>,
## #   `cyl / trans` <chr>, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

```r
filter(dados, manufacturer == "audi", year == 1999) 
```

```
## # A tibble: 9 x 15
##   manufact~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##   <fct>      <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
## 1 audi       a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
## 2 audi       a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
## 3 audi       a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
## 4 audi       a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
## 5 audi       a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
## 6 audi       a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 7 audi       a4 q~   2.8  1999     6 auto~ 4        15    25 p     comp~    3.87
## 8 audi       a4 q~   2.8  1999     6 manu~ 4        17    25 p     comp~    4.12
## 9 audi       a6 q~   2.8  1999     6 auto~ 4        15    24 p     mids~    3.87
## # ... with 3 more variables: `soma de variáveis` <dbl>, car <chr>,
## #   `cyl / trans` <chr>, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

```r
filter(dados, manufacturer == "audi" | manufacturer == "dodge") %>%
  print(n = 20)
```

```
## # A tibble: 55 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  2 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  3 audi      a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
##  4 audi      a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
##  5 audi      a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
##  6 audi      a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
##  7 audi      a4      3.1  2008     6 auto~ f        18    27 p     comp~    4.24
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 audi      a4 q~   2    2008     4 manu~ 4        20    28 p     comp~    4.47
## 11 audi      a4 q~   2    2008     4 auto~ 4        19    27 p     comp~    4.36
## 12 audi      a4 q~   2.8  1999     6 auto~ 4        15    25 p     comp~    3.87
## 13 audi      a4 q~   2.8  1999     6 manu~ 4        17    25 p     comp~    4.12
## 14 audi      a4 q~   3.1  2008     6 auto~ 4        17    25 p     comp~    4.12
## 15 audi      a4 q~   3.1  2008     6 manu~ 4        15    25 p     comp~    3.87
## 16 audi      a6 q~   2.8  1999     6 auto~ 4        15    24 p     mids~    3.87
## 17 audi      a6 q~   3.1  2008     6 auto~ 4        17    25 p     mids~    4.12
## 18 audi      a6 q~   4.2  2008     8 auto~ 4        16    23 p     mids~    4   
## 19 dodge     cara~   2.4  1999     4 auto~ f        18    24 r     mini~    4.24
## 20 dodge     cara~   3    1999     6 auto~ f        17    24 r     mini~    4.12
## # ... with 35 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

```r
filter(dados, manufacturer %in% c("audi", "dodge")) %>%
  print(n = 20)
```

```
## # A tibble: 55 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  2 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  3 audi      a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
##  4 audi      a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
##  5 audi      a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
##  6 audi      a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
##  7 audi      a4      3.1  2008     6 auto~ f        18    27 p     comp~    4.24
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 audi      a4 q~   2    2008     4 manu~ 4        20    28 p     comp~    4.47
## 11 audi      a4 q~   2    2008     4 auto~ 4        19    27 p     comp~    4.36
## 12 audi      a4 q~   2.8  1999     6 auto~ 4        15    25 p     comp~    3.87
## 13 audi      a4 q~   2.8  1999     6 manu~ 4        17    25 p     comp~    4.12
## 14 audi      a4 q~   3.1  2008     6 auto~ 4        17    25 p     comp~    4.12
## 15 audi      a4 q~   3.1  2008     6 manu~ 4        15    25 p     comp~    3.87
## 16 audi      a6 q~   2.8  1999     6 auto~ 4        15    24 p     mids~    3.87
## 17 audi      a6 q~   3.1  2008     6 auto~ 4        17    25 p     mids~    4.12
## 18 audi      a6 q~   4.2  2008     8 auto~ 4        16    23 p     mids~    4   
## 19 dodge     cara~   2.4  1999     4 auto~ f        18    24 r     mini~    4.24
## 20 dodge     cara~   3    1999     6 auto~ f        17    24 r     mini~    4.12
## # ... with 35 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

```r
filter(dados, hwy >= 30) %>% 
  select(hwy) %>%
  print(n = 26)
```

```
## # A tibble: 26 x 1
##      hwy
##    <int>
##  1    31
##  2    30
##  3    30
##  4    33
##  5    32
##  6    32
##  7    32
##  8    34
##  9    36
## 10    36
## 11    30
## 12    31
## 13    31
## 14    32
## 15    31
## 16    31
## 17    31
## 18    31
## 19    30
## 20    33
## 21    35
## 22    37
## 23    35
## 24    44
## 25    44
## 26    41
```

```r
filter(dados, year != 1999) %>% 
  select(year) %>%
  print(n = 30)
```

```
## # A tibble: 117 x 1
##     year
##    <int>
##  1  2008
##  2  2008
##  3  2008
##  4  2008
##  5  2008
##  6  2008
##  7  2008
##  8  2008
##  9  2008
## 10  2008
## 11  2008
## 12  2008
## 13  2008
## 14  2008
## 15  2008
## 16  2008
## 17  2008
## 18  2008
## 19  2008
## 20  2008
## 21  2008
## 22  2008
## 23  2008
## 24  2008
## 25  2008
## 26  2008
## 27  2008
## 28  2008
## 29  2008
## 30  2008
## # ... with 87 more rows
```

```r
filter(dados, between(cty,15, 22)) 
```

```
## # A tibble: 143 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  2 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  3 audi      a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
##  4 audi      a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
##  5 audi      a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
##  6 audi      a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
##  7 audi      a4      3.1  2008     6 auto~ f        18    27 p     comp~    4.24
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 audi      a4 q~   2    2008     4 manu~ 4        20    28 p     comp~    4.47
## # ... with 133 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```


## slice() para linhas


```r
slice(dados, 1:5)
```

```
## # A tibble: 5 x 15
##   manufact~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##   <fct>      <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
## 1 audi       a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
## 2 audi       a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
## 3 audi       a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
## 4 audi       a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
## 5 audi       a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
## # ... with 3 more variables: `soma de variáveis` <dbl>, car <chr>,
## #   `cyl / trans` <chr>, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

```r
# dados[1:5,]

slice(dados, 20:30)
```

```
## # A tibble: 11 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 chevrolet c150~   5.3  2008     8 auto~ r        11    15 e     suv      3.32
##  2 chevrolet c150~   5.3  2008     8 auto~ r        14    20 r     suv      3.74
##  3 chevrolet c150~   5.7  1999     8 auto~ r        13    17 r     suv      3.61
##  4 chevrolet c150~   6    2008     8 auto~ r        12    17 r     suv      3.46
##  5 chevrolet corv~   5.7  1999     8 manu~ r        16    26 p     2sea~    4   
##  6 chevrolet corv~   5.7  1999     8 auto~ r        15    23 p     2sea~    3.87
##  7 chevrolet corv~   6.2  2008     8 manu~ r        16    26 p     2sea~    4   
##  8 chevrolet corv~   6.2  2008     8 auto~ r        15    25 p     2sea~    3.87
##  9 chevrolet corv~   7    2008     8 manu~ r        15    24 p     2sea~    3.87
## 10 chevrolet k150~   5.3  2008     8 auto~ 4        14    19 r     suv      3.74
## 11 chevrolet k150~   5.3  2008     8 auto~ 4        11    14 e     suv      3.32
## # ... with 3 more variables: `soma de variáveis` <dbl>, car <chr>,
## #   `cyl / trans` <chr>, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

```r
# dados[20:30,]
```


## arrange() para linhas


```r
# ordenar "displ" de menor a maior
arrange(dados, displ)
```

```
## # A tibble: 234 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 honda     civic   1.6  1999     4 manu~ f        28    33 r     subc~    5.29
##  2 honda     civic   1.6  1999     4 auto~ f        24    32 r     subc~    4.90
##  3 honda     civic   1.6  1999     4 manu~ f        25    32 r     subc~    5   
##  4 honda     civic   1.6  1999     4 manu~ f        23    29 p     subc~    4.80
##  5 honda     civic   1.6  1999     4 auto~ f        24    32 r     subc~    4.90
##  6 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  7 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 honda     civic   1.8  2008     4 manu~ f        26    34 r     subc~    5.10
## # ... with 224 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

```r
arrange(dados, displ) %>% 
  print(n=20)
```

```
## # A tibble: 234 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 honda     civic   1.6  1999     4 manu~ f        28    33 r     subc~    5.29
##  2 honda     civic   1.6  1999     4 auto~ f        24    32 r     subc~    4.90
##  3 honda     civic   1.6  1999     4 manu~ f        25    32 r     subc~    5   
##  4 honda     civic   1.6  1999     4 manu~ f        23    29 p     subc~    4.80
##  5 honda     civic   1.6  1999     4 auto~ f        24    32 r     subc~    4.90
##  6 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  7 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 honda     civic   1.8  2008     4 manu~ f        26    34 r     subc~    5.10
## 11 honda     civic   1.8  2008     4 auto~ f        25    36 r     subc~    5   
## 12 honda     civic   1.8  2008     4 auto~ f        24    36 c     subc~    4.90
## 13 toyota    coro~   1.8  1999     4 auto~ f        24    30 r     comp~    4.90
## 14 toyota    coro~   1.8  1999     4 auto~ f        24    33 r     comp~    4.90
## 15 toyota    coro~   1.8  1999     4 manu~ f        26    35 r     comp~    5.10
## 16 toyota    coro~   1.8  2008     4 manu~ f        28    37 r     comp~    5.29
## 17 toyota    coro~   1.8  2008     4 auto~ f        26    35 r     comp~    5.10
## 18 volkswag~ pass~   1.8  1999     4 manu~ f        21    29 p     mids~    4.58
## 19 volkswag~ pass~   1.8  1999     4 auto~ f        18    29 p     mids~    4.24
## 20 volkswag~ jetta   1.9  1999     4 manu~ f        33    44 d     comp~    5.74
## # ... with 214 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

```r
# ordenar "displ" de maior a  menor
arrange(dados, desc(displ))
```

```
## # A tibble: 234 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 chevrolet corv~   7    2008     8 manu~ r        15    24 p     2sea~    3.87
##  2 chevrolet k150~   6.5  1999     8 auto~ 4        14    17 d     suv      3.74
##  3 chevrolet corv~   6.2  2008     8 manu~ r        16    26 p     2sea~    4   
##  4 chevrolet corv~   6.2  2008     8 auto~ r        15    25 p     2sea~    3.87
##  5 jeep      gran~   6.1  2008     8 auto~ 4        11    14 p     suv      3.32
##  6 chevrolet c150~   6    2008     8 auto~ r        12    17 r     suv      3.46
##  7 dodge     dura~   5.9  1999     8 auto~ 4        11    15 r     suv      3.32
##  8 dodge     ram ~   5.9  1999     8 auto~ 4        11    15 r     pick~    3.32
##  9 chevrolet c150~   5.7  1999     8 auto~ r        13    17 r     suv      3.61
## 10 chevrolet corv~   5.7  1999     8 manu~ r        16    26 p     2sea~    4   
## # ... with 224 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

```r
arrange(dados, desc(displ)) %>% 
  print(n=20)
```

```
## # A tibble: 234 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 chevrolet corv~   7    2008     8 manu~ r        15    24 p     2sea~    3.87
##  2 chevrolet k150~   6.5  1999     8 auto~ 4        14    17 d     suv      3.74
##  3 chevrolet corv~   6.2  2008     8 manu~ r        16    26 p     2sea~    4   
##  4 chevrolet corv~   6.2  2008     8 auto~ r        15    25 p     2sea~    3.87
##  5 jeep      gran~   6.1  2008     8 auto~ 4        11    14 p     suv      3.32
##  6 chevrolet c150~   6    2008     8 auto~ r        12    17 r     suv      3.46
##  7 dodge     dura~   5.9  1999     8 auto~ 4        11    15 r     suv      3.32
##  8 dodge     ram ~   5.9  1999     8 auto~ 4        11    15 r     pick~    3.32
##  9 chevrolet c150~   5.7  1999     8 auto~ r        13    17 r     suv      3.61
## 10 chevrolet corv~   5.7  1999     8 manu~ r        16    26 p     2sea~    4   
## 11 chevrolet corv~   5.7  1999     8 auto~ r        15    23 p     2sea~    3.87
## 12 chevrolet k150~   5.7  1999     8 auto~ 4        11    15 r     suv      3.32
## 13 dodge     dura~   5.7  2008     8 auto~ 4        13    18 r     suv      3.61
## 14 dodge     ram ~   5.7  2008     8 auto~ 4        13    17 r     pick~    3.61
## 15 jeep      gran~   5.7  2008     8 auto~ 4        13    18 r     suv      3.61
## 16 toyota    land~   5.7  2008     8 auto~ 4        13    18 r     suv      3.61
## 17 nissan    path~   5.6  2008     8 auto~ 4        12    18 p     suv      3.46
## 18 ford      expe~   5.4  1999     8 auto~ r        11    17 r     suv      3.32
## 19 ford      expe~   5.4  2008     8 auto~ r        12    18 r     suv      3.46
## 20 ford      f150~   5.4  1999     8 auto~ 4        11    15 r     pick~    3.32
## # ... with 214 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

```r
select(dados, displ, cty) %>% 
  arrange(displ, cty) %>% 
  print(n = 20)
```

```
## # A tibble: 234 x 2
##    displ   cty
##    <dbl> <int>
##  1   1.6    23
##  2   1.6    24
##  3   1.6    24
##  4   1.6    25
##  5   1.6    28
##  6   1.8    16
##  7   1.8    18
##  8   1.8    18
##  9   1.8    18
## 10   1.8    21
## 11   1.8    21
## 12   1.8    24
## 13   1.8    24
## 14   1.8    24
## 15   1.8    25
## 16   1.8    26
## 17   1.8    26
## 18   1.8    26
## 19   1.8    28
## 20   1.9    29
## # ... with 214 more rows
```

```r
select(dados, displ, cty) %>% 
  arrange(displ, desc(cty)) %>% 
  print(n = 20)
```

```
## # A tibble: 234 x 2
##    displ   cty
##    <dbl> <int>
##  1   1.6    28
##  2   1.6    25
##  3   1.6    24
##  4   1.6    24
##  5   1.6    23
##  6   1.8    28
##  7   1.8    26
##  8   1.8    26
##  9   1.8    26
## 10   1.8    25
## 11   1.8    24
## 12   1.8    24
## 13   1.8    24
## 14   1.8    21
## 15   1.8    21
## 16   1.8    18
## 17   1.8    18
## 18   1.8    18
## 19   1.8    16
## 20   1.9    35
## # ... with 214 more rows
```


## distinct() para linhas


```r
dados_exemplo <- data.frame(id = 1:3,
                         name = c("John", "Max", "Julia"))
dados_exemplo
```

```
##   id  name
## 1  1  John
## 2  2   Max
## 3  3 Julia
```

```r
# bind_rows == rbind()
dados_exemplo<- bind_rows(dados_exemplo, slice(dados_exemplo, 2)) 
dados_exemplo
```

```
##   id  name
## 1  1  John
## 2  2   Max
## 3  3 Julia
## 4  2   Max
```

```r
distinct(dados_exemplo)
```

```
##   id  name
## 1  1  John
## 2  2   Max
## 3  3 Julia
```

```r
dados_exemplo2 <- data.frame(id = c(1,1,2),
                         name = c("John", "Max", "Julia"))
dados_exemplo2
```

```
##   id  name
## 1  1  John
## 2  1   Max
## 3  2 Julia
```

```r
distinct(dados_exemplo2)
```

```
##   id  name
## 1  1  John
## 2  1   Max
## 3  2 Julia
```

```r
dados_duplicados <- select(dados, manufacturer, model)
dados_duplicados
```

```
## # A tibble: 234 x 2
##    manufacturer model     
##    <fct>        <fct>     
##  1 audi         a4        
##  2 audi         a4        
##  3 audi         a4        
##  4 audi         a4        
##  5 audi         a4        
##  6 audi         a4        
##  7 audi         a4        
##  8 audi         a4 quattro
##  9 audi         a4 quattro
## 10 audi         a4 quattro
## # ... with 224 more rows
```

```r
dados_nao_duplicados <- distinct(dados_duplicados)
dados_nao_duplicados
```

```
## # A tibble: 38 x 2
##    manufacturer model             
##    <fct>        <fct>             
##  1 audi         a4                
##  2 audi         a4 quattro        
##  3 audi         a6 quattro        
##  4 chevrolet    c1500 suburban 2wd
##  5 chevrolet    corvette          
##  6 chevrolet    k1500 tahoe 4wd   
##  7 chevrolet    malibu            
##  8 dodge        caravan 2wd       
##  9 dodge        dakota pickup 4wd 
## 10 dodge        durango 4wd       
## # ... with 28 more rows
```

## summarise()


```r
summarise(dados, `média hwy` = mean(hwy))
```

```
## # A tibble: 1 x 1
##   `média hwy`
##         <dbl>
## 1        23.4
```

```r
summarise(dados, 
          `num. de dados` = n(),
          `num. modelos` = n_distinct(model))
```

```
## # A tibble: 1 x 2
##   `num. de dados` `num. modelos`
##             <int>          <int>
## 1             234             38
```

```r
# levels(dados$model)
summarise(dados, 
          `mín. hwy` = min(hwy, na.rm = TRUE),
          `mín. cty` = min(cty, na.rm = TRUE),
          `máx. hwy` = max(hwy, na.rm = TRUE),
          `máx. cty` = max(cty, na.rm = TRUE))
```

```
## # A tibble: 1 x 4
##   `mín. hwy` `mín. cty` `máx. hwy` `máx. cty`
##        <int>      <int>      <int>      <int>
## 1         12          9         44         35
```

```r
dados %>%
  summarise_at(c("hwy", "cty"), list(min, max), na.rm = TRUE)
```

```
## # A tibble: 1 x 4
##   hwy_fn1 cty_fn1 hwy_fn2 cty_fn2
##     <int>   <int>   <int>   <int>
## 1      12       9      44      35
```

```r
dados %>%
  summarise_if(is.numeric, list(min, max), na.rm = TRUE) 
```

```
## # A tibble: 1 x 14
##   displ_fn1 year_fn1 cyl_fn1 cty_fn1 hwy_fn1 sqrt_cty_~1 soma ~2 displ~3 year_~4
##       <dbl>    <int>   <int>   <int>   <int>       <dbl>   <dbl>   <dbl>   <int>
## 1       1.6     1999       4       9      12           3    10.5       7    2008
## # ... with 5 more variables: cyl_fn2 <int>, cty_fn2 <int>, hwy_fn2 <int>,
## #   sqrt_cty_fn2 <dbl>, `soma de variáveis_fn2` <dbl>, and abbreviated variable
## #   names 1: sqrt_cty_fn1, 2: `soma de variáveis_fn1`, 3: displ_fn2,
## #   4: year_fn2
```

```r
dados %>%
  summarise_if(is.numeric, min, na.rm = TRUE) 
```

```
## # A tibble: 1 x 7
##   displ  year   cyl   cty   hwy sqrt_cty `soma de variáveis`
##   <dbl> <int> <int> <int> <int>    <dbl>               <dbl>
## 1   1.6  1999     4     9    12        3                10.5
```

```r
dados %>%
  summarise_if(is.numeric, max, na.rm = TRUE) 
```

```
## # A tibble: 1 x 7
##   displ  year   cyl   cty   hwy sqrt_cty `soma de variáveis`
##   <dbl> <int> <int> <int> <int>    <dbl>               <dbl>
## 1     7  2008     8    35    44     5.92                39.5
```

```r
Tiago<- function(dados){
  sd(dados)/mean(dados)
}

dados %>%
  summarise_if(is.numeric, Tiago) 
```

```
## # A tibble: 1 x 7
##   displ    year   cyl   cty   hwy sqrt_cty `soma de variáveis`
##   <dbl>   <dbl> <dbl> <dbl> <dbl>    <dbl>               <dbl>
## 1 0.372 0.00225 0.274 0.252 0.254    0.125               0.251
```

## group_by() 


```r
group_by(dados, manufacturer)
```

```
## # A tibble: 234 x 15
## # Groups:   manufacturer [15]
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 audi      a4      1.8  1999     4 auto~ f        18    29 p     comp~    4.24
##  2 audi      a4      1.8  1999     4 manu~ f        21    29 p     comp~    4.58
##  3 audi      a4      2    2008     4 manu~ f        20    31 p     comp~    4.47
##  4 audi      a4      2    2008     4 auto~ f        21    30 p     comp~    4.58
##  5 audi      a4      2.8  1999     6 auto~ f        16    26 p     comp~    4   
##  6 audi      a4      2.8  1999     6 manu~ f        18    26 p     comp~    4.24
##  7 audi      a4      3.1  2008     6 auto~ f        18    27 p     comp~    4.24
##  8 audi      a4 q~   1.8  1999     4 manu~ 4        18    26 p     comp~    4.24
##  9 audi      a4 q~   1.8  1999     4 auto~ 4        16    25 p     comp~    4   
## 10 audi      a4 q~   2    2008     4 manu~ 4        20    28 p     comp~    4.47
## # ... with 224 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

```r
dados %>% 
  group_by(manufacturer) %>%
  summarise(`num. carros` = n())
```

```
## # A tibble: 15 x 2
##    manufacturer `num. carros`
##    <fct>                <int>
##  1 audi                    18
##  2 chevrolet               19
##  3 dodge                   37
##  4 ford                    25
##  5 honda                    9
##  6 hyundai                 14
##  7 jeep                     8
##  8 land rover               4
##  9 lincoln                  3
## 10 mercury                  4
## 11 nissan                  13
## 12 pontiac                  5
## 13 subaru                  14
## 14 toyota                  34
## 15 volkswagen              27
```

```r
dados %>% 
  group_by(model) %>%
  summarise(`média hwy` = mean(hwy),
          `min. hwy` = min(hwy),
          `max. hwy` = max(hwy))
```

```
## # A tibble: 38 x 4
##    model              `média hwy` `min. hwy` `max. hwy`
##    <fct>                    <dbl>      <int>      <int>
##  1 4runner 4wd               18.8         17         20
##  2 a4                        28.3         26         31
##  3 a4 quattro                25.8         25         28
##  4 a6 quattro                24           23         25
##  5 altima                    28.7         26         32
##  6 c1500 suburban 2wd        17.8         15         20
##  7 camry                     28.3         26         31
##  8 camry solara              28.1         26         31
##  9 caravan 2wd               22.4         17         24
## 10 civic                     32.6         29         36
## # ... with 28 more rows
```

## count()


```r
count(dados)
```

```
## # A tibble: 1 x 1
##       n
##   <int>
## 1   234
```

```r
dados %>% 
  group_by(manufacturer) %>%
  count()
```

```
## # A tibble: 15 x 2
## # Groups:   manufacturer [15]
##    manufacturer     n
##    <fct>        <int>
##  1 audi            18
##  2 chevrolet       19
##  3 dodge           37
##  4 ford            25
##  5 honda            9
##  6 hyundai         14
##  7 jeep             8
##  8 land rover       4
##  9 lincoln          3
## 10 mercury          4
## 11 nissan          13
## 12 pontiac          5
## 13 subaru          14
## 14 toyota          34
## 15 volkswagen      27
```

```r
# Equivalente com o código anterior
dados %>% 
  group_by(manufacturer) %>%
  summarise(cars = n())
```

```
## # A tibble: 15 x 2
##    manufacturer  cars
##    <fct>        <int>
##  1 audi            18
##  2 chevrolet       19
##  3 dodge           37
##  4 ford            25
##  5 honda            9
##  6 hyundai         14
##  7 jeep             8
##  8 land rover       4
##  9 lincoln          3
## 10 mercury          4
## 11 nissan          13
## 12 pontiac          5
## 13 subaru          14
## 14 toyota          34
## 15 volkswagen      27
```

## sample_n() 


```r
set.seed(567)
sample_n(dados, size = 10, replace = F)
```

```
## # A tibble: 10 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 mercury   moun~   5    1999     8 auto~ 4        13    17 r     suv      3.61
##  2 chevrolet corv~   7    2008     8 manu~ r        15    24 p     2sea~    3.87
##  3 dodge     ram ~   4.7  2008     8 manu~ 4        12    16 r     pick~    3.46
##  4 toyota    land~   4.7  1999     8 auto~ 4        11    15 r     suv      3.32
##  5 volkswag~ jetta   2    1999     4 auto~ f        19    26 r     comp~    4.36
##  6 dodge     cara~   3.8  1999     6 auto~ f        15    21 r     mini~    3.87
##  7 honda     civic   1.8  2008     4 auto~ f        25    36 r     subc~    5   
##  8 ford      must~   4.6  1999     8 auto~ r        15    21 r     subc~    3.87
##  9 chevrolet c150~   5.3  2008     8 auto~ r        14    20 r     suv      3.74
## 10 ford      expe~   5.4  1999     8 auto~ r        11    17 r     suv      3.32
## # ... with 3 more variables: `soma de variáveis` <dbl>, car <chr>,
## #   `cyl / trans` <chr>, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

```r
sample_n(dados, size = 10, replace = T)
```

```
## # A tibble: 10 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 chevrolet c150~   5.3  2008     8 auto~ r        11    15 e     suv      3.32
##  2 volkswag~ gti     2    2008     4 auto~ f        22    29 p     comp~    4.69
##  3 dodge     dako~   4.7  2008     8 auto~ 4        14    19 r     pick~    3.74
##  4 ford      expl~   4.6  2008     8 auto~ 4        13    19 r     suv      3.61
##  5 dodge     cara~   3.8  2008     6 auto~ f        16    23 r     mini~    4   
##  6 chevrolet k150~   5.3  2008     8 auto~ 4        14    19 r     suv      3.74
##  7 dodge     dura~   5.2  1999     8 auto~ 4        11    16 r     suv      3.32
##  8 toyota    camry   2.4  2008     4 manu~ f        21    31 r     mids~    4.58
##  9 toyota    camry   3    1999     6 manu~ f        18    26 r     mids~    4.24
## 10 subaru    impr~   2.2  1999     4 auto~ 4        21    26 r     subc~    4.58
## # ... with 3 more variables: `soma de variáveis` <dbl>, car <chr>,
## #   `cyl / trans` <chr>, and abbreviated variable names 1: manufacturer,
## #   2: sqrt_cty
```

## sample_frac() 


```r
sample_frac(dados, size = 0.1, replace = F)
```

```
## # A tibble: 23 x 15
##    manufac~1 model displ  year   cyl trans drv     cty   hwy fl    class sqrt_~2
##    <fct>     <fct> <dbl> <int> <int> <fct> <fct> <int> <int> <fct> <fct>   <dbl>
##  1 toyota    coro~   1.8  2008     4 manu~ f        28    37 r     comp~    5.29
##  2 lincoln   navi~   5.4  1999     8 auto~ r        11    17 r     suv      3.32
##  3 honda     civic   1.6  1999     4 auto~ f        24    32 r     subc~    4.90
##  4 audi      a6 q~   2.8  1999     6 auto~ 4        15    24 p     mids~    3.87
##  5 nissan    path~   4    2008     6 auto~ 4        14    20 p     suv      3.74
##  6 toyota    camry   3.5  2008     6 auto~ f        19    28 r     mids~    4.36
##  7 subaru    impr~   2.5  2008     4 auto~ 4        20    25 p     comp~    4.47
##  8 toyota    toyo~   3.4  1999     6 auto~ 4        15    19 r     pick~    3.87
##  9 audi      a4 q~   3.1  2008     6 manu~ 4        15    25 p     comp~    3.87
## 10 toyota    coro~   1.8  1999     4 manu~ f        26    35 r     comp~    5.10
## # ... with 13 more rows, 3 more variables: `soma de variáveis` <dbl>,
## #   car <chr>, `cyl / trans` <chr>, and abbreviated variable names
## #   1: manufacturer, 2: sqrt_cty
```

<!--chapter:end:01_dplyr.RMD-->

# ggplot2 (60 minutos)

## Carrega pacotes a serem usados


```r
#install.packages("tidyverse") 
#install.packages("dplyr")     
#install.packages("tidyr")     
#install.packages("ggplot2")   

library(tidyverse)
# Manipulação de dados
#library(dplyr)

# Visualização de gráficos
library(ggplot2)
library(gridExtra)
library(patchwork)
library(plotly)
library(esquisse)

# Para dados gráfico de perfis
library(nlme)
```

Alguns links

[The R Graph Gallery](https://r-graph-gallery.com/)

[120 registered extensions available to explore](https://exts.ggplot2.tidyverse.org/gallery/)

[link 1: patchwork](https://patchwork.data-imaginist.com/articles/guides/assembly.html)

[link 2: patchwork](https://patchwork.data-imaginist.com/articles/patchwork.html)


## Lista de funções do pacote ggplot2


```r
ls("package:ggplot2")
```

## Primeiros passos usando geom_point


```r
dados <- mpg
ggplot(dados)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-1.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-2.pdf)<!-- --> 

```r
# Alternativas
ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point()
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-3.pdf)<!-- --> 

```r
ggplot(dados) + 
  geom_point(aes(x = cty, y = hwy))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-4.pdf)<!-- --> 

```r
ggplot() + 
  geom_point(data = dados, aes(x = cty, y = hwy))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-5.pdf)<!-- --> 

```r
# Fim 

ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point(colour = "red")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-6.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point(colour = "red", size = 6)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-7.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point(colour = "red", size = 6, shape = 10)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-8.pdf)<!-- --> 

```r
# Alternativa
ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point(colour = "red", size = 6, shape = "circle plus")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-9.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point(colour = "red", size = 6, shape = 10)+
  labs(x = "cty (city miles per gallon hwy)", 
       y = "hwy (highway miles per gallon)", 
       title = "Pensar em algum título...", 
       subtitle = "Escrever alguma coisa")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-3-10.pdf)<!-- --> 

### Mais detalhes sobre geom_point 

> geom_point() understands the following aesthetics (required aesthetics are in bold):

  - x

  - y

  - alpha

  - colour

  - fill

  - group

  - shape

  - size

  - stroke
  

```r
ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point()
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-1.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, col = factor(year))) + 
  geom_point() + 
  labs(col = "year")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-2.pdf)<!-- --> 

```r
# Alternativa
ggplot(dados, aes(x = cty, y = hwy, col = factor(class))) + 
  geom_point() + 
  labs(col = "class")+
  scale_color_brewer(type = "qual")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-3.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, col = factor(class))) + 
  geom_point() + 
  labs(col = "class")+
  scale_color_brewer(type = "div")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-4.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, col = factor(class))) + 
  geom_point() + 
  labs(col = "class")+
  scale_color_brewer(palette = "Set1", name = "Tipo de carro")+
  scale_y_continuous(breaks = seq(10,60,3))+
  scale_x_continuous(breaks = seq(10,40,3))+
  theme_minimal()
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-5.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, alpha = factor(year))) + 
  geom_point() + 
  labs(alpha = "year")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-6.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, size = factor(year))) + 
  geom_point() + 
  labs(size = "year")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-7.pdf)<!-- --> 

```r
# Alternativa
ggplot(dados, aes(x = cty, y = hwy, col = cty <= 20)) + 
  geom_point() + 
  geom_vline(xintercept = 20)+
  labs(col = "year")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-8.pdf)<!-- --> 

```r
# Erro comum
ggplot(dados, aes(x = cty, y = hwy, col = "red")) + 
  geom_point()+
  labs(col = "year")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-9.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point(col = "red")+
  labs(col = "year")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-10.pdf)<!-- --> 

```r
# Fim  Erro comum

ggplot(dados, aes(x = cty, y = hwy, shape = factor(year))) + 
  geom_point(col = "red") + 
  labs(shape = "year")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-11.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, size = class)) + 
  geom_point() + 
  labs(size = "class")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-12.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, 
                  size = class, 
                  col = class)) + 
  geom_point() + 
  guides(colour = guide_legend("Tipo de carro (color)"),
         size = guide_legend("Tipo de carro (size)"))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-13.pdf)<!-- --> 

```r
ggplot(dados, aes(x = cty, y = hwy, 
                  size = class, 
                  col = class)) + 
  geom_point() + 
  labs(col = "Tipo de Carro", size = "Tipo de Carro")+
  guides(col = "legend")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-4-14.pdf)<!-- --> 

## smooth, boxplot, histogram



```r
v1<- ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_point(col = "blue")+
  geom_smooth(method = mgcv::gam,
              formula = y ~ s(x, bs = "cs") ,
              col = "red", 
              se = FALSE)
v1
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-5-1.pdf)<!-- --> 

```r
v2 <- ggplot(dados, aes(x = cty)) + 
  geom_boxplot(fill = "red")
v2
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-5-2.pdf)<!-- --> 

```r
v3 <- ggplot(dados, aes(x = cty)) + 
  geom_histogram(bins = 10, fill = "red", col = "blue", lwd=2)
v3
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-5-3.pdf)<!-- --> 

```r
v4<- ggplot(dados, aes(x = cty)) + 
  geom_histogram(aes(y = after_stat(density)),
                 bins = 10, fill = "yellow", col = "red") +
  geom_density(col = "blue", lwd =3)
v4
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-5-4.pdf)<!-- --> 

```r
# Adicional (estatístic experimental)
ggplot(dados, aes(x = drv, y = cty, col = drv)) + 
  geom_boxplot()+
  theme_bw()+
  theme(legend.position = "none")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-5-5.pdf)<!-- --> 

## gridExtra e patchwork


Alguns links

[link 1: patchwork](https://patchwork.data-imaginist.com/articles/guides/assembly.html)

[link 2: patchwork](https://patchwork.data-imaginist.com/articles/patchwork.html)


```r
# gridExtra
grid.arrange(v1, v2, v3, v4) 
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-1.pdf)<!-- --> 

```r
# patchwork
v1 + v2
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-2.pdf)<!-- --> 

```r
v1 | v2
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-3.pdf)<!-- --> 

```r
v1 / v2
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-4.pdf)<!-- --> 

```r
v1 + v2 + v3
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-5.pdf)<!-- --> 

```r
v1 + (v2 + v3)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-6.pdf)<!-- --> 

```r
v1 | (v2 / v3)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-7.pdf)<!-- --> 

```r
v1 / (v2 + v3)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-8.pdf)<!-- --> 

```r
v1 + v2 + v3 + v4 
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-9.pdf)<!-- --> 

```r
v1/(v2+v3+v4)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-10.pdf)<!-- --> 

```r
v1  + (v2 + v3 + v4)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-11.pdf)<!-- --> 

```r
v1  + v2 + (v3 + v4)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-12.pdf)<!-- --> 

```r
(v1 | v2 | v3) / v4
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-6-13.pdf)<!-- --> 


## bar, col, density, density2d


```r
v5 <- ggplot(dados , aes(x = manufacturer)) + 
  geom_bar()+ 
  theme(axis.text.x = element_text(angle = 45))
v5
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-7-1.pdf)<!-- --> 

```r
# Dúvidas no geom_col
v6 <- ggplot(dados , aes(x = manufacturer, y = cty)) + 
  geom_col()+
  theme(axis.text.x = element_text(angle = 45))
v6
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-7-2.pdf)<!-- --> 

```r
dados %>% 
  select(manufacturer, cty) %>% 
  group_by(manufacturer) %>% 
  summarise(soma_total_cty = sum(cty),
            n = n())
```

```
## # A tibble: 15 x 3
##    manufacturer soma_total_cty     n
##    <chr>                 <int> <int>
##  1 audi                    317    18
##  2 chevrolet               285    19
##  3 dodge                   486    37
##  4 ford                    350    25
##  5 honda                   220     9
##  6 hyundai                 261    14
##  7 jeep                    108     8
##  8 land rover               46     4
##  9 lincoln                  34     3
## 10 mercury                  53     4
## 11 nissan                  235    13
## 12 pontiac                  85     5
## 13 subaru                  270    14
## 14 toyota                  630    34
## 15 volkswagen              565    27
```

```r
# dados %>% 
#   filter(manufacturer == "audi") %>% 
#   select(cty) %>% 
#   sum()
v7 <- ggplot(dados , aes(x = cty)) + 
  geom_density()
v7
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-7-3.pdf)<!-- --> 

```r
v8 <- ggplot(dados, aes(x = cty, y = hwy)) + 
  geom_density2d()+
  geom_point(colour = "red")
v8
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-7-4.pdf)<!-- --> 

```r
(v5+v6)/ (v7 + v8)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-7-5.pdf)<!-- --> 

```r
# Deixar pra depois...
 dados %>% 
    select(manufacturer, hwy, year) %>% 
    filter(manufacturer == "audi", year == "1999") %>% 
    summarise(media = max(hwy))
```

```
## # A tibble: 1 x 1
##   media
##   <int>
## 1    29
```


```r
# plotly
ggplotly(
ggplot(dados, aes(x = manufacturer, y = hwy, fill = factor(year))) + 
  geom_col(position = "dodge") + 
  labs(fill = "year") +
  theme(axis.text.x = element_text(angle = 45)))

dados %>% select(manufacturer, hwy, year) %>% 
  group_by(manufacturer, year) %>% 
  summarise(media = mean(hwy))
```


```r
# Para pensar

(dados_trat <- data.frame(tratamento = LETTERS[1:3], 
                         resposta = c(2.3, 1.9, 3.2)))
```

```
##   tratamento resposta
## 1          A      2.3
## 2          B      1.9
## 3          C      3.2
```

```r
ggplot(dados_trat, aes(tratamento, resposta)) +
  geom_col(fill = "red")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-9-1.pdf)<!-- --> 

```r
# Mais detalhes...
dados %>% select(manufacturer, hwy, year) %>% 
  group_by(manufacturer, year) %>% 
  summarise(media = mean(hwy), .groups = "drop") %>% 
  ggplot(aes(x = manufacturer, y = media, fill = factor(year)))+
  geom_col(position = "dodge")+
  labs(fill = "year") +
  theme(axis.text.x = element_text(angle = 45))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-9-2.pdf)<!-- --> 

## facet_grid, facet_wrap


```r
p1<- ggplot(dados, aes(x = cty, y = hwy)) +
  geom_point()
p1
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-1.pdf)<!-- --> 

```r
p1 + facet_grid(rows = vars(cyl))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-2.pdf)<!-- --> 

```r
p1 + facet_grid(cols = vars(cyl))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-3.pdf)<!-- --> 

```r
p1 + facet_grid(~cyl)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-4.pdf)<!-- --> 

```r
p1 + facet_grid(rows = vars(year), cols =vars(cyl))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-5.pdf)<!-- --> 

```r
p1 + facet_grid(year~cyl)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-6.pdf)<!-- --> 

```r
p1 + facet_wrap(year ~ cyl)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-7.pdf)<!-- --> 

```r
p1 + facet_wrap(cyl ~ year)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-8.pdf)<!-- --> 

```r
p1 + facet_wrap(~cyl + year)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-9.pdf)<!-- --> 

```r
p1 + facet_wrap(~year + cyl)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-10.pdf)<!-- --> 

```r
p1 + facet_wrap(year ~ cyl, ncol = 4)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-11.pdf)<!-- --> 

```r
p1 + facet_wrap(cyl ~ year, ncol = 4)
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-10-12.pdf)<!-- --> 


## stat_function


```r
a<- -3 # média
b<- 4  # desv. padrão
ggplot(data.frame(x = c(a - 3*b, a + 3*b)), aes(x)) + 
  stat_function(fun = dnorm, args = list(mean = a, sd = b))+
  geom_vline(xintercept = c(a - 3*b, a, a + 3*b), col = "red", lty = 2)+
  theme_minimal()
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-11-1.pdf)<!-- --> 

## stat_summary


```r
ggplot(dados, aes(x = manufacturer, y = hwy)) + 
  geom_boxplot()+
  geom_point(col = "red", size=0.8)+
  stat_summary(fun = mean, col = "blue")+
  theme_minimal()+
  theme(axis.text.x = element_text(angle = 45))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-12-1.pdf)<!-- --> 

## theme_*()


```r
a1<- p1 + theme_bw() + labs(title = "theme_bw()")
a2<- p1 + theme_classic() + labs(title = "theme_classic()")
a3<- p1 + theme_light() + labs(title = "theme_light()")
a4<- p1 + theme_minimal() + labs(title = "theme_minimal()")

a1 + a2 + a3 + a4
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-13-1.pdf)<!-- --> 


## Gráfico de perfis (Spaguetti plot)


```r
glimpse(Orange)
```

```
## Rows: 35
## Columns: 3
## $ Tree          <ord> 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 3,~
## $ age           <dbl> 118, 484, 664, 1004, 1231, 1372, 1582, 118, 484, 664, 10~
## $ circumference <dbl> 30, 58, 87, 115, 120, 142, 145, 33, 69, 111, 156, 172, 2~
```

```r
ggplot(Orange, aes(x = age, y = circumference, group = Tree, 
                   col = Tree)) +
  geom_line()+
  stat_summary(aes(group = 1), fun = mean, col = "red", 
               geom = "line", size = 1, show.legend = FALSE,
               linetype = 2)+
  xlim(0, 1600)+
  theme_minimal()
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-14-1.pdf)<!-- --> 

```r
ggplot(Orange, aes(x = age, y = circumference, group = Tree)) +
  geom_line()+
  xlim(0, 1600)+
  facet_wrap(~Tree)+
  theme_minimal()+
  theme(legend.position = "none")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-14-2.pdf)<!-- --> 

## plotly

[plotly cran](https://cran.r-project.org/web/packages/plotly/index.html)

[Interactive web-based data visualization with R, plotly, and shiny](https://plotly-r.com/)

[Plotly R Open Source Graphing Library](https://plotly.com/r/)


```r
ggplotly(v1)
ggplotly(v2)
ggplotly(v4)
ggplotly(v5)
```


## esquisse

Alguns links de interesse

[esquisse](https://cran.r-project.org/web/packages/esquisse/vignettes/get-started.html)

[esquisse + shiny](https://cran.r-project.org/web/packages/esquisse/vignettes/shiny-usage.html)


```r
esquisser(dados)
```

## Exemplo esquisse


```r
ggplot(dados) +
  aes(x = displ, y = hwy, colour = drv) +
  geom_point(shape = "circle", size = 1.85) +
  scale_color_hue(direction = 1) +
  theme_minimal() +
  theme(legend.position = "top")
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-17-1.pdf)<!-- --> 



```r
ggplot(dados) +
  aes(x = displ, y = cty, colour = class, size = cty) +
  geom_point(shape = "circle") +
  scale_color_hue(direction = 1) +
  theme(legend.position = "top") +
  facet_wrap(vars(drv))
```

![](02_ggplot2_files/figure-latex/unnamed-chunk-18-1.pdf)<!-- --> 


<!--chapter:end:02_ggplot2.Rmd-->

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

## map functions


```r
example("map")
```

```
## 
## map> # Compute normal distributions from an atomic vector
## map> 1:10 |>
## map+   map(rnorm, n = 10)
## [[1]]
##  [1]  0.7706814  1.0778917  0.8659079 -0.5469408 -0.6835418  0.8185052
##  [7] -0.1467090  0.8540078  1.9580895 -0.5476033
## 
## [[2]]
##  [1] 1.3271860 1.4684349 2.7523912 2.6571434 0.5828927 2.7418533 1.2359310
##  [8] 0.8474503 1.9426752 0.5121808
## 
## [[3]]
##  [1] 2.886344 2.995391 6.093176 3.096349 2.884517 3.433177 2.200988 1.974804
##  [9] 4.252117 2.932833
## 
## [[4]]
##  [1] 2.467539 3.363974 4.881392 4.834386 5.120413 4.049837 3.772937 3.743463
##  [9] 3.765030 5.549189
## 
## [[5]]
##  [1] 5.175840 5.905306 5.333564 5.913896 2.592296 4.159747 3.518124 4.206779
##  [9] 6.400150 2.825167
## 
## [[6]]
##  [1] 6.777801 5.180100 4.145228 5.645151 6.386505 4.163602 5.698994 6.845392
##  [9] 5.038437 5.085129
## 
## [[7]]
##  [1] 5.576387 6.676060 5.944081 6.835571 5.869919 4.695889 7.055215 7.622597
##  [9] 6.955308 5.657366
## 
## [[8]]
##  [1] 8.183812 8.501903 8.814359 6.343407 9.023586 9.452663 8.179030 8.500478
##  [9] 8.068492 7.710221
## 
## [[9]]
##  [1]  8.543773  7.496393  8.989845 10.199891  7.013487  9.702990  9.038455
##  [8]  7.256682  8.318006  7.916599
## 
## [[10]]
##  [1] 10.456244 10.647667 10.055424 10.739523  9.630510 13.137922 10.563972
##  [8] 11.324160  9.931244  9.063508
## 
## 
## map> # You can also use an anonymous function
## map> 1:10 |>
## map+   map(\(x) rnorm(10, x))
## [[1]]
##  [1] 0.5374114 0.3875516 0.9453895 1.2634531 0.1375999 1.0597011 0.7086053
##  [8] 0.8226393 0.9097425 0.1562656
## 
## [[2]]
##  [1] 3.6041744 1.3249959 1.4300217 0.8034132 2.6248112 1.6596241 1.5246083
##  [8] 1.9046861 0.6928620 1.7345800
## 
## [[3]]
##  [1] 2.328563 2.929934 1.802092 2.777194 2.721814 3.304734 1.880340 2.312416
##  [9] 1.442148 2.884682
## 
## [[4]]
##  [1] 2.371379 5.768774 5.935330 3.683650 5.656505 4.145886 4.442122 4.650919
##  [9] 6.721979 4.686662
## 
## [[5]]
##  [1] 3.830025 5.100294 3.520947 5.185729 4.328423 5.615221 5.927575 3.416533
##  [9] 2.525554 4.241039
## 
## [[6]]
##  [1] 4.942078 3.596742 6.337023 6.462203 5.816146 6.774359 5.792085 5.056070
##  [9] 7.062996 7.215999
## 
## [[7]]
##  [1] 7.747120 8.636978 7.186557 6.431169 6.923283 6.596002 8.015734 4.919695
##  [9] 6.295227 5.439257
## 
## [[8]]
##  [1]  7.495199 11.051554  9.119173  9.200069  8.753870  6.943532  7.326731
##  [8]  7.454786  8.531817  9.007250
## 
## [[9]]
##  [1]  9.244614  8.035186  8.984489 10.070108  8.205788  8.529548 10.229356
##  [8]  8.444626  9.084566  7.354850
## 
## [[10]]
##  [1]  9.571777  8.972963 11.156868 10.382397 10.082096 11.615919 11.209532
##  [8]  9.478363  9.367625  9.573103
## 
## 
## map> # Simplify output to a vector instead of a list by computing the mean of the distributions
## map> 1:10 |>
## map+   map(rnorm, n = 10) |>  # output a list
## map+   map_dbl(mean)           # output an atomic vector
##  [1]  0.8078029  1.8166889  3.3181177  4.1376363  4.8694183  6.1056248
##  [7]  6.5827590  7.7742234  8.1784520 10.1540693
## 
## map> # Using set_names() with character vectors is handy to keep track
## map> # of the original inputs:
## map> set_names(c("foo", "bar")) |> map_chr(paste0, ":suffix")
##          foo          bar 
## "foo:suffix" "bar:suffix" 
## 
## map> # Working with lists
## map> favorite_desserts <- list(Sophia = "banana bread", Eliott = "pancakes", Karina = "chocolate cake")
## 
## map> favorite_desserts |> map_chr(\(food) paste(food, "rocks!"))
##                  Sophia                  Eliott                  Karina 
##   "banana bread rocks!"       "pancakes rocks!" "chocolate cake rocks!" 
## 
## map> # Extract by name or position
## map> # .default specifies value for elements that are missing or NULL
## map> l1 <- list(list(a = 1L), list(a = NULL, b = 2L), list(b = 3L))
## 
## map> l1 |> map("a", .default = "???")
## [[1]]
## [1] 1
## 
## [[2]]
## [1] "???"
## 
## [[3]]
## [1] "???"
## 
## 
## map> l1 |> map_int("b", .default = NA)
## [1] NA  2  3
## 
## map> l1 |> map_int(2, .default = NA)
## [1] NA  2 NA
## 
## map> # Supply multiple values to index deeply into a list
## map> l2 <- list(
## map+   list(num = 1:3,     letters[1:3]),
## map+   list(num = 101:103, letters[4:6]),
## map+   list()
## map+ )
## 
## map> l2 |> map(c(2, 2))
## [[1]]
## [1] "b"
## 
## [[2]]
## [1] "e"
## 
## [[3]]
## NULL
## 
## 
## map> # Use a list to build an extractor that mixes numeric indices and names,
## map> # and .default to provide a default value if the element does not exist
## map> l2 |> map(list("num", 3))
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 103
## 
## [[3]]
## NULL
## 
## 
## map> l2 |> map_int(list("num", 3), .default = NA)
## [1]   3 103  NA
## 
## map> # Working with data frames
## map> # Use map_lgl(), map_dbl(), etc to return a vector instead of a list:
## map> mtcars |> map_dbl(sum)
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  642.900  198.000 7383.100 4694.000  115.090  102.952  571.160   14.000 
##       am     gear     carb 
##   13.000  118.000   90.000 
## 
## map> # A more realistic example: split a data frame into pieces, fit a
## map> # model to each piece, summarise and extract R^2
## map> mtcars |>
## map+   split(mtcars$cyl) |>
## map+   map(\(df) lm(mpg ~ wt, data = df)) |>
## map+   map(summary) |>
## map+   map_dbl("r.squared")
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
example("map_at")
```

```
## 
## map_at> # Use a predicate function to decide whether to map a function:
## map_at> iris |> map_if(is.factor, as.character) |> str()
## List of 5
##  $ Sepal.Length: num [1:150] 5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num [1:150] 3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num [1:150] 1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num [1:150] 0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : chr [1:150] "setosa" "setosa" "setosa" "setosa" ...
## 
## map_at> # Specify an alternative with the `.else` argument:
## map_at> iris |> map_if(is.factor, as.character, .else = as.integer) |> str()
## List of 5
##  $ Sepal.Length: int [1:150] 5 4 4 4 5 5 4 5 4 4 ...
##  $ Sepal.Width : int [1:150] 3 3 3 3 3 3 3 3 2 3 ...
##  $ Petal.Length: int [1:150] 1 1 1 1 1 1 1 1 1 1 ...
##  $ Petal.Width : int [1:150] 0 0 0 0 0 0 0 0 0 0 ...
##  $ Species     : chr [1:150] "setosa" "setosa" "setosa" "setosa" ...
## 
## map_at> # Use numeric vector of positions select elements to change:
## map_at> iris |> map_at(c(4, 5), is.numeric) |> str()
## List of 5
##  $ Sepal.Length: num [1:150] 5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num [1:150] 3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num [1:150] 1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : logi TRUE
##  $ Species     : logi FALSE
## 
## map_at> # Use vector of names to specify which elements to change:
## map_at> iris |> map_at("Species", toupper) |> str()
## List of 5
##  $ Sepal.Length: num [1:150] 5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num [1:150] 3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num [1:150] 1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num [1:150] 0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : chr [1:150] "SETOSA" "SETOSA" "SETOSA" "SETOSA" ...
```

```r
example("map_chr")
```

```
## 
## mp_chr> # Compute normal distributions from an atomic vector
## mp_chr> 1:10 |>
## mp_chr+   map(rnorm, n = 10)
## [[1]]
##  [1] 0.63939931 0.49002056 2.80050374 0.75889095 0.04263668 2.11756440
##  [7] 2.89833971 0.95982599 1.09605472 1.04042177
## 
## [[2]]
##  [1] 2.962975 1.185815 2.549648 1.855881 1.132906 2.040768 2.468693 3.206737
##  [9] 2.526103 2.141372
## 
## [[3]]
##  [1] 1.583703 2.093150 3.145986 3.387898 2.811265 3.165860 3.622801 2.628895
##  [9] 2.824235 2.769992
## 
## [[4]]
##  [1] 5.3685542 2.6983121 2.6651234 3.8276251 3.3072521 0.9343684 4.7563190
##  [8] 4.2424715 4.6361754 5.2986951
## 
## [[5]]
##  [1] 5.582627 3.992861 5.950463 4.312486 6.244274 4.969177 5.050664 5.451345
##  [9] 5.321688 4.286701
## 
## [[6]]
##  [1] 5.765523 6.034296 6.303868 5.417565 4.865683 5.759894 5.578559 6.721226
##  [9] 4.532542 6.250637
## 
## [[7]]
##  [1] 6.820783 7.437017 6.358082 7.473710 7.486257 8.048095 6.551808 5.139374
##  [9] 7.981895 8.490175
## 
## [[8]]
##  [1] 7.392960 7.011786 8.064788 7.594251 9.431106 8.561478 8.977753 6.817360
##  [9] 6.672255 7.469237
## 
## [[9]]
##  [1]  9.592080  9.687880  8.742280 10.072156  7.816203  9.907760  9.795218
##  [8]  8.128724  7.589979  8.407206
## 
## [[10]]
##  [1]  9.710385 10.236900  9.957201  9.541138  9.786400  9.430467 10.984872
##  [8] 10.618524  8.707406 10.275645
## 
## 
## mp_chr> # You can also use an anonymous function
## mp_chr> 1:10 |>
## mp_chr+   map(\(x) rnorm(10, x))
## [[1]]
##  [1] 3.2618682 2.1020976 0.2644421 2.5366012 2.3743961 0.7832418 1.0753367
##  [8] 0.5068742 2.0326070 1.0010100
## 
## [[2]]
##  [1] 2.2406044 2.5231579 1.4432487 1.6835522 0.2483248 2.1079797 1.3770062
##  [8] 1.8408663 3.7514505 3.3806638
## 
## [[3]]
##  [1] 3.753080 3.423619 2.735414 1.767156 1.768500 3.089558 3.105553 4.368615
##  [9] 2.636255 2.035776
## 
## [[4]]
##  [1] 3.853306 5.568344 6.403811 4.272954 4.582583 4.325278 4.044500 5.654508
##  [9] 3.647882 6.433293
## 
## [[5]]
##  [1] 4.045760 5.742547 5.792165 5.036698 3.807530 5.184332 7.986978 6.038416
##  [9] 6.485404 5.446028
## 
## [[6]]
##  [1] 5.146146 6.210449 7.416705 5.389370 7.694983 6.090558 7.257279 7.346927
##  [9] 5.294723 4.728695
## 
## [[7]]
##  [1] 7.121143 9.469610 7.338385 7.600939 7.945195 7.835490 8.798118 4.510841
##  [9] 7.253536 8.192856
## 
## [[8]]
##  [1] 7.505081 7.854772 7.218210 8.105936 7.931782 8.247612 8.519944 9.247297
##  [9] 5.568718 7.386539
## 
## [[9]]
##  [1]  9.935282  9.583177  9.993144  9.520430  9.260083  9.460926  9.624162
##  [8]  8.384939  8.952531 11.048061
## 
## [[10]]
##  [1]  9.625063 11.621902 11.014155  9.961205  8.986575  9.764876 10.414404
##  [8]  8.599048 10.947245  8.802055
## 
## 
## mp_chr> # Simplify output to a vector instead of a list by computing the mean of the distributions
## mp_chr> 1:10 |>
## mp_chr+   map(rnorm, n = 10) |>  # output a list
## mp_chr+   map_dbl(mean)           # output an atomic vector
##  [1]  1.229743  2.312451  3.090747  4.191184  4.686815  6.479546  6.874953
##  [8]  7.762505  9.269010 10.215922
## 
## mp_chr> # Using set_names() with character vectors is handy to keep track
## mp_chr> # of the original inputs:
## mp_chr> set_names(c("foo", "bar")) |> map_chr(paste0, ":suffix")
##          foo          bar 
## "foo:suffix" "bar:suffix" 
## 
## mp_chr> # Working with lists
## mp_chr> favorite_desserts <- list(Sophia = "banana bread", Eliott = "pancakes", Karina = "chocolate cake")
## 
## mp_chr> favorite_desserts |> map_chr(\(food) paste(food, "rocks!"))
##                  Sophia                  Eliott                  Karina 
##   "banana bread rocks!"       "pancakes rocks!" "chocolate cake rocks!" 
## 
## mp_chr> # Extract by name or position
## mp_chr> # .default specifies value for elements that are missing or NULL
## mp_chr> l1 <- list(list(a = 1L), list(a = NULL, b = 2L), list(b = 3L))
## 
## mp_chr> l1 |> map("a", .default = "???")
## [[1]]
## [1] 1
## 
## [[2]]
## [1] "???"
## 
## [[3]]
## [1] "???"
## 
## 
## mp_chr> l1 |> map_int("b", .default = NA)
## [1] NA  2  3
## 
## mp_chr> l1 |> map_int(2, .default = NA)
## [1] NA  2 NA
## 
## mp_chr> # Supply multiple values to index deeply into a list
## mp_chr> l2 <- list(
## mp_chr+   list(num = 1:3,     letters[1:3]),
## mp_chr+   list(num = 101:103, letters[4:6]),
## mp_chr+   list()
## mp_chr+ )
## 
## mp_chr> l2 |> map(c(2, 2))
## [[1]]
## [1] "b"
## 
## [[2]]
## [1] "e"
## 
## [[3]]
## NULL
## 
## 
## mp_chr> # Use a list to build an extractor that mixes numeric indices and names,
## mp_chr> # and .default to provide a default value if the element does not exist
## mp_chr> l2 |> map(list("num", 3))
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 103
## 
## [[3]]
## NULL
## 
## 
## mp_chr> l2 |> map_int(list("num", 3), .default = NA)
## [1]   3 103  NA
## 
## mp_chr> # Working with data frames
## mp_chr> # Use map_lgl(), map_dbl(), etc to return a vector instead of a list:
## mp_chr> mtcars |> map_dbl(sum)
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  642.900  198.000 7383.100 4694.000  115.090  102.952  571.160   14.000 
##       am     gear     carb 
##   13.000  118.000   90.000 
## 
## mp_chr> # A more realistic example: split a data frame into pieces, fit a
## mp_chr> # model to each piece, summarise and extract R^2
## mp_chr> mtcars |>
## mp_chr+   split(mtcars$cyl) |>
## mp_chr+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp_chr+   map(summary) |>
## mp_chr+   map_dbl("r.squared")
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
example("map_dbl")
```

```
## 
## mp_dbl> # Compute normal distributions from an atomic vector
## mp_dbl> 1:10 |>
## mp_dbl+   map(rnorm, n = 10)
## [[1]]
##  [1]  0.53587595 -0.04405526  1.27502932  0.75630555  0.13100821  0.76211669
##  [7]  2.24217024  0.29932788 -0.31677433  1.00821230
## 
## [[2]]
##  [1] 1.0745837 3.6257795 2.3226349 2.3025742 3.0152779 0.2261725 1.4452026
##  [8] 3.6173448 3.0386915 1.8350511
## 
## [[3]]
##  [1] 3.339814 2.994191 2.549963 4.529949 2.662812 1.818848 3.144193 1.357303
##  [9] 1.658886 3.590226
## 
## [[4]]
##  [1] 1.950475 5.066051 4.417312 4.226219 5.078705 5.066559 4.272570 4.489154
##  [9] 3.426650 3.403009
## 
## [[5]]
##  [1] 4.854769 5.910342 5.986890 4.144259 6.526932 5.319600 4.536216 5.385695
##  [9] 2.825945 5.256420
## 
## [[6]]
##  [1] 6.254164 6.334346 7.735498 6.564409 7.059149 4.872487 7.313520 6.258936
##  [9] 5.870238 5.186830
## 
## [[7]]
##  [1] 6.446430 6.284641 4.804172 6.181554 7.184561 6.714888 6.015607 6.695444
##  [9] 7.234394 5.979753
## 
## [[8]]
##  [1] 6.686459 8.430734 8.199370 7.163894 7.809162 6.284847 8.041634 7.983855
##  [9] 8.492824 7.399897
## 
## [[9]]
##  [1]  9.424616  9.512675  7.345365  9.931990  9.230038 11.647829  9.080870
##  [8]  9.409414  8.310752 10.551496
## 
## [[10]]
##  [1]  7.854324  9.799220 11.232986  9.794871  9.774677 11.402935 11.563191
##  [8] 10.571139 10.193928 10.378227
## 
## 
## mp_dbl> # You can also use an anonymous function
## mp_dbl> 1:10 |>
## mp_dbl+   map(\(x) rnorm(10, x))
## [[1]]
##  [1] 0.3321349 0.3558975 0.3036189 1.3119287 1.3288096 1.4576604 2.0896836
##  [8] 0.6083906 1.6097178 0.1585393
## 
## [[2]]
##  [1] 2.5838217 1.5105984 0.4269778 3.9566939 0.5650197 2.2092442 2.2829381
##  [8] 1.8690362 0.9227519 0.7318775
## 
## [[3]]
##  [1] 3.239617 2.321448 2.109813 2.142451 3.431338 2.705153 3.491334 2.025043
##  [9] 2.405860 4.003753
## 
## [[4]]
##  [1]  5.0953858756  3.6476607669  3.6491819324  4.7672021667 -0.0003282979
##  [6]  3.3737555187  2.2702243965  5.0774009435  4.5221141329  3.8501193698
## 
## [[5]]
##  [1] 4.492351 5.096688 4.777393 4.723137 5.757002 3.249649 5.812082 5.245861
##  [9] 4.088867 4.505730
## 
## [[6]]
##  [1] 6.596431 4.824518 6.046853 7.276412 4.979410 4.172086 6.886653 5.409845
##  [9] 5.861187 6.415654
## 
## [[7]]
##  [1] 6.378847 7.924404 6.929478 8.709194 6.623437 6.563100 6.263966 5.476350
##  [9] 5.911773 6.715885
## 
## [[8]]
##  [1] 7.158751 6.982809 7.501969 6.992835 7.467657 9.322410 8.072033 8.793120
##  [9] 7.281489 6.816812
## 
## [[9]]
##  [1]  8.688610  8.203706  7.956347  9.080909  8.037857 10.321655  7.403217
##  [8]  9.800003  9.812086  7.899123
## 
## [[10]]
##  [1] 10.122564  9.449031 11.178114  9.517247  9.961643 11.199039 10.615914
##  [8] 10.972602  9.775351 10.231673
## 
## 
## mp_dbl> # Simplify output to a vector instead of a list by computing the mean of the distributions
## mp_dbl> 1:10 |>
## mp_dbl+   map(rnorm, n = 10) |>  # output a list
## mp_dbl+   map_dbl(mean)           # output an atomic vector
##  [1] 0.9670105 2.1371682 2.8590074 3.7489292 4.9747756 5.9369291 6.6914686
##  [8] 7.7690124 9.3004427 9.7587477
## 
## mp_dbl> # Using set_names() with character vectors is handy to keep track
## mp_dbl> # of the original inputs:
## mp_dbl> set_names(c("foo", "bar")) |> map_chr(paste0, ":suffix")
##          foo          bar 
## "foo:suffix" "bar:suffix" 
## 
## mp_dbl> # Working with lists
## mp_dbl> favorite_desserts <- list(Sophia = "banana bread", Eliott = "pancakes", Karina = "chocolate cake")
## 
## mp_dbl> favorite_desserts |> map_chr(\(food) paste(food, "rocks!"))
##                  Sophia                  Eliott                  Karina 
##   "banana bread rocks!"       "pancakes rocks!" "chocolate cake rocks!" 
## 
## mp_dbl> # Extract by name or position
## mp_dbl> # .default specifies value for elements that are missing or NULL
## mp_dbl> l1 <- list(list(a = 1L), list(a = NULL, b = 2L), list(b = 3L))
## 
## mp_dbl> l1 |> map("a", .default = "???")
## [[1]]
## [1] 1
## 
## [[2]]
## [1] "???"
## 
## [[3]]
## [1] "???"
## 
## 
## mp_dbl> l1 |> map_int("b", .default = NA)
## [1] NA  2  3
## 
## mp_dbl> l1 |> map_int(2, .default = NA)
## [1] NA  2 NA
## 
## mp_dbl> # Supply multiple values to index deeply into a list
## mp_dbl> l2 <- list(
## mp_dbl+   list(num = 1:3,     letters[1:3]),
## mp_dbl+   list(num = 101:103, letters[4:6]),
## mp_dbl+   list()
## mp_dbl+ )
## 
## mp_dbl> l2 |> map(c(2, 2))
## [[1]]
## [1] "b"
## 
## [[2]]
## [1] "e"
## 
## [[3]]
## NULL
## 
## 
## mp_dbl> # Use a list to build an extractor that mixes numeric indices and names,
## mp_dbl> # and .default to provide a default value if the element does not exist
## mp_dbl> l2 |> map(list("num", 3))
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 103
## 
## [[3]]
## NULL
## 
## 
## mp_dbl> l2 |> map_int(list("num", 3), .default = NA)
## [1]   3 103  NA
## 
## mp_dbl> # Working with data frames
## mp_dbl> # Use map_lgl(), map_dbl(), etc to return a vector instead of a list:
## mp_dbl> mtcars |> map_dbl(sum)
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  642.900  198.000 7383.100 4694.000  115.090  102.952  571.160   14.000 
##       am     gear     carb 
##   13.000  118.000   90.000 
## 
## mp_dbl> # A more realistic example: split a data frame into pieces, fit a
## mp_dbl> # model to each piece, summarise and extract R^2
## mp_dbl> mtcars |>
## mp_dbl+   split(mtcars$cyl) |>
## mp_dbl+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp_dbl+   map(summary) |>
## mp_dbl+   map_dbl("r.squared")
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
example("map_df")
```

```
## 
## map_df> # map ---------------------------------------------
## map_df> # Was:
## map_df> mtcars |>
## map_df+   split(mtcars$cyl) |>
## map_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## map_df+   map_dfr(\(mod) as.data.frame(t(as.matrix(coef(mod)))))
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## map_df> # Now:
## map_df> mtcars |>
## map_df+   split(mtcars$cyl) |>
## map_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## map_df+   map(\(mod) as.data.frame(t(as.matrix(coef(mod))))) |>
## map_df+   list_rbind()
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## map_df> # map2 ---------------------------------------------
## map_df> 
## map_df> ex_fun <- function(arg1, arg2){
## map_df+   col <- arg1 + arg2
## map_df+   x <- as.data.frame(col)
## map_df+ }
## 
## map_df> arg1 <- 1:4
## 
## map_df> arg2 <- 10:13
## 
## map_df> # was
## map_df> map2_dfr(arg1, arg2, ex_fun)
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## map_df> # now
## map_df> map2(arg1, arg2, ex_fun) |> list_rbind()
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## map_df> # was
## map_df> map2_dfc(arg1, arg2, ex_fun)
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
## 
## map_df> # now
## map_df> map2(arg1, arg2, ex_fun) |> list_cbind()
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
```

```r
example("map_dfc")
```

```
## 
## mp_dfc> # map ---------------------------------------------
## mp_dfc> # Was:
## mp_dfc> mtcars |>
## mp_dfc+   split(mtcars$cyl) |>
## mp_dfc+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp_dfc+   map_dfr(\(mod) as.data.frame(t(as.matrix(coef(mod)))))
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp_dfc> # Now:
## mp_dfc> mtcars |>
## mp_dfc+   split(mtcars$cyl) |>
## mp_dfc+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp_dfc+   map(\(mod) as.data.frame(t(as.matrix(coef(mod))))) |>
## mp_dfc+   list_rbind()
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp_dfc> # map2 ---------------------------------------------
## mp_dfc> 
## mp_dfc> ex_fun <- function(arg1, arg2){
## mp_dfc+   col <- arg1 + arg2
## mp_dfc+   x <- as.data.frame(col)
## mp_dfc+ }
## 
## mp_dfc> arg1 <- 1:4
## 
## mp_dfc> arg2 <- 10:13
## 
## mp_dfc> # was
## mp_dfc> map2_dfr(arg1, arg2, ex_fun)
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp_dfc> # now
## mp_dfc> map2(arg1, arg2, ex_fun) |> list_rbind()
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp_dfc> # was
## mp_dfc> map2_dfc(arg1, arg2, ex_fun)
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
## 
## mp_dfc> # now
## mp_dfc> map2(arg1, arg2, ex_fun) |> list_cbind()
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
```

```r
example("map_dfr")
```

```
## 
## mp_dfr> # map ---------------------------------------------
## mp_dfr> # Was:
## mp_dfr> mtcars |>
## mp_dfr+   split(mtcars$cyl) |>
## mp_dfr+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp_dfr+   map_dfr(\(mod) as.data.frame(t(as.matrix(coef(mod)))))
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp_dfr> # Now:
## mp_dfr> mtcars |>
## mp_dfr+   split(mtcars$cyl) |>
## mp_dfr+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp_dfr+   map(\(mod) as.data.frame(t(as.matrix(coef(mod))))) |>
## mp_dfr+   list_rbind()
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp_dfr> # map2 ---------------------------------------------
## mp_dfr> 
## mp_dfr> ex_fun <- function(arg1, arg2){
## mp_dfr+   col <- arg1 + arg2
## mp_dfr+   x <- as.data.frame(col)
## mp_dfr+ }
## 
## mp_dfr> arg1 <- 1:4
## 
## mp_dfr> arg2 <- 10:13
## 
## mp_dfr> # was
## mp_dfr> map2_dfr(arg1, arg2, ex_fun)
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp_dfr> # now
## mp_dfr> map2(arg1, arg2, ex_fun) |> list_rbind()
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp_dfr> # was
## mp_dfr> map2_dfc(arg1, arg2, ex_fun)
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
## 
## mp_dfr> # now
## mp_dfr> map2(arg1, arg2, ex_fun) |> list_cbind()
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
```

```r
example("map_int")
```

```
## 
## map_nt> # Compute normal distributions from an atomic vector
## map_nt> 1:10 |>
## map_nt+   map(rnorm, n = 10)
## [[1]]
##  [1] 0.7834910 1.0440573 0.9293196 0.9037143 0.5395108 1.4495532 1.4511843
##  [8] 1.6737782 1.5536728 1.1446487
## 
## [[2]]
##  [1] 0.1447770 2.1335774 0.2559423 1.2827102 1.9435746 2.2228966 1.4139859
##  [8] 1.5309665 2.0556086 2.8598577
## 
## [[3]]
##  [1] 2.4266525 2.4569519 2.6824990 1.9092748 0.9500857 5.0637002 5.0825716
##  [8] 3.8781669 4.0863374 1.6570133
## 
## [[4]]
##  [1] 3.742007 3.277264 4.667187 4.497702 4.355476 3.479650 2.871682 3.735486
##  [9] 6.052162 4.747481
## 
## [[5]]
##  [1] 5.116282 5.348842 4.926826 6.067386 6.600608 5.334395 4.632454 5.322020
##  [9] 3.060201 5.159946
## 
## [[6]]
##  [1] 7.864940 5.499341 6.654130 4.144052 5.638742 5.909119 6.092944 5.384642
##  [9] 4.049731 6.976941
## 
## [[7]]
##  [1] 7.417704 7.336168 7.028278 5.496210 6.827893 5.832918 8.430474 9.464063
##  [9] 4.975849 4.918261
## 
## [[8]]
##  [1] 6.909941 8.285957 7.677557 8.657568 7.779786 7.453379 8.462023 8.433546
##  [9] 8.531569 5.565174
## 
## [[9]]
##  [1]  7.101523  9.556897  5.889971  8.777200  8.775967  6.910625  8.927280
##  [8] 10.170874  9.529269  8.016478
## 
## [[10]]
##  [1]  9.397874  8.892540  9.735682  9.688379 11.207517  9.029532 10.977393
##  [8] 10.847355  9.804742 12.288762
## 
## 
## map_nt> # You can also use an anonymous function
## map_nt> 1:10 |>
## map_nt+   map(\(x) rnorm(10, x))
## [[1]]
##  [1] 1.4042135 0.1088515 1.9184566 1.8008463 1.4184782 0.8880164 0.4501262
##  [8] 0.6701458 0.4154933 3.8179951
## 
## [[2]]
##  [1]  3.5480907  1.5710389  3.9140750  4.1220447  1.3134506  2.6950831
##  [7]  2.4581271 -0.1078862  1.9300298  3.2687681
## 
## [[3]]
##  [1] 4.2780208 2.4446873 3.0411242 2.2748659 0.5869952 1.4208429 3.8129910
##  [8] 4.0242742 2.9900054 2.1262215
## 
## [[4]]
##  [1] 5.103283 4.837564 6.506807 6.201569 5.410327 3.692716 4.123327 4.269119
##  [9] 3.721392 2.779414
## 
## [[5]]
##  [1] 4.186933 3.057441 5.193329 4.825344 4.560222 3.934172 5.661010 4.885111
##  [9] 6.591666 5.529428
## 
## [[6]]
##  [1] 7.258292 6.402779 5.839974 5.393996 6.663928 6.998217 5.677656 7.038256
##  [9] 6.318093 6.843341
## 
## [[7]]
##  [1] 6.848441 6.144532 6.961344 6.804025 8.405646 8.011103 6.232490 6.115354
##  [9] 6.663552 7.884361
## 
## [[8]]
##  [1] 7.580154 7.383678 6.961426 6.188367 7.152232 8.907791 7.569484 9.195889
##  [9] 7.107187 7.550929
## 
## [[9]]
##  [1]  9.236595  7.858210 10.035025  6.716339  8.093876  9.885570  9.827929
##  [8]  6.966665  8.749372  8.360615
## 
## [[10]]
##  [1] 10.281462  9.004887  9.302069  9.545560 10.561598  9.918449 11.307889
##  [8] 10.522577 10.983411 10.315303
## 
## 
## map_nt> # Simplify output to a vector instead of a list by computing the mean of the distributions
## map_nt> 1:10 |>
## map_nt+   map(rnorm, n = 10) |>  # output a list
## map_nt+   map_dbl(mean)           # output an atomic vector
##  [1] 1.619356 1.492227 2.689051 4.249763 4.978264 6.045021 7.789882 8.690897
##  [9] 9.424162 9.949567
## 
## map_nt> # Using set_names() with character vectors is handy to keep track
## map_nt> # of the original inputs:
## map_nt> set_names(c("foo", "bar")) |> map_chr(paste0, ":suffix")
##          foo          bar 
## "foo:suffix" "bar:suffix" 
## 
## map_nt> # Working with lists
## map_nt> favorite_desserts <- list(Sophia = "banana bread", Eliott = "pancakes", Karina = "chocolate cake")
## 
## map_nt> favorite_desserts |> map_chr(\(food) paste(food, "rocks!"))
##                  Sophia                  Eliott                  Karina 
##   "banana bread rocks!"       "pancakes rocks!" "chocolate cake rocks!" 
## 
## map_nt> # Extract by name or position
## map_nt> # .default specifies value for elements that are missing or NULL
## map_nt> l1 <- list(list(a = 1L), list(a = NULL, b = 2L), list(b = 3L))
## 
## map_nt> l1 |> map("a", .default = "???")
## [[1]]
## [1] 1
## 
## [[2]]
## [1] "???"
## 
## [[3]]
## [1] "???"
## 
## 
## map_nt> l1 |> map_int("b", .default = NA)
## [1] NA  2  3
## 
## map_nt> l1 |> map_int(2, .default = NA)
## [1] NA  2 NA
## 
## map_nt> # Supply multiple values to index deeply into a list
## map_nt> l2 <- list(
## map_nt+   list(num = 1:3,     letters[1:3]),
## map_nt+   list(num = 101:103, letters[4:6]),
## map_nt+   list()
## map_nt+ )
## 
## map_nt> l2 |> map(c(2, 2))
## [[1]]
## [1] "b"
## 
## [[2]]
## [1] "e"
## 
## [[3]]
## NULL
## 
## 
## map_nt> # Use a list to build an extractor that mixes numeric indices and names,
## map_nt> # and .default to provide a default value if the element does not exist
## map_nt> l2 |> map(list("num", 3))
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 103
## 
## [[3]]
## NULL
## 
## 
## map_nt> l2 |> map_int(list("num", 3), .default = NA)
## [1]   3 103  NA
## 
## map_nt> # Working with data frames
## map_nt> # Use map_lgl(), map_dbl(), etc to return a vector instead of a list:
## map_nt> mtcars |> map_dbl(sum)
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  642.900  198.000 7383.100 4694.000  115.090  102.952  571.160   14.000 
##       am     gear     carb 
##   13.000  118.000   90.000 
## 
## map_nt> # A more realistic example: split a data frame into pieces, fit a
## map_nt> # model to each piece, summarise and extract R^2
## map_nt> mtcars |>
## map_nt+   split(mtcars$cyl) |>
## map_nt+   map(\(df) lm(mpg ~ wt, data = df)) |>
## map_nt+   map(summary) |>
## map_nt+   map_dbl("r.squared")
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
example("map_lgl")
```

```
## 
## mp_lgl> # Compute normal distributions from an atomic vector
## mp_lgl> 1:10 |>
## mp_lgl+   map(rnorm, n = 10)
## [[1]]
##  [1] -0.2836132  2.6568074  1.3631730 -1.4051481 -0.2467745  1.1067976
##  [7]  1.2149678  2.1844942  1.6878956  1.0273419
## 
## [[2]]
##  [1] 0.96701944 1.29736442 3.19219589 1.32602585 0.07820912 2.01370405
##  [7] 1.83349951 1.24893070 1.13739624 0.71713276
## 
## [[3]]
##  [1] 3.887533 2.461630 1.466712 2.514817 1.939639 2.844973 2.763797 4.001944
##  [9] 3.133876 2.963580
## 
## [[4]]
##  [1] 3.329915 3.569649 3.137178 4.331993 5.836635 3.467067 4.707064 4.211002
##  [9] 3.024423 1.807016
## 
## [[5]]
##  [1] 4.905749 3.574360 4.587859 3.833664 2.994680 4.064135 7.796050 3.310797
##  [9] 7.731876 4.799193
## 
## [[6]]
##  [1] 7.524440 4.579106 5.479146 4.206892 6.084243 5.942987 7.187773 5.960689
##  [9] 6.482089 7.598063
## 
## [[7]]
##  [1] 5.623483 5.704766 8.080272 6.715599 8.124931 7.448712 6.823210 8.859447
##  [9] 6.324613 7.162155
## 
## [[8]]
##  [1] 6.748329 7.634665 9.119641 7.835459 7.836985 7.050710 5.528374 7.974703
##  [9] 7.893765 7.410074
## 
## [[9]]
##  [1]  8.967038  8.912313  7.644870  8.578961 10.009677  9.673820  8.815356
##  [8] 10.026462  9.522594 10.779734
## 
## [[10]]
##  [1] 10.044040  9.216241 12.045244  8.853402  8.611133 12.285599 10.488375
##  [8]  9.962997  9.625575  9.306273
## 
## 
## mp_lgl> # You can also use an anonymous function
## mp_lgl> 1:10 |>
## mp_lgl+   map(\(x) rnorm(10, x))
## [[1]]
##  [1]  1.6622680  1.2343585  1.7201468 -0.4698268  2.0447124  1.2546345
##  [7]  1.1542941  2.3519246  2.0386709  1.3690032
## 
## [[2]]
##  [1] 1.4818730 1.8964532 2.0856334 0.6647035 2.9659996 2.3049225 2.4455306
##  [8] 0.9710464 0.9681015 2.7400812
## 
## [[3]]
##  [1] 2.714451 3.258899 2.726852 3.052554 1.486252 3.212450 3.815129 3.077018
##  [9] 2.816925 2.994688
## 
## [[4]]
##  [1] 3.515365 2.229874 2.822720 3.140921 5.598486 3.172889 3.743047 3.480923
##  [9] 4.305299 3.289429
## 
## [[5]]
##  [1] 4.793411 4.143484 5.160702 6.221927 2.119281 4.394154 3.357734 4.493301
##  [9] 4.310077 6.150084
## 
## [[6]]
##  [1] 7.606418 7.534464 5.517033 7.489567 6.305109 6.741833 6.300748 5.497074
##  [9] 7.631885 6.760698
## 
## [[7]]
##  [1] 7.186560 6.684181 8.042505 6.299126 8.830740 6.037046 7.650846 8.878259
##  [9] 7.716836 7.236709
## 
## [[8]]
##  [1] 7.848365 9.188464 8.494540 6.472034 6.328761 7.218685 7.979790 8.671498
##  [9] 8.985920 8.295177
## 
## [[9]]
##  [1] 10.527793  9.880324  7.906025 10.611425 10.061821 10.299444  8.354311
##  [8]  8.055911  9.007506  9.373072
## 
## [[10]]
##  [1] 10.203120  9.393647  7.637710  8.825291  9.835573  9.020151 10.279821
##  [8] 10.383185 10.814615 10.458525
## 
## 
## mp_lgl> # Simplify output to a vector instead of a list by computing the mean of the distributions
## mp_lgl> 1:10 |>
## mp_lgl+   map(rnorm, n = 10) |>  # output a list
## mp_lgl+   map_dbl(mean)           # output an atomic vector
##  [1]  0.5425216  1.9309129  2.6621865  4.4913837  4.7329578  5.5574315
##  [7]  6.9007768  7.3394490  8.8562050 10.4869094
## 
## mp_lgl> # Using set_names() with character vectors is handy to keep track
## mp_lgl> # of the original inputs:
## mp_lgl> set_names(c("foo", "bar")) |> map_chr(paste0, ":suffix")
##          foo          bar 
## "foo:suffix" "bar:suffix" 
## 
## mp_lgl> # Working with lists
## mp_lgl> favorite_desserts <- list(Sophia = "banana bread", Eliott = "pancakes", Karina = "chocolate cake")
## 
## mp_lgl> favorite_desserts |> map_chr(\(food) paste(food, "rocks!"))
##                  Sophia                  Eliott                  Karina 
##   "banana bread rocks!"       "pancakes rocks!" "chocolate cake rocks!" 
## 
## mp_lgl> # Extract by name or position
## mp_lgl> # .default specifies value for elements that are missing or NULL
## mp_lgl> l1 <- list(list(a = 1L), list(a = NULL, b = 2L), list(b = 3L))
## 
## mp_lgl> l1 |> map("a", .default = "???")
## [[1]]
## [1] 1
## 
## [[2]]
## [1] "???"
## 
## [[3]]
## [1] "???"
## 
## 
## mp_lgl> l1 |> map_int("b", .default = NA)
## [1] NA  2  3
## 
## mp_lgl> l1 |> map_int(2, .default = NA)
## [1] NA  2 NA
## 
## mp_lgl> # Supply multiple values to index deeply into a list
## mp_lgl> l2 <- list(
## mp_lgl+   list(num = 1:3,     letters[1:3]),
## mp_lgl+   list(num = 101:103, letters[4:6]),
## mp_lgl+   list()
## mp_lgl+ )
## 
## mp_lgl> l2 |> map(c(2, 2))
## [[1]]
## [1] "b"
## 
## [[2]]
## [1] "e"
## 
## [[3]]
## NULL
## 
## 
## mp_lgl> # Use a list to build an extractor that mixes numeric indices and names,
## mp_lgl> # and .default to provide a default value if the element does not exist
## mp_lgl> l2 |> map(list("num", 3))
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 103
## 
## [[3]]
## NULL
## 
## 
## mp_lgl> l2 |> map_int(list("num", 3), .default = NA)
## [1]   3 103  NA
## 
## mp_lgl> # Working with data frames
## mp_lgl> # Use map_lgl(), map_dbl(), etc to return a vector instead of a list:
## mp_lgl> mtcars |> map_dbl(sum)
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  642.900  198.000 7383.100 4694.000  115.090  102.952  571.160   14.000 
##       am     gear     carb 
##   13.000  118.000   90.000 
## 
## mp_lgl> # A more realistic example: split a data frame into pieces, fit a
## mp_lgl> # model to each piece, summarise and extract R^2
## mp_lgl> mtcars |>
## mp_lgl+   split(mtcars$cyl) |>
## mp_lgl+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp_lgl+   map(summary) |>
## mp_lgl+   map_dbl("r.squared")
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

```r
example("map_vec")
```

```
## 
## map_vc> # Compute normal distributions from an atomic vector
## map_vc> 1:10 |>
## map_vc+   map(rnorm, n = 10)
## [[1]]
##  [1]  2.4599069  2.5213597 -0.1002531  1.9526032  0.1732285  0.9801524
##  [7]  0.9200166 -0.6587212  2.8279605  0.5989470
## 
## [[2]]
##  [1] 3.4540283 0.8746493 2.2748041 2.8427845 1.6433030 0.6826985 0.5943002
##  [8] 1.9580331 3.2177907 2.8015305
## 
## [[3]]
##  [1] 1.9747958 4.8579767 3.4460060 2.9127071 2.0717043 2.9183026 0.1704067
##  [8] 2.5629645 3.9668791 2.4767304
## 
## [[4]]
##  [1] 3.940488 4.597040 5.254537 5.740135 4.561433 3.832619 4.672470 4.608821
##  [9] 3.885714 2.432734
## 
## [[5]]
##  [1] 5.037306 6.873209 4.128429 2.656315 5.813804 6.009162 4.999999 4.727371
##  [9] 5.133681 5.505721
## 
## [[6]]
##  [1] 6.730848 6.184964 5.612018 4.920712 6.343530 4.375716 6.520623 6.198159
##  [9] 6.452804 6.118014
## 
## [[7]]
##  [1] 6.252666 5.985260 7.098621 7.363136 7.620991 7.765784 6.282675 8.377981
##  [9] 8.326855 7.468389
## 
## [[8]]
##  [1]  7.471596  9.270402 10.015911  7.597267  8.165466  7.496714  8.764991
##  [8]  6.523485  9.991172  8.552486
## 
## [[9]]
##  [1]  8.464237  8.623231  8.620518  7.900549  9.169226  8.960974  7.650570
##  [8] 11.334901 10.381650  8.024268
## 
## [[10]]
##  [1] 10.867874 11.653854 10.737884 10.676390 10.890095  9.792691 10.538040
##  [8] 11.625562  9.550454 12.273178
## 
## 
## map_vc> # You can also use an anonymous function
## map_vc> 1:10 |>
## map_vc+   map(\(x) rnorm(10, x))
## [[1]]
##  [1] -0.3064638  0.4443596  0.1970444  0.9077796  0.6326817 -0.3159403
##  [7]  1.8767462 -1.0705025  2.2470433  0.4154794
## 
## [[2]]
##  [1] 1.2356293 0.8114301 1.9371621 1.3661968 2.5113588 4.5600987 0.5858804
##  [8] 2.1261124 2.3465882 1.9659909
## 
## [[3]]
##  [1] 3.084011 5.574653 3.830378 3.815504 0.916224 3.615352 1.410303 1.851187
##  [9] 2.395351 1.486672
## 
## [[4]]
##  [1] 3.241263 2.982436 4.455728 4.786855 3.584499 3.265080 5.053554 1.996140
##  [9] 3.785093 3.042148
## 
## [[5]]
##  [1] 7.122778 3.706113 4.477136 5.244013 4.257974 6.035860 5.188386 5.573632
##  [9] 4.336993 4.970046
## 
## [[6]]
##  [1] 5.178417 5.030111 4.094966 5.169873 5.215348 5.206532 5.319880 5.916244
##  [9] 4.218224 6.785596
## 
## [[7]]
##  [1] 5.270173 6.882752 6.668282 8.469663 8.285452 8.806308 7.612341 5.671896
##  [9] 6.305977 8.206254
## 
## [[8]]
##  [1] 6.635247 8.753162 8.123379 6.985604 9.927604 7.040860 8.768288 8.256120
##  [9] 8.672341 7.634024
## 
## [[9]]
##  [1] 11.085755  8.662971  9.225233 10.177002  8.699498  9.272542  8.682806
##  [8]  9.379087  8.558648  8.703353
## 
## [[10]]
##  [1]  9.937142  9.726736 11.047941 10.386907 10.700821 10.366520  9.414713
##  [8] 10.037154  9.368780  9.647352
## 
## 
## map_vc> # Simplify output to a vector instead of a list by computing the mean of the distributions
## map_vc> 1:10 |>
## map_vc+   map(rnorm, n = 10) |>  # output a list
## map_vc+   map_dbl(mean)           # output an atomic vector
##  [1] 0.602731 2.031109 2.418262 3.900947 5.590981 5.597644 6.832588 7.470287
##  [9] 9.134321 9.981749
## 
## map_vc> # Using set_names() with character vectors is handy to keep track
## map_vc> # of the original inputs:
## map_vc> set_names(c("foo", "bar")) |> map_chr(paste0, ":suffix")
##          foo          bar 
## "foo:suffix" "bar:suffix" 
## 
## map_vc> # Working with lists
## map_vc> favorite_desserts <- list(Sophia = "banana bread", Eliott = "pancakes", Karina = "chocolate cake")
## 
## map_vc> favorite_desserts |> map_chr(\(food) paste(food, "rocks!"))
##                  Sophia                  Eliott                  Karina 
##   "banana bread rocks!"       "pancakes rocks!" "chocolate cake rocks!" 
## 
## map_vc> # Extract by name or position
## map_vc> # .default specifies value for elements that are missing or NULL
## map_vc> l1 <- list(list(a = 1L), list(a = NULL, b = 2L), list(b = 3L))
## 
## map_vc> l1 |> map("a", .default = "???")
## [[1]]
## [1] 1
## 
## [[2]]
## [1] "???"
## 
## [[3]]
## [1] "???"
## 
## 
## map_vc> l1 |> map_int("b", .default = NA)
## [1] NA  2  3
## 
## map_vc> l1 |> map_int(2, .default = NA)
## [1] NA  2 NA
## 
## map_vc> # Supply multiple values to index deeply into a list
## map_vc> l2 <- list(
## map_vc+   list(num = 1:3,     letters[1:3]),
## map_vc+   list(num = 101:103, letters[4:6]),
## map_vc+   list()
## map_vc+ )
## 
## map_vc> l2 |> map(c(2, 2))
## [[1]]
## [1] "b"
## 
## [[2]]
## [1] "e"
## 
## [[3]]
## NULL
## 
## 
## map_vc> # Use a list to build an extractor that mixes numeric indices and names,
## map_vc> # and .default to provide a default value if the element does not exist
## map_vc> l2 |> map(list("num", 3))
## [[1]]
## [1] 3
## 
## [[2]]
## [1] 103
## 
## [[3]]
## NULL
## 
## 
## map_vc> l2 |> map_int(list("num", 3), .default = NA)
## [1]   3 103  NA
## 
## map_vc> # Working with data frames
## map_vc> # Use map_lgl(), map_dbl(), etc to return a vector instead of a list:
## map_vc> mtcars |> map_dbl(sum)
##      mpg      cyl     disp       hp     drat       wt     qsec       vs 
##  642.900  198.000 7383.100 4694.000  115.090  102.952  571.160   14.000 
##       am     gear     carb 
##   13.000  118.000   90.000 
## 
## map_vc> # A more realistic example: split a data frame into pieces, fit a
## map_vc> # model to each piece, summarise and extract R^2
## map_vc> mtcars |>
## map_vc+   split(mtcars$cyl) |>
## map_vc+   map(\(df) lm(mpg ~ wt, data = df)) |>
## map_vc+   map(summary) |>
## map_vc+   map_dbl("r.squared")
##         4         6         8 
## 0.5086326 0.4645102 0.4229655
```

## map2 functions


```r
example("map2")
```

```
## 
## map2> x <- list(1, 1, 1)
## 
## map2> y <- list(10, 20, 30)
## 
## map2> map2(x, y, \(x, y) x + y)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## map2> # Or just
## map2> map2(x, y, `+`)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## map2> # Split into pieces, fit model to each piece, then predict
## map2> by_cyl <- mtcars |> split(mtcars$cyl)
## 
## map2> mods <- by_cyl |> map(\(df) lm(mpg ~ wt, data = df))
## 
## map2> map2(mods, by_cyl, predict)
## $`4`
##     Datsun 710      Merc 240D       Merc 230       Fiat 128    Honda Civic 
##       26.47010       21.55719       21.78307       27.14774       30.45125 
## Toyota Corolla  Toyota Corona      Fiat X1-9  Porsche 914-2   Lotus Europa 
##       29.20890       25.65128       28.64420       27.48656       31.02725 
##     Volvo 142E 
##       23.87247 
## 
## $`6`
##      Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
##       21.12497       20.41604       19.47080       18.78968       18.84528 
##      Merc 280C   Ferrari Dino 
##       18.84528       20.70795 
## 
## $`8`
##   Hornet Sportabout          Duster 360          Merc 450SE          Merc 450SL 
##            16.32604            16.04103            14.94481            15.69024 
##         Merc 450SLC  Cadillac Fleetwood Lincoln Continental   Chrysler Imperial 
##            15.58061            12.35773            11.97625            12.14945 
##    Dodge Challenger         AMC Javelin          Camaro Z28    Pontiac Firebird 
##            16.15065            16.33700            15.44907            15.43811 
##      Ford Pantera L       Maserati Bora 
##            16.91800            16.04103
```

```r
example("map2_chr")
```

```
## 
## mp2_ch> x <- list(1, 1, 1)
## 
## mp2_ch> y <- list(10, 20, 30)
## 
## mp2_ch> map2(x, y, \(x, y) x + y)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_ch> # Or just
## mp2_ch> map2(x, y, `+`)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_ch> # Split into pieces, fit model to each piece, then predict
## mp2_ch> by_cyl <- mtcars |> split(mtcars$cyl)
## 
## mp2_ch> mods <- by_cyl |> map(\(df) lm(mpg ~ wt, data = df))
## 
## mp2_ch> map2(mods, by_cyl, predict)
## $`4`
##     Datsun 710      Merc 240D       Merc 230       Fiat 128    Honda Civic 
##       26.47010       21.55719       21.78307       27.14774       30.45125 
## Toyota Corolla  Toyota Corona      Fiat X1-9  Porsche 914-2   Lotus Europa 
##       29.20890       25.65128       28.64420       27.48656       31.02725 
##     Volvo 142E 
##       23.87247 
## 
## $`6`
##      Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
##       21.12497       20.41604       19.47080       18.78968       18.84528 
##      Merc 280C   Ferrari Dino 
##       18.84528       20.70795 
## 
## $`8`
##   Hornet Sportabout          Duster 360          Merc 450SE          Merc 450SL 
##            16.32604            16.04103            14.94481            15.69024 
##         Merc 450SLC  Cadillac Fleetwood Lincoln Continental   Chrysler Imperial 
##            15.58061            12.35773            11.97625            12.14945 
##    Dodge Challenger         AMC Javelin          Camaro Z28    Pontiac Firebird 
##            16.15065            16.33700            15.44907            15.43811 
##      Ford Pantera L       Maserati Bora 
##            16.91800            16.04103
```

```r
example("map2_dbl")
```

```
## 
## mp2_db> x <- list(1, 1, 1)
## 
## mp2_db> y <- list(10, 20, 30)
## 
## mp2_db> map2(x, y, \(x, y) x + y)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_db> # Or just
## mp2_db> map2(x, y, `+`)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_db> # Split into pieces, fit model to each piece, then predict
## mp2_db> by_cyl <- mtcars |> split(mtcars$cyl)
## 
## mp2_db> mods <- by_cyl |> map(\(df) lm(mpg ~ wt, data = df))
## 
## mp2_db> map2(mods, by_cyl, predict)
## $`4`
##     Datsun 710      Merc 240D       Merc 230       Fiat 128    Honda Civic 
##       26.47010       21.55719       21.78307       27.14774       30.45125 
## Toyota Corolla  Toyota Corona      Fiat X1-9  Porsche 914-2   Lotus Europa 
##       29.20890       25.65128       28.64420       27.48656       31.02725 
##     Volvo 142E 
##       23.87247 
## 
## $`6`
##      Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
##       21.12497       20.41604       19.47080       18.78968       18.84528 
##      Merc 280C   Ferrari Dino 
##       18.84528       20.70795 
## 
## $`8`
##   Hornet Sportabout          Duster 360          Merc 450SE          Merc 450SL 
##            16.32604            16.04103            14.94481            15.69024 
##         Merc 450SLC  Cadillac Fleetwood Lincoln Continental   Chrysler Imperial 
##            15.58061            12.35773            11.97625            12.14945 
##    Dodge Challenger         AMC Javelin          Camaro Z28    Pontiac Firebird 
##            16.15065            16.33700            15.44907            15.43811 
##      Ford Pantera L       Maserati Bora 
##            16.91800            16.04103
```

```r
example("map2_df")
```

```
## 
## mp2_df> # map ---------------------------------------------
## mp2_df> # Was:
## mp2_df> mtcars |>
## mp2_df+   split(mtcars$cyl) |>
## mp2_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp2_df+   map_dfr(\(mod) as.data.frame(t(as.matrix(coef(mod)))))
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp2_df> # Now:
## mp2_df> mtcars |>
## mp2_df+   split(mtcars$cyl) |>
## mp2_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp2_df+   map(\(mod) as.data.frame(t(as.matrix(coef(mod))))) |>
## mp2_df+   list_rbind()
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp2_df> # map2 ---------------------------------------------
## mp2_df> 
## mp2_df> ex_fun <- function(arg1, arg2){
## mp2_df+   col <- arg1 + arg2
## mp2_df+   x <- as.data.frame(col)
## mp2_df+ }
## 
## mp2_df> arg1 <- 1:4
## 
## mp2_df> arg2 <- 10:13
## 
## mp2_df> # was
## mp2_df> map2_dfr(arg1, arg2, ex_fun)
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp2_df> # now
## mp2_df> map2(arg1, arg2, ex_fun) |> list_rbind()
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp2_df> # was
## mp2_df> map2_dfc(arg1, arg2, ex_fun)
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
## 
## mp2_df> # now
## mp2_df> map2(arg1, arg2, ex_fun) |> list_cbind()
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
```

```r
example("map2_dfc")
```

```
## 
## mp2_df> # map ---------------------------------------------
## mp2_df> # Was:
## mp2_df> mtcars |>
## mp2_df+   split(mtcars$cyl) |>
## mp2_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp2_df+   map_dfr(\(mod) as.data.frame(t(as.matrix(coef(mod)))))
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp2_df> # Now:
## mp2_df> mtcars |>
## mp2_df+   split(mtcars$cyl) |>
## mp2_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp2_df+   map(\(mod) as.data.frame(t(as.matrix(coef(mod))))) |>
## mp2_df+   list_rbind()
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp2_df> # map2 ---------------------------------------------
## mp2_df> 
## mp2_df> ex_fun <- function(arg1, arg2){
## mp2_df+   col <- arg1 + arg2
## mp2_df+   x <- as.data.frame(col)
## mp2_df+ }
## 
## mp2_df> arg1 <- 1:4
## 
## mp2_df> arg2 <- 10:13
## 
## mp2_df> # was
## mp2_df> map2_dfr(arg1, arg2, ex_fun)
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp2_df> # now
## mp2_df> map2(arg1, arg2, ex_fun) |> list_rbind()
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp2_df> # was
## mp2_df> map2_dfc(arg1, arg2, ex_fun)
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
## 
## mp2_df> # now
## mp2_df> map2(arg1, arg2, ex_fun) |> list_cbind()
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
```

```r
example("map2_dfr")
```

```
## 
## mp2_df> # map ---------------------------------------------
## mp2_df> # Was:
## mp2_df> mtcars |>
## mp2_df+   split(mtcars$cyl) |>
## mp2_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp2_df+   map_dfr(\(mod) as.data.frame(t(as.matrix(coef(mod)))))
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp2_df> # Now:
## mp2_df> mtcars |>
## mp2_df+   split(mtcars$cyl) |>
## mp2_df+   map(\(df) lm(mpg ~ wt, data = df)) |>
## mp2_df+   map(\(mod) as.data.frame(t(as.matrix(coef(mod))))) |>
## mp2_df+   list_rbind()
##   (Intercept)        wt
## 1    39.57120 -5.647025
## 2    28.40884 -2.780106
## 3    23.86803 -2.192438
## 
## mp2_df> # map2 ---------------------------------------------
## mp2_df> 
## mp2_df> ex_fun <- function(arg1, arg2){
## mp2_df+   col <- arg1 + arg2
## mp2_df+   x <- as.data.frame(col)
## mp2_df+ }
## 
## mp2_df> arg1 <- 1:4
## 
## mp2_df> arg2 <- 10:13
## 
## mp2_df> # was
## mp2_df> map2_dfr(arg1, arg2, ex_fun)
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp2_df> # now
## mp2_df> map2(arg1, arg2, ex_fun) |> list_rbind()
##   col
## 1  11
## 2  13
## 3  15
## 4  17
## 
## mp2_df> # was
## mp2_df> map2_dfc(arg1, arg2, ex_fun)
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
## 
## mp2_df> # now
## mp2_df> map2(arg1, arg2, ex_fun) |> list_cbind()
##   col...1 col...2 col...3 col...4
## 1      11      13      15      17
```

```r
example("map2_int")
```

```
## 
## mp2_nt> x <- list(1, 1, 1)
## 
## mp2_nt> y <- list(10, 20, 30)
## 
## mp2_nt> map2(x, y, \(x, y) x + y)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_nt> # Or just
## mp2_nt> map2(x, y, `+`)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_nt> # Split into pieces, fit model to each piece, then predict
## mp2_nt> by_cyl <- mtcars |> split(mtcars$cyl)
## 
## mp2_nt> mods <- by_cyl |> map(\(df) lm(mpg ~ wt, data = df))
## 
## mp2_nt> map2(mods, by_cyl, predict)
## $`4`
##     Datsun 710      Merc 240D       Merc 230       Fiat 128    Honda Civic 
##       26.47010       21.55719       21.78307       27.14774       30.45125 
## Toyota Corolla  Toyota Corona      Fiat X1-9  Porsche 914-2   Lotus Europa 
##       29.20890       25.65128       28.64420       27.48656       31.02725 
##     Volvo 142E 
##       23.87247 
## 
## $`6`
##      Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
##       21.12497       20.41604       19.47080       18.78968       18.84528 
##      Merc 280C   Ferrari Dino 
##       18.84528       20.70795 
## 
## $`8`
##   Hornet Sportabout          Duster 360          Merc 450SE          Merc 450SL 
##            16.32604            16.04103            14.94481            15.69024 
##         Merc 450SLC  Cadillac Fleetwood Lincoln Continental   Chrysler Imperial 
##            15.58061            12.35773            11.97625            12.14945 
##    Dodge Challenger         AMC Javelin          Camaro Z28    Pontiac Firebird 
##            16.15065            16.33700            15.44907            15.43811 
##      Ford Pantera L       Maserati Bora 
##            16.91800            16.04103
```

```r
example("map2_lgl")
```

```
## 
## mp2_lg> x <- list(1, 1, 1)
## 
## mp2_lg> y <- list(10, 20, 30)
## 
## mp2_lg> map2(x, y, \(x, y) x + y)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_lg> # Or just
## mp2_lg> map2(x, y, `+`)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_lg> # Split into pieces, fit model to each piece, then predict
## mp2_lg> by_cyl <- mtcars |> split(mtcars$cyl)
## 
## mp2_lg> mods <- by_cyl |> map(\(df) lm(mpg ~ wt, data = df))
## 
## mp2_lg> map2(mods, by_cyl, predict)
## $`4`
##     Datsun 710      Merc 240D       Merc 230       Fiat 128    Honda Civic 
##       26.47010       21.55719       21.78307       27.14774       30.45125 
## Toyota Corolla  Toyota Corona      Fiat X1-9  Porsche 914-2   Lotus Europa 
##       29.20890       25.65128       28.64420       27.48656       31.02725 
##     Volvo 142E 
##       23.87247 
## 
## $`6`
##      Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
##       21.12497       20.41604       19.47080       18.78968       18.84528 
##      Merc 280C   Ferrari Dino 
##       18.84528       20.70795 
## 
## $`8`
##   Hornet Sportabout          Duster 360          Merc 450SE          Merc 450SL 
##            16.32604            16.04103            14.94481            15.69024 
##         Merc 450SLC  Cadillac Fleetwood Lincoln Continental   Chrysler Imperial 
##            15.58061            12.35773            11.97625            12.14945 
##    Dodge Challenger         AMC Javelin          Camaro Z28    Pontiac Firebird 
##            16.15065            16.33700            15.44907            15.43811 
##      Ford Pantera L       Maserati Bora 
##            16.91800            16.04103
```

```r
example("map2_raw")
example("map2_vec")
```

```
## 
## mp2_vc> x <- list(1, 1, 1)
## 
## mp2_vc> y <- list(10, 20, 30)
## 
## mp2_vc> map2(x, y, \(x, y) x + y)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_vc> # Or just
## mp2_vc> map2(x, y, `+`)
## [[1]]
## [1] 11
## 
## [[2]]
## [1] 21
## 
## [[3]]
## [1] 31
## 
## 
## mp2_vc> # Split into pieces, fit model to each piece, then predict
## mp2_vc> by_cyl <- mtcars |> split(mtcars$cyl)
## 
## mp2_vc> mods <- by_cyl |> map(\(df) lm(mpg ~ wt, data = df))
## 
## mp2_vc> map2(mods, by_cyl, predict)
## $`4`
##     Datsun 710      Merc 240D       Merc 230       Fiat 128    Honda Civic 
##       26.47010       21.55719       21.78307       27.14774       30.45125 
## Toyota Corolla  Toyota Corona      Fiat X1-9  Porsche 914-2   Lotus Europa 
##       29.20890       25.65128       28.64420       27.48656       31.02725 
##     Volvo 142E 
##       23.87247 
## 
## $`6`
##      Mazda RX4  Mazda RX4 Wag Hornet 4 Drive        Valiant       Merc 280 
##       21.12497       20.41604       19.47080       18.78968       18.84528 
##      Merc 280C   Ferrari Dino 
##       18.84528       20.70795 
## 
## $`8`
##   Hornet Sportabout          Duster 360          Merc 450SE          Merc 450SL 
##            16.32604            16.04103            14.94481            15.69024 
##         Merc 450SLC  Cadillac Fleetwood Lincoln Continental   Chrysler Imperial 
##            15.58061            12.35773            11.97625            12.14945 
##    Dodge Challenger         AMC Javelin          Camaro Z28    Pontiac Firebird 
##            16.15065            16.33700            15.44907            15.43811 
##      Ford Pantera L       Maserati Bora 
##            16.91800            16.04103
```

<!--chapter:end:03_purrr.Rmd-->



<!--chapter:end:04_referencias.Rmd-->

