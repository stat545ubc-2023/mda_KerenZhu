Mini Data Analysis Milestone 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(dplyr)
library(ggplot2)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  *What is the density of building registered year?*
2.  *How are the number of storeys distributed among buildings?*
3.  *How is the distribution of separate gas, hydro, and water meter
    services among buildings?*
4.  *What is the relationship between window types and year built?*
    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

**Q1 What is the density of building registered year?**

**Preparation**

``` r
#overview
class(apt_buildings$year_registered)
```

    ## [1] "numeric"

``` r
summary(apt_buildings$year_registered)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    2017    2017    2017    2017    2017    2020      89

``` r
summary(apt_buildings$year_built) 
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##    1805    1955    1962    1962    1970    2019      13

``` r
#clean
Q1cleaned <-apt_buildings %>%
  select(id, year_built, year_registered) %>%
  #unsure NA represents missing value / not registered, so I filtered out all NA in year_registered
  filter(!is.na(year_registered)&!is.na(year_built)) 
print(Q1cleaned)
```

    ## # A tibble: 3,364 × 3
    ##       id year_built year_registered
    ##    <dbl>      <dbl>           <dbl>
    ##  1 10359       1967            2017
    ##  2 10360       1970            2017
    ##  3 10361       1927            2017
    ##  4 10362       1959            2017
    ##  5 10363       1943            2017
    ##  6 10365       1959            2017
    ##  7 10366       1971            2017
    ##  8 10367       1969            2017
    ##  9 10368       1972            2017
    ## 10 10369       1945            2018
    ## # ℹ 3,354 more rows

**Q1.1 Summarizing**

*Task*: Create a categorical variable with 3 or more groups from an
existing numerical variable. You can use this new variable in the other
tasks (Task 3). numeric variable: year_registered categorical variable:
year_reg_group with 3 groups (year before 2017, year 2017, year after
2017)

*Comment*: This task is helpful. While designing the question at the
first stage for MDA1, I thought year_registered would be like year_built
and a density plot is applicable. Indeed, the original data set
apt_buildings gives year_registered dbl data type. However, through
*“explore year_registered”*, I found the range and variety of
year_registered is not as wide as year_built. While year_built ranges
from 1805-2019, buildings mainly registered in 2017. Thereby, I set a
new categorical variable named year_reg_group with 3 groups (year before
2017, year 2017, year after 2017) with an assumption that 2017 is an
identical year for building registration and management. This task thus
can serve as a preparation for future tasks to explore the City of
Toronto’s advancement in managing apartment buildings, which is aligned
with the primary goal of bringing up *Question 1*.

``` r
#create a new variable
answer1.1.1 <-Q1cleaned%>%
  mutate(year_reg_group=factor(case_when(
      year_registered < 2017 ~ "year before 2017",
      year_registered == 2017 ~ "year 2017",
      year_registered > 2017 ~ "year after 2017"),
    levels=c('year before 2017','year 2017','year after 2017')))%>%
    select(-year_registered)
head(answer1.1.1)
```

    ## # A tibble: 6 × 3
    ##      id year_built year_reg_group
    ##   <dbl>      <dbl> <fct>         
    ## 1 10359       1967 year 2017     
    ## 2 10360       1970 year 2017     
    ## 3 10361       1927 year 2017     
    ## 4 10362       1959 year 2017     
    ## 5 10363       1943 year 2017     
    ## 6 10365       1959 year 2017

**Q1.2 Graphing**

*Task*: Make a graph where it makes sense to customize the alpha
transparency (Task 7).

*Comment*: This task is not helpful. Alpha transparency makes sense to
*answer1.2*, using geom_point. This is because the darker color presents
higher density. Although *answer1.2* is helpful to understand Question
1, I think geom_dotplot is a better option to visualize the data.
However, transparency doesn’t make sense to this dotplot. This dotplot
is better because the length differences in the graph is easier to be
distinguished by human eyes than nuances in transparencies.
*answer1.2_better* makes it easier to compare the amount of buildings in
their built years within and among registration year groups.

``` r
#Graph for the Task:
answer1.1.2 <- answer1.1.1%>%
  ggplot(aes(year_reg_group, year_built))+
  geom_point(alpha=0.1) + #customize the transparency
  scale_x_discrete(drop = FALSE) + # Don't drop the year before 2017
  labs(title="The Dot Plot of Built Year by Registration Year Cateogory from 1805 to 2020 ", x= "Registration Year Cateogory", y="Built Year")
  
print(answer1.1.2)
```

![](stat545_mda2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#A better option
answer1.1.2_better <- answer1.1.1%>%
  ggplot(aes(year_reg_group, year_built))+
  geom_dotplot(binaxis = "y", stackdir = "center", binwidth=0.7) +
  scale_x_discrete(drop = FALSE) + # Don't drop the year before 2017
  labs(title="The Dot Plot of Built Year by Registration Year Cateogory from 1805 to 2020 ", x= "Registration Year Cateogory", y="Built Year")
print(answer1.1.2_better)
```

![](stat545_mda2_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->

**Q2 How are the number of storeys distributed among buildings?**

**Preparation**

``` r
#overview
class(apt_buildings$no_of_storeys)
```

    ## [1] "numeric"

``` r
class(apt_buildings$`non-smoking_building`)
```

    ## [1] "character"

``` r
summary(apt_buildings$no_of_storeys)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   0.000   3.000   5.000   7.738  10.000  51.000

``` r
unique(apt_buildings$`non-smoking_building`)
```

    ## [1] "YES" "NO"  NA

``` r
#clean
Q2cleaned <- apt_buildings %>%
  select(id, no_of_storeys, `non-smoking_building`) %>%
  filter(!is.na(no_of_storeys)&!is.na(`non-smoking_building`))
head(Q2cleaned)
```

    ## # A tibble: 6 × 3
    ##      id no_of_storeys `non-smoking_building`
    ##   <dbl>         <dbl> <chr>                 
    ## 1 10359            17 YES                   
    ## 2 10360            14 NO                    
    ## 3 10361             4 YES                   
    ## 4 10362             5 YES                   
    ## 5 10363             4 YES                   
    ## 6 10364             4 NO

**Q2.1 Summarizing**

*Task*: Compute the *range*, *mean*, and *two other summary statistics*
of **one numerical variable** across the groups of **one categorical
variable** from your data (Task 1). two other summary statistics:
standard deviation, median one numerical variable: no_of_storeys one
categorical variable: `non-smoking_building`

*Comment*: This is helpful. It highly links to Question 2. Computing
mean and median supports measuring central tendency and computing range
and standard deviation supports measuring statistical dispersion. In
addition attaching *`non-smoking_building`* to the initial question can
help with making assumptions of the relationship between building’s
smoking type and the number of storeys.

``` r
#calculate within categories
answer1.2.1_seperate <- Q2cleaned %>%
  group_by(`non-smoking_building`)%>%
  summarize(
    Range=paste(min(no_of_storeys),"-", max(no_of_storeys)),
    Mean= mean(no_of_storeys),
    Stdev=sd(no_of_storeys),
    Median=median(no_of_storeys)
  )
#calculate all
answer1.2.1_total <-Q2cleaned %>%
  summarize(
    `non-smoking_building`= "TOTAL",
    Range=paste(min(no_of_storeys),"-", max(no_of_storeys)),
    Mean= mean(no_of_storeys),
    Stdev=sd(no_of_storeys),
    Median=median(no_of_storeys)
  )
#integrate 
answer1.2.1 <- bind_rows(answer1.2.1_seperate, answer1.2.1_total)
  
print(answer1.2.1)
```

    ## # A tibble: 3 × 5
    ##   `non-smoking_building` Range   Mean Stdev Median
    ##   <chr>                  <chr>  <dbl> <dbl>  <dbl>
    ## 1 NO                     0 - 43  8.26  6.45      5
    ## 2 YES                    3 - 51  7.15  5.89      4
    ## 3 TOTAL                  0 - 51  7.83  6.27      5

**Q2.2 Graphing**

*Task*:Create a graph that has at least two geom layers (Task 8).

*Comment*: This is helpful. I used geom_boxplot to present results from
summarizing task. In addition, I used geom_jitter. Adding individual
data points on top of boxplots shows the distribution of data points
within each category. This can be helpful to visualize the spread of the
data.

``` r
#offering an overall view (not distinguish whether buildings allow smoking or not)
answer1.2.2_total <- Q2cleaned %>%
  reframe(
    `non-smoking_building` = "TOTAL",
    no_of_storeys = no_of_storeys
  )
#visualize
answer1.2.2 <- bind_rows(Q2cleaned, answer1.2.2_total) %>%
  ggplot(aes(`non-smoking_building`, no_of_storeys))+
  geom_boxplot()+ # geom layer 1, visualizing summarizing task results
  geom_jitter(aes(color = `non-smoking_building`), alpha=0.04) + # geom layer 2
  labs(
    title= "The Box Plot of the Number of Storeys Categorized by Building Smoking Type",
    x= "Is the Building Non-Smoking",
    y= "The Number of Storeys"
  )+
  scale_color_manual(values = c("TOTAL" = "blue", "YES"="red", "NO" = "red"))
  
print(answer1.2.2)
```

![](stat545_mda2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

**Q3 How is the distribution of separate gas, hydro, and water meter
services among buildings?**

**Preparation**

``` r
#clean
Q3cleaned <- apt_buildings %>%
  select(id, separate_gas_meters, separate_hydro_meters, separate_water_meters) %>%
  filter(!is.na(separate_gas_meters)&!is.na(separate_hydro_meters)&!is.na(separate_water_meters))
head(Q3cleaned)
```

    ## # A tibble: 6 × 4
    ##      id separate_gas_meters separate_hydro_meters separate_water_meters
    ##   <dbl> <chr>               <chr>                 <chr>                
    ## 1 10359 NO                  YES                   NO                   
    ## 2 10360 NO                  YES                   NO                   
    ## 3 10361 NO                  YES                   NO                   
    ## 4 10362 NO                  YES                   NO                   
    ## 5 10363 NO                  YES                   NO                   
    ## 6 10364 NO                  YES                   NO

**Q3.1 Summarizing**

*Task*: Compute the number of observations for at least one of your
categorical variables. Do not use the function table() (Task 2).
Categorical variables: separate_gas_meters, separate_hydro_meters,
separate_water_meters

*Comment*: This is helpful. It offers an overview to understand the
distribution of separate gas, hydro, and water meter services among
buildings.

``` r
library(tidyverse)
#gas
gas_count <- Q3cleaned %>%
  group_by(separate_gas_meters) %>%
  count()%>%
  pivot_wider(names_from = "separate_gas_meters", values_from = "n")
#hydro
hydro_count <- Q3cleaned %>%
  group_by(separate_hydro_meters) %>%
  count()%>%
  pivot_wider(names_from = "separate_hydro_meters", values_from = "n")
#water
water_count <- Q3cleaned %>%
  group_by(separate_water_meters) %>%
  count()%>%
  pivot_wider(names_from = "separate_water_meters", values_from = "n")
#combine
answer1.3.1 <- bind_rows(gas_count, hydro_count, water_count) %>%
  mutate(Service_type=c("gas", "hydro", "water"))%>%
  select(Service_type, everything())

print(answer1.3.1)
```

    ## # A tibble: 3 × 3
    ##   Service_type    NO   YES
    ##   <chr>        <int> <int>
    ## 1 gas           3308    58
    ## 2 hydro         1197  2169
    ## 3 water         3321    45

**Q3.2 Graphing**

*Task*:Create a graph that has at least two geom layers (Task 8).

*Comment*:This is helpful. This visualization supports comparing the
availability of separate gas, hydro, water services. Adding a geom_text
layer makes the exact number clear to the audience.

``` r
#make tibble suitable for visualization
longer<-answer1.3.1%>% #using the outcome from summarizing task
  pivot_longer(cols=c(-Service_type),
             names_to='YES_NO',
             values_to='Count') 

answer1.3.2 <- ggplot(longer, aes(x = Service_type, y = Count, fill = YES_NO)) +
  geom_bar(stat = "identity") + #geom layer1
  labs(
    title = "Count of YES and NO for Each Service Type",
    x = "Service Type",
    y = "Count"
  )+
  geom_text(aes(label=Count),position = position_stack(vjust = 0.5), size=3) #geom layer 2

print(answer1.3.2)
```

![](stat545_mda2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

**Q4What is the relationship between window types and year built?**

**Preparation**

``` r
#overview:
unique(apt_buildings$window_type)
```

    ## [1] "DOUBLE PANE" "SINGLE PANE" "THERMAL"     NA

``` r
#clean
Q4cleaned<-apt_buildings %>%
  select(id, window_type, year_built)%>%
filter(!is.na(window_type)&!is.na(year_built))
head(Q4cleaned)
```

    ## # A tibble: 6 × 3
    ##      id window_type year_built
    ##   <dbl> <chr>            <dbl>
    ## 1 10359 DOUBLE PANE       1967
    ## 2 10360 DOUBLE PANE       1970
    ## 3 10361 DOUBLE PANE       1927
    ## 4 10362 DOUBLE PANE       1959
    ## 5 10363 DOUBLE PANE       1943
    ## 6 10364 SINGLE PANE       1952

**Q4.1 Summarizing**

*Task*:Compute the number of observations for at least one of your
categorical variables. Do not use the function table() (Task 2).
categorical variable:window_type

*Comment*: This is helpful. *window_type* is a categorical variable,
making sense to the task. This task does not directly answer Question 4.
However, it brings insight to understand the variable window_type
related to Question 4.

``` r
answer1.4.1 <- Q4cleaned %>%
  group_by(window_type) %>%
  count()
print(answer1.4.1)
```

    ## # A tibble: 3 × 2
    ## # Groups:   window_type [3]
    ##   window_type     n
    ##   <chr>       <int>
    ## 1 DOUBLE PANE  2884
    ## 2 SINGLE PANE   467
    ## 3 THERMAL        85

**Q4.2 Graphing**

*Task*: Make a graph where it makes sense to customize the alpha
transparency (Task 7).

*Comment*: This is helpful. Because there are overlaps, the transparency
makes sense.The darker color presents higher density. This graph can
give audiences an overview and help them come up an assumption for
Question 4.

``` r
answer1.4.2 <- Q4cleaned%>%
  ggplot(aes(window_type,year_built))+
  geom_jitter(aes(color = window_type), alpha=0.2)+ #customize the transparency
  labs(title="Scatter Plot of Built Year by Window Type",
      x="Window Type",
      y="Built Year")
print(answer1.4.2)
```

![](stat545_mda2_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

**While getting closer to the research questions, there are still
aspects remain unclear and some needs to redefine. Particularly:**

- Q1 needs redefine. The key problem is that the range and variety of
  year_registered is not as wide as year_built. While year_built ranges
  from 1805-2019, buildings mainly registered in 2017. Digging into more
  details about the density of building registered year still has value,
  but redefining the question as *what is the relationship between
  year_registered and year_built?* makes more sense to explore the City
  of Toronto’s advancement in managing apartment buildings.

- Q2 is basically answered. Further exploration may include exploring
  the relationship between non-smoking_building and the number of
  storeys.

- Q3 is basically answered. Further exploration may include exploring
  how different combinations of
  separate_gas_meters,separate_hydro_meters,separate_water_meters
  services are distributed among buildings.

- Q4 remains unclear. The gap of the number of buildings categorized by
  window types is large. Need to find a suitable model to conduct
  statistical test to further answer this question.

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

**I choose the data set *Q1cleaned* in Task 1 for Question 1 as the
context to answer this question.**

**It is tidy because it meets the three requirements. Each row
represents a distinct and individual unit of data. Each column
represents a distinct variable that is measured for each of the
observations. Each cell holds a single, meaningful data point related to
the observation and variable it intersects.**

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

Before:

``` r
head(Q1cleaned)
```

    ## # A tibble: 6 × 3
    ##      id year_built year_registered
    ##   <dbl>      <dbl>           <dbl>
    ## 1 10359       1967            2017
    ## 2 10360       1970            2017
    ## 3 10361       1927            2017
    ## 4 10362       1959            2017
    ## 5 10363       1943            2017
    ## 6 10365       1959            2017

After:

``` r
#untidy my data
untidied <- Q1cleaned %>%
  #make 2 rows present the same observation
  add_row(id=10359, year_built=1967, year_registered=2017)%>% 
  arrange(id)%>%
  #violate Each column is a **variable**
  tidyr::unite(col=BuiltYear_RegisteredYear,c("year_built","year_registered"), sep="_")
#violate each cell is a **value**
untidied[3,"BuiltYear_RegisteredYear"]<-"trhivhsk"
head(untidied)
```

    ## # A tibble: 6 × 2
    ##      id BuiltYear_RegisteredYear
    ##   <dbl> <chr>                   
    ## 1 10359 1967_2017               
    ## 2 10359 1967_2017               
    ## 3 10360 trhivhsk                
    ## 4 10361 1927_2017               
    ## 5 10362 1959_2017               
    ## 6 10363 1943_2017

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *How is the distribution of separate gas, hydro, and water meter
    services among buildings?*
2.  *What is the relationship between window types and year built?*
    <!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

**As explained previously, Q1 needs to be redefined. Besides, 4~8
functions are required for the following task, to make a different, Q2
is not complicated enough.**
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidying,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

**I made two versions of my data, one for each question.**

**For Q3**

Before:

``` r
head(Q3cleaned)
```

    ## # A tibble: 6 × 4
    ##      id separate_gas_meters separate_hydro_meters separate_water_meters
    ##   <dbl> <chr>               <chr>                 <chr>                
    ## 1 10359 NO                  YES                   NO                   
    ## 2 10360 NO                  YES                   NO                   
    ## 3 10361 NO                  YES                   NO                   
    ## 4 10362 NO                  YES                   NO                   
    ## 5 10363 NO                  YES                   NO                   
    ## 6 10364 NO                  YES                   NO

After

``` r
# For Q3
Q3new<-Q3cleaned%>%
  #Function 1:mutate()
  mutate(separate_gas_meters = ifelse(separate_gas_meters == "YES", 1, 0),
         separate_hydro_meters = ifelse(separate_hydro_meters == "YES", 1, 0),
         separate_water_meters = ifelse(separate_water_meters == "YES", 1, 0)
         )%>%
  #Function 2: tidyr::unite()
  tidyr::unite(col=gas_hydro_water,c("separate_gas_meters","separate_hydro_meters", "separate_water_meters"), sep="")
print(Q3new)
```

    ## # A tibble: 3,366 × 2
    ##       id gas_hydro_water
    ##    <dbl> <chr>          
    ##  1 10359 010            
    ##  2 10360 010            
    ##  3 10361 010            
    ##  4 10362 010            
    ##  5 10363 010            
    ##  6 10364 010            
    ##  7 10365 010            
    ##  8 10366 010            
    ##  9 10367 010            
    ## 10 10368 000            
    ## # ℹ 3,356 more rows

**For Q4**

Before

``` r
head(Q4cleaned)
```

    ## # A tibble: 6 × 3
    ##      id window_type year_built
    ##   <dbl> <chr>            <dbl>
    ## 1 10359 DOUBLE PANE       1967
    ## 2 10360 DOUBLE PANE       1970
    ## 3 10361 DOUBLE PANE       1927
    ## 4 10362 DOUBLE PANE       1959
    ## 5 10363 DOUBLE PANE       1943
    ## 6 10364 SINGLE PANE       1952

After:

``` r
Q4new <- Q4cleaned %>%
  #Function 3: filter()>>> because the window types are not mutually exclusive, DOUBLE PANE can also be THERMAL
  filter(window_type!="THERMAL")%>%
  mutate(window_type=ifelse(window_type=="DOUBLE PANE", 2, 1))%>%
  #Function 4: rename()
  rename(pane_type = window_type)%>%
  #Function 5: as.integer()
  mutate(pane_type = as.integer(pane_type))
print(Q4new) 
```

    ## # A tibble: 3,351 × 3
    ##       id pane_type year_built
    ##    <dbl>     <int>      <dbl>
    ##  1 10359         2       1967
    ##  2 10360         2       1970
    ##  3 10361         2       1927
    ##  4 10362         2       1959
    ##  5 10363         2       1943
    ##  6 10364         1       1952
    ##  7 10365         2       1959
    ##  8 10366         1       1971
    ##  9 10367         2       1969
    ## 10 10369         2       1945
    ## # ℹ 3,341 more rows

Note: functions like select() already used to prepare data for Task 1.
Because I am not sure if all functions I listed above counts, I
practiced Function 6: full_join() to make one data set for Q3 and Q4.

``` r
#Function 6: full_join()
Q3Q4<-full_join(Q3new, Q4new, by="id")
print(Q3Q4)
```

    ## # A tibble: 3,442 × 4
    ##       id gas_hydro_water pane_type year_built
    ##    <dbl> <chr>               <int>      <dbl>
    ##  1 10359 010                     2       1967
    ##  2 10360 010                     2       1970
    ##  3 10361 010                     2       1927
    ##  4 10362 010                     2       1959
    ##  5 10363 010                     2       1943
    ##  6 10364 010                     1       1952
    ##  7 10365 010                     2       1959
    ##  8 10366 010                     1       1971
    ##  9 10367 010                     2       1969
    ## 10 10368 000                    NA         NA
    ## # ℹ 3,432 more rows

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**:

What is the relationship between window pane types and year built?
(coming from the original Question 4)

**Variable of interest**:

pane_type (from Task2)

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

**I use lm() for this task.The window pane type is dependent, built year
is independent.The data set used is Q4new from Task 2.**

``` r
model<-lm(pane_type ~ year_built, data=Q4new)
print(model)
```

    ## 
    ## Call:
    ## lm(formula = pane_type ~ year_built, data = Q4new)
    ## 
    ## Coefficients:
    ## (Intercept)   year_built  
    ##   -0.239597     0.001071

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
#prepare
library(broom)

#output a single value: p-value, using broom
results<-tidy(model)%>%
  select(term, p.value) #select the column that contains the thing I'm looking for
print(results)
```

    ## # A tibble: 2 × 2
    ##   term         p.value
    ##   <chr>          <dbl>
    ## 1 (Intercept) 0.701   
    ## 2 year_built  0.000780

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
library(here)
```

    ## here() starts at /Users/zkr/stat545_mda_KerenZhu

``` r
#check the summary table that I made from Task 1 for this task
print(answer1.2.1)
```

    ## # A tibble: 3 × 5
    ##   `non-smoking_building` Range   Mean Stdev Median
    ##   <chr>                  <chr>  <dbl> <dbl>  <dbl>
    ## 1 NO                     0 - 43  8.26  6.45      5
    ## 2 YES                    3 - 51  7.15  5.89      4
    ## 3 TOTAL                  0 - 51  7.83  6.27      5

``` r
#write it as a csv file in my `output` folder. Use the `here::here()` function.
output_path <- here::here("output", "answer1.2.1.csv")
write_csv(answer1.2.1,file = output_path)
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

``` r
# Define the output file path
model_output_path <- here::here("output", "model.rds") 
# Save my model object from Task 3 to an R binary file (an RDS).Use the functions `saveRDS()`
saveRDS(model, file = model_output_path)
# Reload use `readRDS()`.
reload_model <- readRDS(model_output_path)
```

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
