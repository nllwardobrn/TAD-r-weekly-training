<!-- Please edit README.Rmd - not README.md -->

# Week 03: Base R

# A note on style

Much has been made
(e.g. [here](http://varianceexplained.org/r/teach-tidyverse/) and
[here](https://www.r-bloggers.com/2019/12/why-i-dont-use-the-tidyverse/))
over the relative merits of the ‘tidyverse’ versus ‘base R’,
particularly when it comes to learning R. Here we briefly outline the
reasons for the debate and the view we have taken when creating this
training programme.

-   **What is the tidyverse?** The tidyverse is a suite of R packages
    commonly used for data analysis. These are principally authored and
    maintained by RStudio/Hadley Wickham, and include `ggplot2`,
    `dplyr`, `tidyr`, `tibble`, `purrr`, `stringr`, `forcats`and
    `readr`. Additionally, there is something of a ‘tidy’ brand,
    including a [code style guide](https://style.tidyverse.org/) and a
    notion of [‘tidy
    data’](https://r4ds.had.co.nz/tidy-data.html#tidy-data-1).
-   **What is base R?** Similarly, ‘base R’ is a a collection of
    packages which form part of the R source code. These include `base`,
    `datasets`, `utils`, `grDevices`, `graphics`, `stats`and `methods`.
    These are packages which come pre-packaged with R, so, e.g. you
    don’t need to use `libarary(utils)` before using the `head()`
    function.

Here are some of the main arguments for and against base R/the
tidyverse:

<table>
<colgroup>
<col style="width: 2%" />
<col style="width: 43%" />
<col style="width: 54%" />
</colgroup>
<thead>
<tr class="header">
<th></th>
<th>Pros</th>
<th>Cons</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Tidyverse</td>
<td><ul>
<li><p>Packages in the tidyverse have been written to work well with
each other and the pipe <code>%&gt;%</code>. This has given rise to
something of an ecosystem, with one benefit being that a little bit of
learning tends to go a long way.</p></li>
<li><p>Many tidyverse packages are just <em>very good</em>, particularly
<code>ggplot2</code> and <code>dplyr</code></p></li>
<li><p>The tidyverse encourages readable code, which pays dividends when
collaborating</p></li>
<li><p>There is lots of great documentation and community around the
tidyverse, with many places to get help if you need it.</p></li>
</ul></td>
<td><ul>
<li><p>The tidyverse is unashamedly ‘<a
href="https://www.tidyverse.org/">opinionated</a>’ about how you should
write code and think about data analysis. This can be a good thing, but
you may find it less flexible than other approaches for some
applications.</p></li>
<li><p>As tidyverse packages are not bundled with R, you may find you
run into issues with reproducibility, especially if working on the same
project over a long period of time (although <a
href="https://rstudio.github.io/renv/articles/renv.html">there are
ways</a> of dealing with this issue).</p></li>
</ul></td>
</tr>
<tr class="even">
<td>Base R</td>
<td><ul>
<li><p>Base packages are (<a
href="https://stackoverflow.com/questions/31132552/no-visible-global-function-definition-for-median">almost</a>)
always available</p></li>
<li><p>The developers of base packages strive for
backward-compatibility, meaning that code written in base is much more
likely to ‘just work’ in 5 years than code that depends on external
packages</p></li>
<li><p>Tidyverse code is sometimes criticised as being less ‘R-like’
than base. If initally you only use base, you are likely to get a better
feel for R as a language in your first few weeks of coding.</p></li>
<li><p>‘Pure’ base R can often lead to faster code, though this is by no
means always the case</p></li>
</ul></td>
<td><ul>
<li><p>Base R lacks many of the specialised tools that have made the
tidyverse popular</p></li>
<li><p>Compared to many tidyverse functions, many base functions have
surprising quirks, in part because of base’s devotion to
backward-compatibility</p></li>
<li><p>Base code is often harder to read. This is partially due to the
fact that most base functions have not been written to play nicely with
the (base) pipe <code>|&gt;</code>.</p></li>
<li><p>Base lacks a style-guide and largely lets you come up with your
own way of doing things. This can make collaboration difficult (even if
your collaborator is future you!)</p></li>
</ul></td>
</tr>
</tbody>
</table>

For this course, we have chosen to primarily teach the tidyverse way of
doing things. The primary reason for this is that in a business context
where collaboration is paramount, having uniformity in the way we do
things makes the work we do easier for everyone involved, and this is
something the tidyverse provides really well. However, this is not to
say that the ‘tidy’ approach is always better; on the contrary, you may
often find that base is preferable for the reasons given above, and in
such cases you should use your best judgement. However, for the purposes
of this course, we encourage use of the tidyverse where possible, and we
especially encourage you to follow the tidyverse style guide.

## Exercises

Stuff to compare tidyverse and base. Also introduce important base
syntax, e.g. the use of `$`, `[`, `[[`, `{`, `...` etc.
