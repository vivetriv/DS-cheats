1.  Messin' with dataframes
================
Vivek Trivedi
24 December 2016

![Fig 1: R object type cheat sheet](fig_1.PNG)

Fig 1: R Object type cheat table

Summary:
========

-   tidyverse is great.
-   `str(obj)` : shows the structure of an R object.
-   `names(df)` : gives the column names (variables) of a dataframe. Similarly, `ncol()` is equaivalent to `length()`. `nrow()` and `dim()` give number of rows and row x col dimensions of the dataframe.
-   `as_tibble(df)` : a much nicer summary of large dataframes.
-   `summary(num_obj/df_col)` : do this to get a sense of scale and dist.
-   plot(dep\_var ~ indep\_var, df)
-   `%>%` : pipe operator. Think "then". Plugs the first preceding R object into the first argument of a function succeeding it (usually on a different line).
-   `rename()` :
-   `filter(df, col {operator} value)` : filters dataframe for the condition in the second argument. Can add more arguments if multiple filter conditions needed. `{operator}` can be `==`, `>=`, `<`, `!=`, `%in%` and the like.
-   `x %in% y`: asks if x is contained in y. Not to be confused with `==` or `=`.
-   `select(df, col/condition)` : Ditto `filter()`, with the added ability to subset a dataframe with only certain columns/variables.
-   `mutate(df, new var/col = func)` : defines and inserts new column into the dataframe.
-   Percentages and relative quantities hold much more meaning to humans than simply raw numbers.
-   `rep(thing, x)` : Replicates (repeats) thing x times.
-   Example:

``` r
ctib <- my_gap %>%
  filter(country == "Canada")
my_gap <- my_gap %>%
  mutate(tmp = rep(ctib$gdpPercap, nlevels(country)),
         gdpPercapRel = gdpPercap / tmp,
         tmp = NULL)
```

-   Put `eval = False` to display code chunks that do not need output or throw errors.

bla <http://rmarkdown.rstudio.com>.

**bold** *italics*
