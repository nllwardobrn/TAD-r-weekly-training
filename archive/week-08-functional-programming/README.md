<!-- Please edit README.Rmd - not README.md -->

# Week 08: Functional Programming

At this point you should be comfortable writing functions as part of
normal coding practice. This week’s questions focus on some of the
techniques and tools that are collectively known as *functional
programming*. While all of these questions can be completed using base R
functions, these questions expect you to use functions from the
`{purrr}` package, which have a more consistent API and are less likely
to trip you up with subtle idiosyncrasies.

``` r
# `{purrr}` is loaded as part of the tidyverse:
suppressPackageStartupMessages(library(tidyverse))
```

# Prerequisites

-   These questions assume some familiarity with `for`-loops and lists
    in R. If you’re not familiar, please read the [lists chapter of R
    for Data Science](https://r4ds.had.co.nz/vectors.html#lists) and the
    [loops chapter of Advanced
    R](https://adv-r.hadley.nz/control-flow.html#loops) before
    attempting these questions.

-   The [RStudio `{purrr}`
    cheatsheet](https://github.com/rstudio/cheatsheets/blob/main/purrr.pdf)
    is a useful resource to get quick summaries of how the most useful
    `{purrr}` functions work. This is well worth skimming over before
    attempting these questions - you’ll likely find it a useful
    reference later on.

-   The [Advanced R chapter on
    functionals](https://adv-r.hadley.nz/functionals.html) gives a great
    overview of function programming with `{purrr}`.

# Q1: For-loops and `map()`

This example will walk through the use of for-loops and hopefully
motivate the use of a very useful tool in R programming - the `map()`
function from the package `{purrr}`.

1.  The following code takes a vector of 5 random sentences, then loops
    over the vector to reverse the words in each sentence:

    ``` r
    # This is the 'input'
    my_sentences <- sentences[1:5]

    # Set up an output vector
    reversed_sentences <- character(5)

    # Reverse each sentence and assign to the output vector
    for (i in seq_along(my_sentences)) {
      sentence     <- my_sentences[i]
      words        <- strsplit(sentence, split = " ")[[1]]
      rev_sentence <- paste(rev(words), collapse = " ")

      reversed_sentences[i] <- rev_sentence
    }

    reversed_sentences
    ```

        #> [1] "planks. smooth the on slid canoe birch The" 
        #> [2] "background. blue dark the to sheet the Glue"
        #> [3] "well. a of depth the tell to easy It's"     
        #> [4] "dish. rare a is leg chicken a days These"   
        #> [5] "bowls. round in served often is Rice"

    Create a function `str_reverse()` to do the job of reversing the
    sentence. This should take two arguments:

    -   `x`: The string to reverse
    -   `split`: The character used to split and recombine the string.
        This should have a default value of `" "`.

    Using this function, refactor the above code. The for-loop should
    now look like this:

        for (i in seq_along(my_sentences)) {
          reversed_sentences[i] <- str_reverse(my_sentences[i])
        }

2.  Now, create a function `my_map()` which abstracts the above logic -
    i.e. does the hard work of creating and executing a for-loop for
    you. This function should have the following arguments:

    -   `x`: An object to iterate over
    -   `f`: A function to be applied to each element of `x`

    Refactor your answer to part 2 to use `my_map()`. Your code, not
    including the definition for `my_map()`, should now look something
    like this, and should produce exactly the same output as part 2:

        reversed_sentences <- my_map(my_sentences, str_reverse)

3.  Notice that `str_reverse()` has the following behaviour:

        str_reverse(c("hi", "there"), sep = "")

        #> [1] "ih"   "ereht"

    Add a `...` argument to `my_map()`, giving arguments to be ‘passed’
    to `f()`. Using this new functionality, try using `my_map()` and
    `str_reverse()` to reverse words rather than sentences - e.g.

        my_words <- c("one", "two", "three")

        # Changing the `sep` argument should mean that `str_reverse` internally
        # splits strings into vectors of characters rather than vectors of words
        my_map(my_words, str_reverse, sep = "")

        #> [1] "eno"   "owt"   "eerht"

# Note: why `map()`?

The function just created mimics the behaviour of `purrr::map()`. You
may wonder, why use `map()` if you can use a `for`-loop?

-   Once you’ve internalised the usage of `map()` (and other similar
    functions), you’ll find that 90% of the time it’s much easier to
    write code in this way. The only possible exception is when you need
    to recursively modify an object which exists outside of your loop.

-   While `for`-loops can be performant if written correctly, there are
    a few gotchas that you need to be aware of to avoid bottlenecks.
    `map()` tends to be fast by default.

-   `map()` encourages a more ‘functional’ style of programming, which
    is worth leaning into to become a really effective R programmer.

# 2. `map()`, `map2()`, `imap()` and `pmap()`

1.  Read the documentation for `purrr::map()` and use this function to
    simplify the following code:

    ``` r
    sample_sizes <- c(3, 5, 7)
    samples      <- vector("list", length = 3)

    for (i in 1:3) {
      samples[[i]] <- rnorm(sample_sizes[i])
    }

    samples
    ```

        #> [[1]]
        #> [1] 0.239317921 0.002048468 1.914491410
        #> 
        #> [[2]]
        #> [1] -0.562456474  1.512656273  0.880152818  0.009543478  0.711017532
        #> 
        #> [[3]]
        #> [1] -2.5743957 -1.3773594 -0.2875036 -0.2242932 -1.1685238  1.4182845 -2.2542368

2.  Read the documentation for `purrr::map2()` and use it to simplify
    the following code:

    ``` r
    sample_sizes  <- c(3, 5, 7)
    distributions <- list(rnorm, runif, rexp) 
    samples       <- vector("list", length = 3)

    for (i in 1:3) {
      samples[[i]] <- distributions[[i]](sample_sizes[[i]])
    }

    samples
    ```

        #> [[1]]
        #> [1] 0.796526024 0.002549439 1.587361657
        #> 
        #> [[2]]
        #> [1] 0.56938834 0.74501977 0.02795429 0.51892452 0.48140754
        #> 
        #> [[3]]
        #> [1] 0.2264951 0.7526558 0.4758782 0.6495749 0.3007080 2.2590945 0.9559354

    Hint: try writing a function to do the work of the code inside the
    `for`-loop `distributions[[i]](sample_sizes[[i]])`. This function
    should take two arguments: (1) the size of the sample to produce
    and (2) the function to use when producing the sample.

3.  Read the documentation for `imap()` and use it to simplify the
    following code:

    ``` r
    mtcars_list <- as.list(mtcars)[1:4]

    for (col in names(mtcars_list)) {
      mtcars_list[[col]] <- paste0(
        "Median `", col, "`: ", 
        median(mtcars_list[[col]])
      )  
    }

    mtcars_list
    ```

        #> $mpg
        #> [1] "Median `mpg`: 19.2"
        #> 
        #> $cyl
        #> [1] "Median `cyl`: 6"
        #> 
        #> $disp
        #> [1] "Median `disp`: 196.3"
        #> 
        #> $hp
        #> [1] "Median `hp`: 123"

4.  Read the documentation for `purrr::pmap()`. Using this function,
    produce 3 histogram plots, one for each row of the following example
    dataset. Each histogram should have a title that refrences the `id`
    and `distribution` columns.

    ``` r
    # Make random numbers reproducible
    set.seed(100)

    # Dataset containing samples for 3 distributions
    plot_data <- tibble(
      id           = sample(1:100, 3),
      distribution = c("normal", "uniform", "exponential"),
      values       = list(rnorm(1000), runif(1000), rexp(1000))
    )
    ```

    Hint: You can create a quick histogram using `qplot()`, e.g. 
    `qplot(rnorm(1000))`

# 3. Anonymous functions

A key characteristic of most `{purrr}` functions is that they tend to
take other functions as arguments. Creating a named, custom function
every time you use a tool like `map()` can feel cumbersome, so it’s
worth knowing how to create anonymous functions to make your code easier
to write and read. These questions outline a few ways of doing this.

1.  The following code investigates the relationship between diamond
    size and price by fitting a linear relationship between the two for
    each value of `cut`, then extracting the [coefficient of
    determination
    *R*<sup>2</sup>](https://en.wikipedia.org/wiki/Coefficient_of_determination)
    for each model.

    ``` r
    # 1. Create a split version of `diamonds`
    diamonds_split <- split(diamonds, diamonds$cut)

    # 2. Define a function to compute R^2
    size_to_price_relationship_strength <- function(data) {
      data %>% 
        lm(price ~ x * y * z, data = .) %>% 
        summary() %>% 
        .$r.squared
    }

    # 3. 
    diamonds_split %>% 
      map(size_to_price_relationship_strength)
    ```

        #> $Fair
        #> [1] 0.7576571
        #> 
        #> $Good
        #> [1] 0.8690485
        #> 
        #> $`Very Good`
        #> [1] 0.8768184
        #> 
        #> $Premium
        #> [1] 0.8668765
        #> 
        #> $Ideal
        #> [1] 0.8800288

    Rewrite step \#3 in the above code to use an anonymous function.
    This should be as simple as copy/pasting the definition for
    `size_to_price_relationship_strength()` into the `map()` call.

2.  Read the documentation for `purrr::map()` and repeat part 1 using
    formula syntax. Hint: you may also find the documentation for
    `rlang::as_function()` helpful.

3.  The pipe `%>%` can also be used to create compact anonymous
    functions. Read the examples in `` ?`%>%` `` and use repeat part 1
    using this syntax.

4.  Comment on the relative strengths/weaknesses of each technique:

    -   Creating named custom functions beforehand
    -   Defining anonymous function using R’s default `function` syntax
    -   `{purrr}`’s formula syntax
    -   `{magrittr}`’s pipe syntax

# 4. Other iterators from `{purrr}`

`{purrr}` has several other functions which are less frequently needed
but are still worth knowing about.

1.  Read the documentation for `accumulate()`. Use this function to
    create alternative implementations for `cumsum()` and `cumprod()`

2.  Read the documentation for `reduce()`. Use this function to create a
    more powerful version of `inner_join()`, `inner_join2()`, which
    should behave identically to `inner_join()`, but should allow the
    user to supply a list of dataframes as the `x` argument, in which
    case they will be recursively joined. Hint: You’ll need to think
    carefully about the default behaviour of the `suffix` argument.

Other functions from `{purrr}` are beyond the scope of these questions
but are still worth knowing about! These are a bit simpler and help you
solve common problems, so are worth knowing about! Some of the most
useful are `pluck()`, `chuck()`, `walk()`, `compact()`, `compose()`,
`set_names()`, `keep()` and `flatten()`.
