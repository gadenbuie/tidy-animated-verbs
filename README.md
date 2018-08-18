
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Tidy Animated Verbs

Garrick Aden-Buie – [@grrrck](https://twitter.com/grrrck) –
[garrickadenbuie.com](https://www.garrickadenbuie.com)

[![Binder](http://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/gadenbuie/tidy-animated-verbs/master?urlpath=rstudio)
[![CC0](https://img.shields.io/badge/license-CC0-green.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

  - Mutating Joins: [`inner_join()`](#inner-join),
    [`left_join()`](#left-join), [`right_join()`](#right-join),
    [`full_join()`](#full-join)

  - Filtering Joins: [`semi_join()`](#semi-join),
    [`anti_join()`](#anti-join)

  - Set Operations: [`union()`](#union), [`union_all()`](#union-all),
    [`intersect()`](#intersect), [`setdiff()`](#setdiff)

  - Learn more about
    
      - [Relational Data](#relational-data)
      - [gganimate](#gganimate)

Please feel free to use these images for teaching or learning about
action verbs from the [tidyverse](https://tidyverse.org). You can
directly download the [original animations](images/) or static images in
[svg](images/static/svg/) or [png](images/static/png/) formats, or you
can use the [scripts](R/) to recreate the images locally.

Currently, the animations cover the [dplyr two-table
verbs](https://dplyr.tidyverse.org/articles/two-table.html) and I’d like
to expand the animations to include more verbs from the tidyverse.
[Suggestions are
welcome\!](https://github.com/gadenbuie/tidy-animated-verbs/issues)

## Mutating Joins

<img src="images/static/png/original-dfs.png" width="480px" />

``` r
x
#> # A tibble: 3 x 2
#>      id x    
#>   <int> <chr>
#> 1     1 x1   
#> 2     2 x2   
#> 3     3 x3
y
#> # A tibble: 3 x 2
#>      id y    
#>   <int> <chr>
#> 1     1 y1   
#> 2     2 y2   
#> 3     4 y4
```

### Inner Join

> All rows from `x` where there are matching values in `y`, and all
> columns from `x` and `y`.

![](images/inner-join.gif)

``` r
inner_join(x, y, by = "id")
#> # A tibble: 2 x 3
#>      id x     y    
#>   <int> <chr> <chr>
#> 1     1 x1    y1   
#> 2     2 x2    y2
```

### Left Join

> All rows from `x`, and all columns from `x` and `y`. Rows in `x` with
> no match in `y` will have `NA` values in the new columns.

![](images/left-join.gif)

``` r
left_join(x, y, by = "id")
#> # A tibble: 3 x 3
#>      id x     y    
#>   <int> <chr> <chr>
#> 1     1 x1    y1   
#> 2     2 x2    y2   
#> 3     3 x3    <NA>
```

### Left Join (Extra Rows in y)

> … If there are multiple matches between `x` and `y`, all combinations
> of the matches are returned.

![](images/left-join-extra.gif)

``` r
y_extra # has multiple rows with the key from `x`
#> # A tibble: 4 x 2
#>      id y    
#>   <dbl> <chr>
#> 1     1 y1   
#> 2     2 y2   
#> 3     4 y4   
#> 4     2 y5
left_join(x, y_extra, by = "id")
#> # A tibble: 4 x 3
#>      id x     y    
#>   <dbl> <chr> <chr>
#> 1     1 x1    y1   
#> 2     2 x2    y2   
#> 3     2 x2    y5   
#> 4     3 x3    <NA>
```

### Right Join

> All rows from y, and all columns from `x` and `y`. Rows in `y` with no
> match in `x` will have `NA` values in the new columns.

![](images/right-join.gif)

``` r
right_join(x, y, by = "id")
#> # A tibble: 3 x 3
#>      id x     y    
#>   <int> <chr> <chr>
#> 1     1 x1    y1   
#> 2     2 x2    y2   
#> 3     4 <NA>  y4
```

### Full Join

> All rows and all columns from both `x` and `y`. Where there are not
> matching values, returns `NA` for the one missing.

![](images/full-join.gif)

``` r
full_join(x, y, by = "id")
#> # A tibble: 4 x 3
#>      id x     y    
#>   <int> <chr> <chr>
#> 1     1 x1    y1   
#> 2     2 x2    y2   
#> 3     3 x3    <NA> 
#> 4     4 <NA>  y4
```

## Filtering Joins

### Semi Join

> All rows from `x` where there are matching values in `y`, keeping just
> columns from `x`.

![](images/semi-join.gif)

``` r
semi_join(x, y, by = "id")
#> # A tibble: 2 x 2
#>      id x    
#>   <int> <chr>
#> 1     1 x1   
#> 2     2 x2
```

### Anti Join

> All rows from `x` where there are not matching values in `y`, keeping
> just columns from `x`.

![](images/anti-join.gif)

``` r
anti_join(x, y, by = "id")
#> # A tibble: 1 x 2
#>      id x    
#>   <int> <chr>
#> 1     3 x3
```

## Set Operations

<img src="images/static/png/original-dfs-so.png" width="480px" />

``` r
x
#> # A tibble: 3 x 2
#>   x     y    
#>   <chr> <chr>
#> 1 x1    y1   
#> 2 x1    y2   
#> 3 x2    y1
y 
#> # A tibble: 2 x 2
#>   x     y    
#>   <chr> <chr>
#> 1 x1    y1   
#> 2 x2    y2
```

### Union

> All unique rows from `x` and `y`.

![](images/union.gif)

``` r
union(x, y)
#> # A tibble: 4 x 2
#>   x     y    
#>   <chr> <chr>
#> 1 x1    y1   
#> 2 x1    y2   
#> 3 x2    y2   
#> 4 x2    y1
```

### Union All

> All rows from `x` and `y`, keeping duplicates.

![](images/union_all.gif)

``` r
union_all(x, y)
#> # A tibble: 5 x 2
#>   x     y    
#>   <chr> <chr>
#> 1 x1    y1   
#> 2 x1    y2   
#> 3 x2    y1   
#> 4 x1    y1   
#> 5 x2    y2
```

### Intersection

> Common rows in both `x` and `y`, keeping just unique rows.

![](images/intersect.gif)

``` r
intersect(x, y)
#> # A tibble: 1 x 2
#>   x     y    
#>   <chr> <chr>
#> 1 x1    y1
```

### Set Difference

> All rows from `x` which are not also rows in `y`, keeping just unique
> rows.

![](images/setdiff.gif)

``` r
setdiff(x, y)
#> # A tibble: 2 x 2
#>   x     y    
#>   <chr> <chr>
#> 1 x1    y2   
#> 2 x2    y1
```

## Learn More

### Relational Data

The [Relational Data](http://r4ds.had.co.nz/relation-data.html) chapter
of the [R for Data Science](http://r4ds.had.co.nz/) book by Garrett
Grolemund and Hadley Wickham is an excellent resource for learning more
about relational data.

The [dplyr two-table verbs
vignette](https://dplyr.tidyverse.org/articles/two-table.html) and Jenny
Bryan’s [Cheatsheet for dplyr join
functions](http://stat545.com/bit001_dplyr-cheatsheet.html) are also
great resources.

### gganimate

The animations were made possible by the newly re-written
[gganimate](https://github.com/thomasp85/gganimate#README) package by
[Thomas Lin Pedersen](https://github.com/thomasp85) (original by [Dave
Robinson](https://github.com/dgrtwo)). The [package
readme](https://github.com/thomasp85/gganimate#README) provides an
excellent (and quick) introduction to gganimte.
