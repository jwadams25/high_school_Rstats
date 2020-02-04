Inference 2.0
================
Mr. Adams
2/4/2020

## Load Libraries

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ──────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(dslabs)
```

## 1\. Get the data

The name of the dataset is heights. Run the following code to view the
dataset.

``` r
view(heights)
```

What are the two variables in this dataset?

1.  
2.  
## 2\. Examine the data

What variable do I want to study?

What type of variable is it?

## 3\. Go down the Quantitative Path or the Categorical Path

\#\#Quantitatve Path If you selected a QUANTITATIVE VARIABLE use this
section, if not, skip below to find info on how to work with categorical
variables.

``` r
heights %>%
  ggplot(aes(x = height)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](KEY_Inference_with_R_2_files/figure-gfm/Histogram%20for%20quant%20variable-1.png)<!-- -->

What is the shape of the distribution of reported heights in your
sample?

``` r
height_sum_stats <- heights %>%
    summarize(avg = mean(height), med = median(height), 
              standard_dev = sd(height), sample_size = n(), iqr = IQR(height))

height_sum_stats
```

    ##        avg  med standard_dev sample_size iqr
    ## 1 68.32301 68.5     4.078617        1050   5

``` r
t.test(heights$height, conf.level = 0.95)
```

    ## 
    ##  One Sample t-test
    ## 
    ## data:  heights$height
    ## t = 542.81, df = 1049, p-value < 2.2e-16
    ## alternative hypothesis: true mean is not equal to 0
    ## 95 percent confidence interval:
    ##  68.07603 68.57000
    ## sample estimates:
    ## mean of x 
    ##  68.32301

Write a sentence intepreting the confidence interval you created.

## Categorical Path -

If you selected a CATEGORICAL VARIABLE use this section to create a bar
graph, generate stats, and calculate confidence Interval

``` r
heights %>%
  ggplot(aes(x = sex)) +
  geom_bar()
```

![](KEY_Inference_with_R_2_files/figure-gfm/Bar%20graph%20for%20categorical%20variable-1.png)<!-- -->

``` r
sex_count <- heights %>%
            group_by(sex) %>%
            summarize(count = n()) %>%
            mutate(prop = count/sum(count)) %>%
            arrange(desc(count))
sex_count
```

    ## # A tibble: 2 x 3
    ##   sex    count  prop
    ##   <fct>  <int> <dbl>
    ## 1 Male     812 0.773
    ## 2 Female   238 0.227

``` r
n <- sum(sex_count$count)
n
```

    ## [1] 1050

``` r
prop.test(812, 1050, conf.level = 0.95)
```

    ## 
    ##  1-sample proportions test with continuity correction
    ## 
    ## data:  812 out of 1050, null probability 0.5
    ## X-squared = 312.69, df = 1, p-value < 2.2e-16
    ## alternative hypothesis: true p is not equal to 0.5
    ## 95 percent confidence interval:
    ##  0.7465465 0.7980901
    ## sample estimates:
    ##         p 
    ## 0.7733333

Write a sentence interpreting the confidence interval you created.
