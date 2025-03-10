Exercises
================

1.  Create a new R script and name it after your AD username (the same
    one you use to log in on your laptop)

2.  Load the packages `dplyr`, `tidyr` and `ggplot2` at the top of your
    script

3.  Run `mtcars` in the console and look at the output. You’ll see the
    first column doesn’t have a header - this is because it gives *row
    names* for the dataset. Create a new version of the dataset,
    `mtcars2`, which gives the car names in a new column named `car`.
    This should be a `tibble`.

4.  1)  Add a new column which gives `TRUE`/`FALSE` depending on whether
        a car does more then 20 miles/gallon
    2)  Use `group_by()` and `summarise()` to create a new dataset
        giving the average miles/gallon for each combination of `cyl`
        and `gear`
    3)  `summarise()` is a function from `dplyr` which can be used to
        combine rows in a dataset. Run `?summarise` in the console to
        view the function’s documentation, and read through to discover
        what the `.groups` argument does. Using this information, review
        your answer to part b) and ensure the output is not ‘grouped’.
    4)  Bonus: Do the same, but include each *make* of car in the
        output. (Hint: look at the examples in the documentation for
        `tidyr`’s `separate()` function).

5.  1)  Create a scatter-plot of miles/gallon against weight (R4DS
        chapter 3, section 3 may be helpful here)
    2)  Colour the points to indicate the number of cylinders
    3)  Give the points a different shape if the car does more than 20
        miles/gallon
    4)  Give your plot a descriptive title and optional subtitle
        describing a key finding
    5)  Bonus: Fit a smooth line between the different points. (Hint:
        type `geom_` in the console and see what the autocomplete
        options are).

6.  Make sure your script is nicely commented. You might also want to
    try dividing it into [different
    sections](https://support.rstudio.com/hc/en-us/articles/200484568-Code-Folding-and-Sections-in-the-RStudio-IDE)
