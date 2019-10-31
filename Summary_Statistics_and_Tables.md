Summary Statistics and Tables
================
Mr. Adams

# Using R to Generate Statistics and Create Tables

R is an incredibly powerful tool to help you quickly and efficiently
make sense of a dataset. This document will show you how to generate
summary statistics and build
    tables.

## Load Tidyverse and Skimr

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(skimr)
```

    ## 
    ## Attaching package: 'skimr'

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

## Summarizing Quantitative Data

When analyzing quantitative data, it’s important to examine the shape,
center, and spread of that distribution. Here are two ways you can do
that.

### Using the summarize function.

#### Summarize one variable

The summarize function is a great tool to use when you want to look at
summary statistics for one variable or when grouping variables by a
category. We will do both.

Here we generate the summary statistics for one variable. More
specifically, we will have R calculate the mean, median, standard
deviation, and IQR for the price variable.

``` r
price_summary_stats <- diamonds %>% 
          summarize(avg = mean(price), med = median(price), 
                    standard_dev = sd(price), 
                    iqr = IQR(price))
price_summary_stats
```

    ## # A tibble: 1 x 4
    ##     avg   med standard_dev   iqr
    ##   <dbl> <dbl>        <dbl> <dbl>
    ## 1 3933.  2401        3989. 4374.

#### Using Group\_by and Summarize Functions

Here we will group the diamonds by color and then take the summary
statistics for each color.

``` r
price_color_ss <- diamonds %>%
    group_by(color) %>%
    summarize(avg = mean(price), med = median(price), standard_dev = sd(price), 
                    iqr = IQR(price))

price_color_ss
```

    ## # A tibble: 7 x 5
    ##   color   avg   med standard_dev   iqr
    ##   <ord> <dbl> <dbl>        <dbl> <dbl>
    ## 1 D     3170. 1838         3357. 3302.
    ## 2 E     3077. 1739         3344. 3121 
    ## 3 F     3725. 2344.        3785. 3886.
    ## 4 G     3999. 2242         4051. 5117 
    ## 5 H     4487. 3460         4216. 4996.
    ## 6 I     5092. 3730         4722. 6081.
    ## 7 J     5324. 4234         4438. 5834.

#### Select and Skimr

Let’s say, when you look at a dataset, you are not quite sure exactly
where to start. A good strategy is to look at the summary statistics for
a few variables. By examing a few variables and making some initial
conclusions, you can ask further questions and dive deeper into your
analysis.

For this example, let’s use the diamonds dataset again, but let’s look
at the depth, carat, and price variables.

``` r
diamonds %>%
  select(carat, depth, price) %>%
  skim()
```

    ## Skim summary statistics
    ##  n obs: 53940 
    ##  n variables: 3 
    ## 
    ## ── Variable type:integer ───────────────────────────────────────────────────────────────────────────────
    ##  variable missing complete     n   mean      sd  p0 p25  p50     p75  p100
    ##     price       0    53940 53940 3932.8 3989.44 326 950 2401 5324.25 18823
    ##      hist
    ##  ▇▃▂▁▁▁▁▁
    ## 
    ## ── Variable type:numeric ───────────────────────────────────────────────────────────────────────────────
    ##  variable missing complete     n  mean   sd   p0  p25  p50   p75  p100
    ##     carat       0    53940 53940  0.8  0.47  0.2  0.4  0.7  1.04  5.01
    ##     depth       0    53940 53940 61.75 1.43 43   61   61.8 62.5  79   
    ##      hist
    ##  ▇▅▁▁▁▁▁▁
    ##  ▁▁▁▃▇▁▁▁

## Analyzing Categorical Variables

### Making a Table to Study One Categorical Variable

When we have a categorical variable, we first want to see how many
observations are in each category and what proportion of those
observations fall in each category.

For example, when using the diamonds dataset, we could ask, “How many
and what proportion of diamonds in the dataset are each respective cut?”
To answer this question, we will use the group\_by function again as
well as the summarize function. The last line will arrange the groups in
descending order to make it easier to draw conclusions. I have also
named the table cut\_count\_prop.

``` r
cut_count_prop <- diamonds %>%
            group_by(cut) %>%
            summarize(count = n()) %>%
            mutate(prop = count/sum(count)) %>%
            arrange(desc(count))
cut_count_prop
```

    ## # A tibble: 5 x 3
    ##   cut       count   prop
    ##   <ord>     <int>  <dbl>
    ## 1 Ideal     21551 0.400 
    ## 2 Premium   13791 0.256 
    ## 3 Very Good 12082 0.224 
    ## 4 Good       4906 0.0910
    ## 5 Fair       1610 0.0298

### Making Two-Way Tables to Study Conditional Distributions

This two-way table shows the conditional distributions with totals.

``` r
totals <- diamonds %>%
            group_by(color, cut) %>%
            summarize(num = n()) %>%
            spread(color, num)
totals
```

    ## # A tibble: 5 x 8
    ##   cut           D     E     F     G     H     I     J
    ##   <ord>     <int> <int> <int> <int> <int> <int> <int>
    ## 1 Fair        163   224   312   314   303   175   119
    ## 2 Good        662   933   909   871   702   522   307
    ## 3 Very Good  1513  2400  2164  2299  1824  1204   678
    ## 4 Premium    1603  2337  2331  2924  2360  1428   808
    ## 5 Ideal      2834  3903  3826  4884  3115  2093   896

Here we will create two-way tables with proportions.

Each number in the table created with the following code represents the
proportion of diamonds that are a specific color AND a specific cut. For
example, 0.003 is the proportion of the TOTAL diamonds that are the
color D and the cut FAIR.

``` r
prop.table(table(diamonds$cut, diamonds$color)) %>% 
  round(4)
```

    ##            
    ##                  D      E      F      G      H      I      J
    ##   Fair      0.0030 0.0042 0.0058 0.0058 0.0056 0.0032 0.0022
    ##   Good      0.0123 0.0173 0.0169 0.0161 0.0130 0.0097 0.0057
    ##   Very Good 0.0280 0.0445 0.0401 0.0426 0.0338 0.0223 0.0126
    ##   Premium   0.0297 0.0433 0.0432 0.0542 0.0438 0.0265 0.0150
    ##   Ideal     0.0525 0.0724 0.0709 0.0905 0.0577 0.0388 0.0166

You can also create a table to look at what proportion of a certain
color are the various cuts. For example, the proportion of D colored
diamonds that are Ideal cut is 0.41.

``` r
prop.table(table(diamonds$cut, diamonds$color), margin = 2) %>% 
  round(4)
```

    ##            
    ##                  D      E      F      G      H      I      J
    ##   Fair      0.0241 0.0229 0.0327 0.0278 0.0365 0.0323 0.0424
    ##   Good      0.0977 0.0952 0.0953 0.0771 0.0845 0.0963 0.1093
    ##   Very Good 0.2233 0.2450 0.2268 0.2036 0.2197 0.2221 0.2415
    ##   Premium   0.2366 0.2385 0.2443 0.2589 0.2842 0.2634 0.2877
    ##   Ideal     0.4183 0.3984 0.4010 0.4325 0.3751 0.3860 0.3191
