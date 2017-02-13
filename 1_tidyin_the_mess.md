Tidyin' the Mess
================
Vivek Trivedi
6 February 2017

Summary
=======

-   R is a data-aware language. It instrinsically knows the structure of data.
-   Tidy data is (essentially) a single data frame with columns representing variables and rows representing observations. It looks not very pretty to us, but R loves tidy data data aggregation for graphics.
-   Remember, the first argument is most of these functions is the initial object to be manipulated. Therefore, use the pipe operator if needed.
-   Be vary of commands that change the arrangement/order of rows.

------------------------------------------------------------------------

### Operations for similar data frames

-   `read_csv(file.path(path/to/file))`: Imports csv files into the workspace. Other `read` variants [also exist](https://github.com/tidyverse/readr). `read_csv(chrDataFrame, trim_ws = TRUE, skip = 1)` can extract comma separated data from a character type object into a tibble/data frame.

``` r
# library(tidyverse)
males <- read_csv(file.path("males.csv"))
females <- read_csv(file.path("females.csv"))
```

-   Use `message = FALSE` in RMarkdown code chunk headers to hide code execution messages from being displayed.
-   `bind_rows(df1, df2...)`: Combines multiple dataframes into a single one by putting the names of the dataframes in a separate column/variable.

``` r
data_untidy <- bind_rows(males, females)
data_untidy
```

    ## # A tibble: 6 × 5
    ##   Gender                       Film   Elf Hobbit   Man
    ##    <chr>                      <chr> <int>  <int> <int>
    ## 1   Male The Fellowship Of The Ring   971   3644  1995
    ## 2   Male             The Two Towers   513   2463  3589
    ## 3   Male     The Return Of The King   510   2673  2459
    ## 4 Female The Fellowship Of The Ring  1229     14     0
    ## 5 Female             The Two Towers   331      0   401
    ## 6 Female     The Return Of The King   183      2   268

-   `gather(untidy_df, key = 'new_col/var', value = 'numbers', old_col1, old_col2...)`: Turns the title of column/s in a dataframe into an observation in each row/sample.

``` r
data_tidy <- gather(data_untidy, key = 'Species', value = 'Words', Elf, Hobbit, Man)
data_tidy
```

    ## # A tibble: 18 × 4
    ##    Gender                       Film Species Words
    ##     <chr>                      <chr>   <chr> <int>
    ## 1    Male The Fellowship Of The Ring     Elf   971
    ## 2    Male             The Two Towers     Elf   513
    ## 3    Male     The Return Of The King     Elf   510
    ## 4  Female The Fellowship Of The Ring     Elf  1229
    ## 5  Female             The Two Towers     Elf   331
    ## 6  Female     The Return Of The King     Elf   183
    ## 7    Male The Fellowship Of The Ring  Hobbit  3644
    ## 8    Male             The Two Towers  Hobbit  2463
    ## 9    Male     The Return Of The King  Hobbit  2673
    ## 10 Female The Fellowship Of The Ring  Hobbit    14
    ## 11 Female             The Two Towers  Hobbit     0
    ## 12 Female     The Return Of The King  Hobbit     2
    ## 13   Male The Fellowship Of The Ring     Man  1995
    ## 14   Male             The Two Towers     Man  3589
    ## 15   Male     The Return Of The King     Man  2459
    ## 16 Female The Fellowship Of The Ring     Man     0
    ## 17 Female             The Two Towers     Man   401
    ## 18 Female     The Return Of The King     Man   268

-   `write_csv(tidy_df, file.path("desired/path/to/csv"))`: Write dataframes to given location as a .csv file.

``` r
write_csv(data_tidy, file.path("lotr_tidy.csv"))
```

-   `unite(tib_df, combined_var, var1, var2)`: Combines the data and names of two columns into a single column.

``` r
data_mashed <- data_tidy %>%
  unite(Race_Gender, Species, Gender)

data_mashed
```

    ## # A tibble: 18 × 3
    ##      Race_Gender                       Film Words
    ## *          <chr>                      <chr> <int>
    ## 1       Elf_Male The Fellowship Of The Ring   971
    ## 2       Elf_Male             The Two Towers   513
    ## 3       Elf_Male     The Return Of The King   510
    ## 4     Elf_Female The Fellowship Of The Ring  1229
    ## 5     Elf_Female             The Two Towers   331
    ## 6     Elf_Female     The Return Of The King   183
    ## 7    Hobbit_Male The Fellowship Of The Ring  3644
    ## 8    Hobbit_Male             The Two Towers  2463
    ## 9    Hobbit_Male     The Return Of The King  2673
    ## 10 Hobbit_Female The Fellowship Of The Ring    14
    ## 11 Hobbit_Female             The Two Towers     0
    ## 12 Hobbit_Female     The Return Of The King     2
    ## 13      Man_Male The Fellowship Of The Ring  1995
    ## 14      Man_Male             The Two Towers  3589
    ## 15      Man_Male     The Return Of The King  2459
    ## 16    Man_Female The Fellowship Of The Ring     0
    ## 17    Man_Female             The Two Towers   401
    ## 18    Man_Female     The Return Of The King   268

-   `spread(tidy_df, key = new_col/vars, value = old_col/vars)` : Opposite of `gather()`. Turns the observations of the `key` variable into column/variable. The result is much nicer to human eyeballs.

``` r
data_mashed %>%
  spread(key = Race_Gender, value = Words) %>%
  print(width = Inf)
```

    ## # A tibble: 3 × 7
    ##                         Film Elf_Female Elf_Male Hobbit_Female Hobbit_Male
    ## *                      <chr>      <int>    <int>         <int>       <int>
    ## 1 The Fellowship Of The Ring       1229      971            14        3644
    ## 2     The Return Of The King        183      510             2        2673
    ## 3             The Two Towers        331      513             0        2463
    ##   Man_Female Man_Male
    ## *      <int>    <int>
    ## 1          0     1995
    ## 2        268     2459
    ## 3        401     3589

------------------------------------------------------------------------

### Operations for (kind of) disparate data frames

-   **Caution:** Join functions love changing the order of the original data. Arrange, filter and ascertain data order appropriately before performing analysis.
-   **Also, caution:** Order of data frames in arguments matters. A lot.
-   `read_csv(chrDataFrame, trim_ws = TRUE, skip = 1)`: Extracts comma separated data from a character type object into a tibble/data frame. The `trim_ws` argument is used to exclude the preceding and succeeding whitespace around the data. `skip` is used to point out how many lines from the top are not part of the data.

``` r
superheroes <- "
name,   alignment,  gender, publisher   
Magneto,   bad,    male,   Marvel
Storm,  good,   female, Marvel
Mystique,   bad,    female,    Marvel
Batman,   good,   male,   DC
Joker,  bad,    male,   DC
Catwoman,   bad,    female, DC
Hellboy,    good,   male,   Dark Horse Comics
" # Data separated by one tab. The next action works with other types of whitespaces too (such as space).

superheroes <- read_csv(superheroes, trim_ws = TRUE, skip = 1)

publishers <- "
  publisher, yr_founded
         DC,       1934
     Marvel,       1939
      Image,       1992
"
publishers <- read_csv(publishers, trim_ws = TRUE, skip = 1)

superheroes
```

    ## # A tibble: 7 × 4
    ##       name alignment gender         publisher
    ##      <chr>     <chr>  <chr>             <chr>
    ## 1  Magneto       bad   male            Marvel
    ## 2    Storm      good female            Marvel
    ## 3 Mystique       bad female            Marvel
    ## 4   Batman      good   male                DC
    ## 5    Joker       bad   male                DC
    ## 6 Catwoman       bad female                DC
    ## 7  Hellboy      good   male Dark Horse Comics

``` r
publishers
```

    ## # A tibble: 3 × 2
    ##   publisher yr_founded
    ##       <chr>      <int>
    ## 1        DC       1934
    ## 2    Marvel       1939
    ## 3     Image       1992

**Going from most mutating to most filtering...**

-   `full_join(df1, df2)` : df1 union df2. Returns all rows and columns for df1 and df2, but with `NA` for missing values.

``` r
full_join(publishers, superheroes)
```

    ## Joining, by = "publisher"

    ## # A tibble: 8 × 5
    ##           publisher yr_founded     name alignment gender
    ##               <chr>      <int>    <chr>     <chr>  <chr>
    ## 1                DC       1934   Batman      good   male
    ## 2                DC       1934    Joker       bad   male
    ## 3                DC       1934 Catwoman       bad female
    ## 4            Marvel       1939  Magneto       bad   male
    ## 5            Marvel       1939    Storm      good female
    ## 6            Marvel       1939 Mystique       bad female
    ## 7             Image       1992     <NA>      <NA>   <NA>
    ## 8 Dark Horse Comics         NA  Hellboy      good   male

-   `left_join(df1, df2)`: Basically, df1 union df2, except only returns the rows from df1. Multiple matches produce multiple rows. Puts down `NA` values for non-matching values.

``` r
left_join(publishers, superheroes)
```

    ## Joining, by = "publisher"

    ## # A tibble: 7 × 5
    ##   publisher yr_founded     name alignment gender
    ##       <chr>      <int>    <chr>     <chr>  <chr>
    ## 1        DC       1934   Batman      good   male
    ## 2        DC       1934    Joker       bad   male
    ## 3        DC       1934 Catwoman       bad female
    ## 4    Marvel       1939  Magneto       bad   male
    ## 5    Marvel       1939    Storm      good female
    ## 6    Marvel       1939 Mystique       bad female
    ## 7     Image       1992     <NA>      <NA>   <NA>

Notice that the Hellboy row from `superheroes` does not appear in the result, as it has not matching value in df1, `publishers`.

-   `inner_join(df1, df2)`: Kind of like df1 intersect df2. Returns all rows from df1 where there are matching values in df2, and all columns from df1 and df2.

``` r
inner_join(superheroes, publishers)
```

    ## Joining, by = "publisher"

    ## # A tibble: 6 × 5
    ##       name alignment gender publisher yr_founded
    ##      <chr>     <chr>  <chr>     <chr>      <int>
    ## 1  Magneto       bad   male    Marvel       1939
    ## 2    Storm      good female    Marvel       1939
    ## 3 Mystique       bad female    Marvel       1939
    ## 4   Batman      good   male        DC       1934
    ## 5    Joker       bad   male        DC       1934
    ## 6 Catwoman       bad female        DC       1934

Notice how Hellboy is not present in the dataframe, as the "publisher" value of that row is not present in the `publishers` data frame. Also, the publisher "Image" is not present in the result due to the same reason.

-   `semi_join(df1, df2)`: Sort of `inner_join()`, except only returns rows from df1. Filters and selects df1 rows with matching values in df2.

``` r
semi_join(publishers, superheroes)
```

    ## Joining, by = "publisher"

    ## # A tibble: 2 × 2
    ##   publisher yr_founded
    ##       <chr>      <int>
    ## 1    Marvel       1939
    ## 2        DC       1934

-   `anti_join(df1, df2)` : df1 - (df1 intersect df2), but only with the columns of df1. If a row in df1 has a matching value in df2, it will be filtered out of the results. Opposite of `semi_join()`.

``` r
anti_join(superheroes, publishers)
```

    ## Joining, by = "publisher"

    ## # A tibble: 1 × 4
    ##      name alignment gender         publisher
    ##     <chr>     <chr>  <chr>             <chr>
    ## 1 Hellboy      good   male Dark Horse Comics
