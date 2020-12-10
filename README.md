<!-- README.md is generated from README.Rmd. Please edit that file -->
OpenCaseStudies: Influence of Multicollinearity on Measured Impact of Right-to-Carry Gun Laws Part 1
====================================================================================================

<!-- badges: start -->
[![R build
status](https://github.com/opencasestudies/ocs-bp-RTC-wrangling/workflows/R-CMD-check/badge.svg)](https://github.com/opencasestudies/ocs-bp-RTC-wrangling/actions)
<!-- badges: end -->

### Important links

-   HTML:
    <a href="https://www.opencasestudies.org/ocs-bp-RTC-wrangling" class="uri">https://www.opencasestudies.org/ocs-bp-RTC-wrangling</a>
-   GitHub:
    <a href="https://github.com/opencasestudies/ocs-bp-RTC-wrangling" class="uri">https://github.com/opencasestudies/ocs-bp-RTC-wrangling</a>
-   Bloomberg American Health Initiative:
    <a href="https://americanhealth.jhu.edu/open-case-studies" class="uri">https://americanhealth.jhu.edu/open-case-studies</a>

### Disclaimer

The purpose of the [Open Case
Studies](https://opencasestudies.github.io) project is **to demonstrate
the use of various data science methods, tools, and software in the
context of messy, real-world data**. A given case study does not cover
all aspects of the research process, is not claiming to be the most
appropriate way to analyze a given dataset, and should not be used in
the context of making policy decisions without external consultation
from scientific experts.

### License

This case study is part of the
[OpenCaseStudies](https://opencasestudies.github.io) project. This work
is licensed under the Creative Commons Attribution-NonCommercial 3.0
([CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/us/))
United States License.

### Citation

To cite this case study:

Wright, Carrie and Ontiveros, Michael and Jager, Leah and Taub, Margaret
and Hicks, Stephanie. (2020).
<a href="https://github.com/opencasestudies/ocs-bp-RTC-wrangling" class="uri">https://github.com/opencasestudies/ocs-bp-RTC-wrangling</a>.
Influence of Multicollinearity on Measured Impact of Right-to-Carry Gun
Laws Part 1 (Version v1.0.0).

### Acknowledgments

We would like to acknowledge [Daniel
Webster](https://www.jhsph.edu/faculty/directory/profile/739/daniel-webster)
for assisting in framing the major direction of the case study.

We would also like to acknowledge the [Bloomberg American Health
Initiative](https://americanhealth.jhu.edu/) for funding this work.

### Title

Influence of Multicollinearity on Measured Impact of Right-to-Carry Gun
Laws Part 1

### Motivation

The influence of the implementation of less restrictive right-to-carry
gun laws on violent crime is a historically controversial topic. One
reason for the controversy, is concern that some earlier reports
examining this topic may have used methods that were inappropriate.

One of the major concerns is that an earlier report included multiple
demographic variables that were collinear with one another. This
resulted in different a very coefficient estimate for right-to-carry gun
law adoption than other reports that did not include collinear
variables. This phenomenon is called
[multicollinearity](https://en.wikipedia.org/wiki/Multicollinearity),
and it can result in aberrant findings for particular explanatory
variables, despite not altering the overall predictive power of a model.

In the next case study we perform a simplified analysis similar to those
of these reports on this topic to explore the influence of
multicollinearity on coefficient estimate stability. We however, do not
recreate the previous analyses.

The reports that we use as a guide for our analysis are:

1.  John J. Donohue et al., Right‐to‐Carry Laws and Violent Crime: A
    Comprehensive Assessment Using Panel Data and a State‐Level
    Synthetic Control Analysis. *Journal of Empirical Legal Studies*,
    16,2 (2019).

2.  David B. Mustard & John Lott. Crime, Deterrence, and Right-to-Carry
    Concealed Handguns. *Coase-Sandor Institute for Law & Economics*
    Working Paper No. 41, (1996).

### Motivating question

1.  How does the inclusion of different numbers of age groups influence
    the results of an analysis of right to carry laws and violence
    rates?

### Data

In this case study, we perform analyses similar to those in
<a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue, et al.</a>
article and the
<a href="https://chicagounbound.uchicago.edu/cgi/viewcontent.cgi?article=1150&amp;context=law_and_economics" target="_blank">Lott and Mustard</a>
article, however **we do not try to recreate them**, instead we perform
simplified analyses to allow us to focus on multicollinearity.

Therefore we use a subset of the **explanatory variables** used by each
article including:

1.  Data about state demographics in terms of population compositions
    for age, sex, race, as well as overall population values from the US
    Census Bureau:

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>Data</th>
<th>Link</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>years 1977 to 1979</strong></td>
<td><a href="https://www2.census.gov/programs-surveys/popest/tables/1900-1980/state/asrh/">link</a></td>
</tr>
<tr class="even">
<td><strong>years 1980 to 1989</strong></td>
<td><a href="https://www2.census.gov/programs-surveys/popest/tables/1980-1990/counties/asrh/">link</a> * county data was used for this decade which also has state information</td>
</tr>
<tr class="odd">
<td><strong>years 1990 to 1999</strong></td>
<td><a href="https://www2.census.gov/programs-surveys/popest/tables/1990-2000/state/asrh/">link</a></td>
</tr>
<tr class="even">
<td><strong>years 2000 to 2010</strong></td>
<td><a href="https://www.census.gov/data/datasets/time-series/demo/popest/intercensal-2000-2010-state.html">link</a> <br> <a href="https://www2.census.gov/programs-surveys/popest/technical-documentation/file-layouts/2000-2010/intercensal/state/st-est00int-alldata.pdf" target="_blank">technical documentation</a></td>
</tr>
</tbody>
</table>

Six demographic variables are created for the
<a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue, et al.</a>-like
analysis and 36 were created for the
<a href="https://chicagounbound.uchicago.edu/cgi/viewcontent.cgi?article=1150&amp;context=law_and_economics" target="_blank">Lott and Mustard</a>-like
analysis.

To use this data, we also need [Federal Information Processing Standard
(FIPS) state
codes](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard_state_code){target="\_blank",
to identify what demographic data corresponds to what state. This is
also available from the
<a href="https://www.census.gov/geographies/reference-files/2014/demo/popest/2014-geocodes-state.html" target="_blank">US Census Bureau</a>.

1.  Police staffing data, which was downloaded from the
    <a href="https://crime-data-explorer.fr.cloud.gov/downloads-and-docs" target="_blank">Federal Bureau of Investigation</a>

2.  Unemployment data, which was downloaded from the
    <a href="https://data.bls.gov/cgi-bin/dsrv?la" target="_blank">U.S. Bureau of Labor Statistics</a>.

3.  Poverty data, extracted from Table 21 from this
    <a href="https://www.census.gov/data/tables/time-series/demo/income-poverty/historical-poverty-people.html" target="_blank">US Census Bureau Poverty Data</a>

4.  Right-to-carry law data, which is available in a table in the
    <a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue paper</a>

Finally our outcome of interest is violent crime rates. The violent
crime data was downloaded from the
<a href="https://www.ucrdatatool.gov/Search/Crime/State/StatebyState.cfm" target="_blank">FBI uniform crime reporting system</a>

#### Learning Objectives

The skills, methods, and concepts that students will be familiar with by
the end of this case study are:

<u>**Data Science Learning Objectives:**</u>

1.  Data import of many different file types with special cases
    (`readr`, `readxl`, `pdftools`)  
2.  Joining data from multiple sources (`dplyr`)  
3.  Working with character strings (`stringr`)  
4.  Data comparisons (`dplyr` and `janitor`)  
5.  Reshaping data into different formats (`tidyr`)

#### Data import

In this case study particularly covers many different types of data
import. We cover data import using the Tidyverse `readr` package to
import the various types of text files, the Tidyverse `readxl` package
to import excel files, and the Tidyverse `pdftools` package to import a
PDF file. We demonstrate special cases where header rows need to be
skipped, or column data types need to be specified. We also use the the
`list.files()` base function together with the `map()` function of the
Tidyverse `purrr` package to efficiently perform data importation of
multiple files with one command.

#### Data wrangling

This case study is covers many details about wrangling data from excel
files with unusual header structures and with similar data in multiple
tables within the same file. This involves using the `stringr` package
to split, subset, detect, extract, and modify patterns of text. This
also involves using the `tidyr` package to change data shape and using
the `dplyr` package to summarize, select, filter, modify, and join data.
They case study also covers using various `map_*()` functions of the
`purrr` package to perform functions across tibbles within lists and the
`across()` function of the `dplyr` package to perform functions across
columns of an individual tibble. This case study provides especially
diverse material about data wrangling.

See [this case
study](https://github.com/opencasestudies/ocs-bp-RTC-analysis) for part
2 which includes the following sections:

#### Data Visualization

The next [case
study](https://github.com/opencasestudies/ocs-bp-RTC-analysis)
demonstrates how to make correlation plots and scatter plots with error
bars. We also show how to add formulas and arrows to plots. The
instruction about data visualization assumes that students have some
familiarity with `ggplot2`.

### Analysis

The next [case
study](https://github.com/opencasestudies/ocs-bp-RTC-analysis) covers
balanced panel regression model data analysis with fixed effects. In
doing so we provide an introduction to longitudinal analysis in general,
as well as use of the `plm` package. We also show how to calculate
[Variance inflation factor
(VIF)](https://en.wikipedia.org/wiki/Variance_inflation_factor) values
using the `car` package to quantify the severity of multicollinearity.
As another assessment of multicollinearity, we demonstrate how to
perform simulations to evaluate the stability of coefficient estimates.

### Other notes and resources

<a href="https://www.tidyverse.org/" target="_blank">Tidyverse</a>

The articles used to motivate this case study are:  
<a href="https://chicagounbound.uchicago.edu/cgi/viewcontent.cgi?article=1150&amp;context=law_and_economics" target="_blank">Mustard and Lott</a>  
<a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue, et al.</a>  
<a href="https://en.wikipedia.org/wiki/More_Guns,_Less_Crime" target="_blank">See here for a list of studies on this topic</a>

<u>**Packages used in this case study:** </u>

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>Package</th>
<th>Use in this case study</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://github.com/jennybc/here_here" target="_blank">here</a></td>
<td>to easily load and save data</td>
</tr>
<tr class="even">
<td><a href="https://readxl.tidyverse.org/" target="_blank">readxl</a></td>
<td>to import the data in the excel files</td>
</tr>
<tr class="odd">
<td><a href="https://readr.tidyverse.org/" target="_blank">readr</a></td>
<td>to import the CSV file data</td>
</tr>
<tr class="even">
<td><a href="https://github.com/ropensci/pdftools" target="_blank">pdftools</a></td>
<td>to import data from a pdf file</td>
</tr>
<tr class="odd">
<td><a href="https://dplyr.tidyverse.org/" target="_blank">dplyr</a></td>
<td>to arrange/filter/select/compare specific subsets of the data</td>
</tr>
<tr class="even">
<td><a href="https://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.html" target="_blank">magrittr</a></td>
<td>to use the compound assignment pipe operator <code>%&lt;&gt;%</code></td>
</tr>
<tr class="odd">
<td><a href="https://tidyr.tidyverse.org/" target="_blank">tidyr</a></td>
<td>to rearrange data in wide and long formats</td>
</tr>
<tr class="even">
<td><a href="https://stringr.tidyverse.org/articles/stringr.html" target="_blank">stringr</a></td>
<td>to manipulate the character strings within the data</td>
</tr>
<tr class="odd">
<td><a href="https://purrr.tidyverse.org/" target="_blank">purrr</a></td>
<td>to import the data in all the different excel and csv files efficiently</td>
</tr>
<tr class="even">
<td><a href="https://forcats.tidyverse.org/" target="_blank">forcats</a></td>
<td>to allow for reordering of factors in plots</td>
</tr>
<tr class="odd">
<td><a href="https://tibble.tidyverse.org/" target="_blank">tibble</a></td>
<td>to create data objects that we can manipulate with <code>dplyr</code>/<code>stringr</code>/<code>tidyr</code>/<code>purrr</code></td>
</tr>
</tbody>
</table>

#### For users

There is a [`Makefile`](Makefile) in this folder that allows you to type
`make` to knit the case study contained in the `index.Rmd` to
`index.html` and it will also knit the [`README.Rmd`](README.Rmd) to a
markdown file (`README.md`).

#### For instructors

Instructors can skip the Data Import and Data Wrangling and start at the
Data Exploration section, which gives a brief introduction of the data
before the Data Analysis section, by skipping to this [this case
study](https://github.com/opencasestudies/ocs-bp-RTC-analysis).

#### Target audience

This case study is intended for individuals or classes with some
familiarity with R programming.

Part 2 of this case study is intended for individuals or classes with
some familiarity with regression. See this
<a href="https://opencasestudies.github.io/ocs-bp-diet/" target="_blank">case study</a>
for an introduction to regression.

#### Suggested homework

Ask students to import and wrangle similar datasets to those used here.
