1.  Messin' with dataframes
================
Vivek Trivedi
24 December 2016

![Fig 1: R object type cheat sheet](fig_1.png)

Fig 1: R Object type cheat table

**Summary:**
------------

-   tidyverse is great.
-   `as_tibble(df)` : a much nicer summary of large dataframes.
-   `summary(num_obj/df_col)` : do this to get a sense of scale and dist.
-   `%>%` : pipe operator. Think "then". Plugs the first preceding R object into the first argument of a function succeeding it (usually on a different line).
-   `rename()` :
-   `filter(df, col {operator} value)` : filters dataframe for the condition in the second argument. Can add more arguments if multiple filter conditions needed. `{operator}` can be `==`, `>=`, `<`, `!=`, `%in%` and the like.
-   `x %in% y`: asks if x is contained in y. Not to be confused with `==` or `=`.
-   `select(df, col/condition)` : Ditto `filter()`, with the added ability to subset a dataframe with only certain columns/variables.

Title
-----

bla <http://rmarkdown.rstudio.com>.

**bold** *italics*

Plots:

![](1_messin_with_dataframes_files/figure-markdown_github/pressure-1.png)
