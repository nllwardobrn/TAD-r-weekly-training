<!-- Please edit README.Rmd - not README.md -->

# Week 05: Visualisation

In week 1 we briefly touched on some data visualisation using the
`{ggplot2}` package. This problem sheet will provide a deeper dive into
plotting data with R, and hopefully bring you to a point where you’re
able to integrate visualisation into your day-to-day analysis with R.

# Resources

You should refer to the following resources when attempting the
problems:

-   [R for Data Science](https://r4ds.had.co.nz/data-visualisation.html)
    chapters 3 and 28 introduce the ggplot2 API and ‘grammar of
    graphics’

-   The [ggplot2 documentation](https://ggplot2.tidyverse.org/) is a
    good place to read about how individual functions work. There are
    also a number of vignettes (called ‘articles’ on the website) which
    cover individual topics and concepts.

-   The [ggplot2 book](https://ggplot2-book.org/) is an excellent and
    fairly comprehensive resource. If you want to know how to do
    something, check the book before stackoverflow :)

# Exercises

**Note**: Please make sure you complete all these questions using
`{ggplot2}`. R has other plotting functions available (e.g. `plot()`),
but these exercises are intended to introduce the ‘grammar of graphics’
approach. While other functions may get the job done, you’ll gain much
more valuable skills if you stick to `{ggplot2}` for these problems.

## Q1. Plotting `txhousing`

1.  Create a subset of `txhousing` which only includes counties (hint:
    try looking at all the distinct values of `city` first).

2.  Create a plot which shows how the median house price for each county
    changes over time. You should use `geom_line()` to do this, and
    counties should be differentiated using colour.

3.  Now add lines for every city in the `txhousing` dataset to your
    plot. These should be coloured light grey and should not be included
    in the colour legend.

    Hints:

    -   In your layer which includes every city, you’ll need to specify
        that each line should be grey *outside* of any calls to `aes()`

    -   You’ll also need to specify the `group` aesthetic so cities are
        plotted as individual lines

4.  Now, make your plot look nice by:

    -   Using a colourblind-friendly colour palette

    -   Using nice labels on the y-axis (see `scales::label_dollar()`)

    -   Changing the `x`, `y`, `colour` and `title` labels using
        `labs()`. Hint: If it’s self-evident, you can remove a label by
        setting it to `NULL`.

    -   Applying a nicer theme than the default `theme_grey()`

## Q2. Why ‘tidy’ data?

This question will demonstrate how `{ggplot2}` is designed to work with
‘tidy’ (i.e. ‘long’) data. If these are unfamiliar concepts, please read
[R for Data Science
Ch12](https://r4ds.had.co.nz/tidy-data.html#case-study) before
attempting these problems.

1.  Create a version of the `economics` dataset where `pce`, `pop`,
    `psavert`, `uempmed` and `unemploy` are rescaled to be between 0
    and 1. You can use this function to do the rescaling:

        rescale01 <- function(x) {
          rng <- range(x, na.rm = TRUE)
          (x - rng[1]) / (rng[2] - rng[1])
        }

2.  Now, create a plot of your rescaled dataset where the rescaled
    columns are all represented as different coloured lines, which
    should all be shown on the same panel.

3.  In part 2 you should have needed 5 separate calls to `geom_line()`.
    This part will demonstrate how you can simplify your plot code by
    reformatting your data.

    Create a new version of the `economics` dataset in the following
    steps:

    -   Use `pivot_longer()` to pivot the columns `pce`, `pop`,
        `psavert`, `uempmed` and `unemploy`. Column names should end up
        in a single column `variable` and values should end up in a
        single column `value`

    -   Use `group_by()`, `mutate()` and `rescale01()` to create a
        rescaled version of the `value` column. Call this new column
        `value01`. Don’t forget to `ungroup()`!

    -   The above steps should be done in a single call using the pipe
        `%>%`.

4.  Using your dataset from part 3, recreate the plot from part 2.

## Q3: Changing plot appearance

1.  Using your final plot from the previous question. Most of these can
    be completed using the `theme()` and `labs()` functions:

    -   Move the colour legend to the bottom of the plot
    -   Choose appropriate labels for the title, subtitle, legend and
        axes
    -   Increase the size of the title and centre the title text above
        the plot
    -   Remove the axis and ‘ticks’ for the y-axis
    -   Use a colourblind-friendly palette for the colour scale, and
        make the lines a bit thicker than the default
    -   Make the background of the whole plot`lightblue`

2.  The axis labels on the following plot are not very clear. Look at
    [this
    post](https://stackoverflow.com/questions/41568411/how-to-maintain-size-of-ggplot-with-long-labels/66169251#66169251)
    on stackoverflow and find a good solution to the problem:

        suppressPackageStartupMessages(library(tidyverse))

        mpg %>% 
          ggplot(aes(manufacturer)) +
          geom_bar()

    ![](/week-05-visualisation/README_files/figure-markdown_strict/unnamed-chunk-2-1.png)

## Q4: Using extensions

While `{ggplot2}` is a very powerful package, it doesn’t do everything.
Fortunately, many extensions exist, meaning the functionality you need
is probably out there somewhere. A list of some of the most popular
extensions can be found
[here](https://exts.ggplot2.tidyverse.org/gallery/).

1.  Use `{geomtextpath}` to add labels directly to the lines in the
    following plot. Remove the colour legend too as this will no longer
    be needed:

        ggplot(diamonds, aes(depth, colour = cut)) +
          geom_density() +
          xlim(55, 70)

        #> Warning: Removed 45 rows containing non-finite values (stat_density).

    ![](/week-05-visualisation/README_files/figure-markdown_strict/unnamed-chunk-3-1.png)

2.  Use `{ggpointdensity}` to improve the following plot:

        txhousing %>% 
          ggplot(aes(log(sales), log(listings))) +
          geom_point()

        #> Warning: Removed 1426 rows containing missing values (geom_point).

    ![](/week-05-visualisation/README_files/figure-markdown_strict/unnamed-chunk-4-1.png)

3.  Use `{ggthemes}` to change the appearances of the previous two
    plots. Try to find a theme that’s reasonable nice :)

4.  Use `{patchwork}` to combine the previous two plots into a single
    image.

## Q5. Some `{ggplot2}` subtleties

1.  We want a plot of the `diamonds` dataset showing `carat`/`price`. We
    want the points to be blue, but the following code isn’t working
    quite right:

        ggplot(diamonds, aes(carat, price, colour = "blue")) +
          geom_point()

    ![](/week-05-visualisation/README_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    Try the following three methods of fixing it:

    -   Putting `colour = "blue"` in a different place
    -   Adding a (appropriately configured) call to
        `scale_colour_manual()`
    -   Adding a call to `scale_colour_identity()`

    What use-cases do you think each method would be most suited to?

2.  The `group` aesthetic is usually not needed, but is good to be aware
    of. Try creating a line-plot of the following data. You should get
    an error - try to fix this using the `group` aesthetic. Why is this
    needed?

        df <- tibble(x = c("1", "2", "3", "4", "5"), y = 1:5)

3.  Re-create the plot from part 2 *without* using the `group`
    aesthetic. Do this by first applying an appropriate `mutate()` to
    the `x` variable in `df`.

4.  Why doesn’t the following code work? Find 3 ways to fix it.

        ggplot(mpg, aes(displ, manufacturer, colour = cyl)) +
          geom_jitter() +
          scale_colour_viridis_d()

    (Hint: try adjusting the scale, adjusting the `colour` aesthetic and
    adjusting the data before plotting).

5.  The following plot is not very space-efficient. Create a version of
    the plot with no empty panels.

        ggplot(mpg, aes(y = fl, fill = factor(year))) +
          geom_bar() +
          facet_grid(cyl ~ class) +
          theme(legend.position = "bottom")

    ![](/week-05-visualisation/README_files/figure-markdown_strict/unnamed-chunk-8-1.png)
