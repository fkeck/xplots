
<!-- README.md is generated from README.Rmd. Please edit that file -->
xplots
=====

<!-- badges: start -->
<!-- badges: end -->
The goal of xplots is to easily save multiple plots stored in list columns of tibbles.

Installation
------------

You can install the released version of xplots from GitHub with:

``` r
devtools::install_github("fkeck/xplots")
```

Example
-------

``` r
library(tidyverse)

iris_plots <- iris %>% 
  as_tibble() %>% 
  group_by(Species) %>% 
  nest() %>% 
  mutate(plots = map2(data, Species,
                      ~ ggplot(.x, aes(Sepal.Length, Sepal.Width)) +
                          geom_point() +
                          labs(title = .y)
    )
  )

iris_plots
#> # A tibble: 3 x 3
#>   Species    data              plots 
#>   <fct>      <list>            <list>
#> 1 setosa     <tibble [50 × 4]> <gg>  
#> 2 versicolor <tibble [50 × 4]> <gg>  
#> 3 virginica  <tibble [50 × 4]> <gg>
```

``` r
xplots::save_plots(iris_plots, plots, files = "myplot.pdf")

# If Ghostscript is available you can add bookmarks to the pdf.
xplots::save_plots(iris_plots, plots, files = "myplot_bkm.pdf",
                  bookmarks = vars(Species))
```
