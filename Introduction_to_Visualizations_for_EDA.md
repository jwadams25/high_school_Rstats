Introduction to Visualizations for EDA
================
John Adams

An essential aspect of exploratory data analysis is visualizing your
data. It allows you to draw conclusions more easily. This document will
walk through a few visualizations you can create to see the story your
data
    tells.

## Load Tidyverse Library

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
library(readr)
```

## Load the Data

``` r
urlfile="https://raw.githubusercontent.com/jwadams25/high_school_Rstats/master/girls_bball_2018.csv"

RGVB_team <- read.csv(url(urlfile))
```

## Histogram

A histogram is a great way of seeing the distribution of quantitative
data. Let’s start by analyzing the distribution of the points scored by
the Rivers Girls Varsity Basketball team in each of their games during
the 2018-2019 season. The variable name in the data fram is Points\_For

``` r
points_hist <- RGVB_team %>%
            ggplot(aes(x = Points_For)) +
            geom_histogram()
points_hist
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/histogram%20distribution%20of%20points%20scored%20by%20Rivers-1.png)<!-- -->

Now let’s add some labels to your axes and give the histogram a title.
To start, I copied and pasted the code from above.

``` r
points_hist <- RGVB_team %>%
            ggplot(aes(x = Points_For)) +
            geom_histogram() +
            labs(x = "Points Scored", y = "Number of Games", title = "Distribution of Points Scored Per Game By Rivers Girls Basketball", subtitle = "2018-2019 Season")
points_hist
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/histogram%20distribution%20of%20points%20for%20with%20labels-1.png)<!-- -->

You’ll see that a message popped up that says in part “Pick a better
value with ‘binwidth’” Let’s make that change by adding in bindwith = 10
Reference data visualization principles to make an appropriate binwidth
for your analysis.

``` r
points_hist <- RGVB_team %>%
            ggplot(aes(x = Points_For)) +
            geom_histogram(binwidth = 5) +
            labs(x = "Points Scored", y = "Number of Games", title = "Distribution of Points Scored Per Game By Rivers Girls Basketball", subtitle = "2018-2019 Season")
points_hist
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/histogram%20distribution%20with%20new%20bins-1.png)<!-- -->

Now let’s change the color and outline of the bars.

``` r
points_hist <- RGVB_team %>%
            ggplot(aes(x = Points_For)) +
            geom_histogram(binwidth = 5, color = "black", fill = "red") +
            labs(x = "Points Scored", y = "Number of Games", title = "Distribution of Points Scored Per Game By Rivers Girls Basketball", subtitle = "2018-2019 Season")
points_hist
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/histogram%20distribution%20with%20style%20changes-1.png)<!-- -->

To finish let’s set the scales of the x and y axes, add in a line for
the median, and change the theme to clean things up a bit.

``` r
points_hist <- RGVB_team %>%
            ggplot(aes(x = Points_For)) +
            geom_histogram(binwidth = 5, color = "white", fill = "red") +
            labs(x = "Points Scored", y = "Number of Games", 
                 title = "Distribution of Points Scored Per Game By Rivers Girls Basketball", 
                 subtitle = "2018-2019 Season")  +
            scale_x_continuous(limits = c(0, 90),
                     breaks = seq(0, 90, 10))+
            scale_y_continuous(limits = c(0, 8),
                     breaks = seq(0, 8, 1)) +
        geom_vline(aes(xintercept = median(Points_For)), color = "black")  +
        theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(), panel.background = element_blank())
            
points_hist
```

    ## Warning: Removed 2 rows containing missing values (geom_bar).

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/histogram%20distribution%20and%20median%20line-1.png)<!-- -->

## Density Curve

There are times when a density curve will more clearly show the
distribution of quantitative data. Let’s say you are going through your
EDA and create a histogram first and realize a density curve would be a
better visualization: - copy and paste your code - change the name -
change geom\_histogram to geom\_density - delete binwidth = - take out
the scales code for the y-axis - change the y-axis label to y = "".

``` r
points_density <- RGVB_team %>%
            ggplot(aes(x = Points_For)) +
            geom_density(color = "white", fill = "red") +
            labs(x = "Points Scored", y = "", 
                 title = "Distribution of Points Scored Per Game By Rivers Girls Basketball", 
                 subtitle = "2018-2019 Season")  +
            scale_x_continuous(limits = c(0, 90),
                     breaks = seq(0, 90, 10)) +
        geom_vline(aes(xintercept = median(Points_For)), color = "black")  +
        theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(), panel.background = element_blank())
            
points_density 
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/Density%20Curves%20for%20distribution-1.png)<!-- -->

## Box Plots

Box plots are great for comparing distributions. Let’s use them to
compare the points scored during home games vs away games.

``` r
points_HVA_boxes <- RGVB_team %>%
            ggplot(aes(x = H_V_A , y = Points_For)) +
            geom_boxplot() +
            theme_classic()
points_HVA_boxes
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/Box%20Plots%20to%20Compare%20Distributions-1.png)<!-- -->

Before adding any style changes to this visualization, we want to make
it easier for our reader to make conclusions. Therefore, we want to
reorder these boxes from the greatest median to the least. To see this
change, look at how code after aes changed.

I also added in some code to change the style. Try to figure out what
each addition changed.

``` r
points_HVA_boxes <- RGVB_team %>%
            ggplot(aes(x = reorder(H_V_A, Points_For, .desc = TRUE), y = Points_For, color = H_V_A)) +
            geom_boxplot() +
            geom_jitter(alpha = 0.3) +
            scale_color_manual(values = c("black", "red")) +
            labs(title = "Distribution of Points Scored by Rivers Girls Basketball During Home and Away", subtitle = "2018-2019 Season", x = "", y = "Points Scored", color = "") +
            theme_classic()
points_HVA_boxes
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/Box%20Plots%20to%20Compare%20Distributions%20reorder-1.png)<!-- -->

Sometimes box plots are easier to read when they are stacked on top of
one another. Use coord\_flip to do that. Let’s also add in labels while
we are here. You will do that using the labs code like you did when
making the histograms and density curves.

``` r
flip_points_HVA_boxes <- RGVB_team %>%
            ggplot(aes(x = reorder(H_V_A, Points_For, .desc = TRUE), y = Points_For, color = H_V_A)) +
            geom_boxplot() +
            geom_jitter(alpha = 0.3) +
            scale_color_manual(values = c("black", "red")) +
            labs(title = "Distribution of Points Scored by Rivers Girls Basketball During Home and Away", subtitle = "2018-2019 Season", x = "", y = "Points Scored", color = "") +
            theme_classic() +
            coord_flip()
flip_points_HVA_boxes
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/Box%20Plots%20to%20Compare%20Distributions%20add%20titles%20and%20flip-1.png)<!-- -->

## Bar Plots

We’ll use the bar plot to visualize categorical data. In this first
example, we will look at the marginal distribution of result. In other
words, the heights of the bars will represent the number of games they
won and lost.

``` r
wl_bars <- RGVB_team %>%
          ggplot(aes(x = Result)) +
          geom_bar()
wl_bars
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/Bar%20plot%20basic-1.png)<!-- -->

Because we would most likely want to highlight the largest bar, let’s
put that one first. To do this, we have to change the order of the bars
to go from greatest to least.

Let’s start by making a table using the skills we learned in previous
activities to see the number of games for each result. After, we will
use that data frame to make our bar plot.

In this example, I have not made changes to the style. Knowing how style
changes were made to previous visualizations, try to make similar
changes to this one. If you are confused, start by changing the title
and axes labels.

``` r
wl_count <- RGVB_team %>%
            group_by(Result) %>%
            summarize(count = n()) %>%
            arrange(desc(count))
wl_count
```

    ## # A tibble: 2 x 2
    ##   Result count
    ##   <fct>  <int>
    ## 1 "W "      15
    ## 2 L          5

``` r
wl_bar_order <- wl_count %>%
          ggplot(aes(x = reorder(Result, -count), y = count)) +
          geom_bar(stat = 'identity')
wl_bar_order
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/Bar%20plot%20ordering%20bars-1.png)<!-- -->

## 100% Stacked Bar Graph

When you want to examine conditional distributions, a good option is a
100% stacked bar graph. For this example, we want to look at the
proportion of home games and away games were won i.e., You will be able
to answer the question, “Did they win a higher percentage of games at
home or away?” It’s important to remember that you use this when
studying two categorical variables.

``` r
wl_venue <- RGVB_team %>%
            ggplot(aes(H_V_A, fill = Result)) +
            geom_bar(position = "fill") +
            labs(x = "", y = "Proportion", fill = "") +
        theme(panel.grid.major = element_blank(), panel.grid.minor = element_blank(), panel.background = element_blank())
wl_venue
```

![](Introduction_to_Visualizations_for_EDA_files/figure-gfm/100%20stacked-1.png)<!-- -->
