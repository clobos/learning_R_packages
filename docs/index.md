--- 
lang: "Pt-Br"
title: "Learning R packages"
author: "Cristian Villegas"
date: "2023-05-11"
site: bookdown::bookdown_site
documentclass: book
bibliography:
- book.bib
- packages.bib
description: |
  This is a minimal example of using the bookdown package to write a book.
  set in the _output.yml file.
  The HTML output format for this example is bookdown::gitbook,
link-citations: yes
github-repo: "rstudio/bookdown-demo"
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



## Alguns atalhos no Rstudio

Para considerar

  > Operador Pipe (%>%): Ctrl + Shift + M (Windows) ou Cmd + Shift + M (Mac).

  > Criar novos chunks: Ctrl + Alt + I (Windows) ou Cmd + Option + I (Mac).
  



