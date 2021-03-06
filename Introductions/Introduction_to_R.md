Introduction to R - Basic Features
================
Mr. Adams

# Welcome to R

Congratulations\! You are taking the first step toward becoming a data
scientist. R is a powerful statistical programming language, and this
document contains tips on the basics\!

## R can be used as a calculator

If you type a math equation into R, it will calculate the answer for
you. Here are some examples\!

``` r
2+3
```

    ## [1] 5

``` r
(4^2/2)
```

    ## [1] 8

## Naming

Naming things you create allows you to stay organized and to keep your
code clean. This action is akin to storing a value on a calculator.
However, we can store more than just values. Here are some examples.

### Naming calculations

We’ll start with what’s familiar - storing calculations. For the answer
to show up (In the data science biz, Print is the technical term for
show up), you need to run the name of the calculation by itself, which
is why you see a, b, and c on separate lines.

``` r
a <- 3+2

b <- a*4

c <- a+b

a 
```

    ## [1] 5

``` r
b
```

    ## [1] 20

``` r
c
```

    ## [1] 25

### Naming Vectors

A vector is a sequence of either numbers or characters. Here are some
examples.

#### A vector of numeric values

In this example, we create a vector of numeric values and then give it a
name.

``` r
c(1,2,3,4,5)
```

    ## [1] 1 2 3 4 5

``` r
numeric_vector <- c(1,2,3,4,5)

numeric_vector
```

    ## [1] 1 2 3 4 5

In this example, we create a vector of characters and then give it a
name. Remember, run the name of the vector by itself to see it.

``` r
c("apple", "b", "c", "d")
```

    ## [1] "apple" "b"     "c"     "d"

``` r
character_vector <- c("a", "b", "c", "d")

character_vector
```

    ## [1] "a" "b" "c" "d"

### Create and Name a data frame

A data frame will contain rows and columns. The columns will represent
the variables. The rows represent the observations.

Let’s make a data frame with 2 variables, age and favorite\_color, and 5
observations. The name of the data frame will be age\_col\_df, which is
short for age and favorite color data frame

``` r
age_col_df <- data.frame(age = c(16, 17, 18, 18, 19), 
                         favorite_color = c("red", "blue", "red", "green", "blue"))

age_col_df
```

    ##   age favorite_color
    ## 1  16            red
    ## 2  17           blue
    ## 3  18            red
    ## 4  18          green
    ## 5  19           blue

### Naming Visualizations

Future tutorials will show you how to create many different types of
visualizations. You can name visualizations too, which becomes helpful
when you have a lot of them, and you want to stay
    organized.

    ## ── Attaching packages ────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.3
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ───────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

Here I am going to create a data frame and then create a histogram of
those data.

``` r
example_data <- data.frame(variable = c(1,2,2,2,3,4,4,4,4,4,2,6,1,1))

histogram_example <- example_data %>%
                  ggplot(aes(x = variable)) +
                  geom_histogram(binwidth = 1, fill = "navy blue", color = "red")

histogram_example
```

![](Introduction_to_R_files/figure-gfm/naming%20visualizations-1.png)<!-- -->
