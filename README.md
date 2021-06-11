# R assessment

We will be sending you a basic skills test that will test your skills in R. As a part of this test, you will need to sign up for a free GitHub account (https://github.com/join).

If you have any questions, send an email to Beatriz.ValcarcelSalamanca@fhi.no

**ALL TASKS MAY BE COMPLETED IN EITHER NORWEGIAN/DANISH/SWEDISH OR ENGLISH**

# Before you start

You will need to install some FHI R packages. You can do this via:

```
install.packages(c("fhidata", "fhiplot", "fhimaps"), repos = c("https://folkehelseinstituttet.github.io/drat", "https://cran.rstudio.com"))
```

# TASK 1

## Background

This is an assignment related to infectious disease surveillance that will test your skills in R. The deadline for submission of the test is found in your email. Please read through all of this document before beginning the test.

The scenario is broadly as follows:

You are responsible for `Disease X`. There is a data file `data_raw/individual_level_data.RDS` which contains individual level daily data for `Disease X` for 356 municipalities between `2010-01-01` and `2020-12-31`. Each row of this dataset corresponds to 1 sick person on that date. To be more explicit:

- If there are 100 rows for `municip0301` on `2010-01-01` it means that there were 100 sick people in `municip0301` on `2010-01-01`.
- If there are 0 rows for `municip0301` on `2010-01-01` it means that there were 0 sick people in `municip0301` on `2010-01-01`.

For each municipality you will use the data between `2010-01-01` and `2019-12-31` to build a regression model that predicts the expected number of sick people. You will then use this model to predict the expected number of sick people between `2020-01-01` and `2020-12-31`. You will then produce Excel sheets and graphs that contain information regarding the suspected outbreaks in 2020.

The dataset `fhidata::norway_locations_names()` contains a data.table that links `location_code` (e.g. `municip0301`) to `location_name` (e.g. `Oslo`). These are real municipality numbers (`kommunenummer`) based on the 2020 municipality lists (https://en.wikipedia.org/wiki/List_of_municipality_numbers_of_Norway).

## English <-> Norwegian

`municipality` = `kommune`

`county` = `fylke`

## Test: Setup 

1. Create a new repository called `xx_05_submission` in GitHub (https://help.github.com/articles/creating-a-new-repository/)
2. Clone your new repository to your local computer (https://help.github.com/articles/cloning-a-repository/)
3. Download the following zip file: https://github.com/fhi-beta/xx_05/archive/main.zip
4. Copy the files into your local `xx_05_submission` folder

## Folder/File Setup

You are required to put your code in the following locations:

- All functions are to be placed in .R files located in the `code_task1` folder (e.g. see `code_task1/CreateFakeData.R`)
- You will write your master file/script/code (that runs all of the requested analyses by calling the functions in `code/*`) in `Run_task1.R` (this file already has a few lines of code in it)
- Your results will be saved in the `results_task1` folder

## Test: Coding

5. Load in the data file `data_raw/individual_level_data.RDS`
6. Create a dataset that contains the aggregated number of sick people per day per municipality.
7. Ensure that your aggregated dataset includes rows/days with zero sick people (e.g. if there were no rows for `2010-01-01`/`municip0301` in `data_raw/individual_level_data.RDS` then your aggregated dataset will still need to have one row for `2010-01-01`/`municip0301` with the value 0).
8. Collapse your data down to iso-year/iso-weeks for each municipality. If you are not familiar with iso-week/years, there is information available at https://en.wikipedia.org/wiki/ISO_week_date and https://rdrr.io/cran/surveillance/man/isoWeekYear.html

Do the following for each of the 356 municipalities (municip*):

9. Split the data into training data (`2010-01` to `2019-52`) and production data (`2020-01` to `2020-53`)
10. Use the training data to estimate: 

    a) the average number of cases per week (e.g. "from 2010 to 2019 we saw an average of 4 cases in week 1, 5 cases in week 2, 6 cases in week 3, ...)

    b) the standard deviation of number of cases per week (e.g. "from 2010 to 2019 we saw that the number of cases in week 1 had a standard deviation of 3, the number of cases in week 2 has a standard deviation of 5, ...).

11. Use your results from #10 to create an "alert" threshold for weeks 1-52 (i.e. "How many people need to get sick for you to think it is an outbreak in week 1, week 2, week 3, week 4, ...?"). You can do this via the formula: average + 2*standard deviation.
12. Identify the potential outbreaks in the training data (i.e. number of sick people > thresholds created in #11)
13. Exclude the potential outbreaks from the training data
14. Repeat steps 10 and 11 to create new "alert" thresholds 
15. Identify the potential outbreaks in the production data, using the thresholds identified in #14
16. Create and save an excel sheet with the potential outbreaks in `results_task1/municip/outbreaks_municipXXXX.xlsx` (i.e. one Excel file for each municipality).
17. Create and save a figure that provides a good overview of the situation in the municipality in `results_task1/municip/outbreaks_municipXXXX.png` (i.e. one graph for each municipality). An example of one such graph is available at https://github.com/fhi-beta/xx_04/blob/master/example_report/5218---overvaking-av-totaldodelighet-5218-uke-52.pdf

**Note:** the graphs must include titles with the real municipality name (e.g. `Oslo` instead of `municip0301`). This information is available from `fhidata::norway_locations_names()`.

## Test: Creative Assignment

Every week there is a meeting at Folkehelseinstituttet to discuss the current outbreak situation in Norway. The current date is `2020-12-31`. You need to produce graph(s) and/or table(s) for this meeting that will give the meeting participants a good summary of the current situation for `Disease X` in Norway and the situation in the last few weeks. In this meeting you would have a maximum of 2 minutes to present, so your graph(s) and/or table(s) must be easily understood.

You may use external structural data files for this creative assigment (e.g. shapefiles for Norway) if you think it is appropriate.

The following links may be helpful for you:

- https://folkehelseinstituttet.github.io/fhimaps/articles/fhimaps.html
- https://folkehelseinstituttet.github.io/fhiplot/articles/

Please save your graph(s) and/or table(s) into `results_task1/creative_assignment`.

Please provide comments in your code that indicate which parts of your code produce the graph(s) and/or table(s) for this creative assignment.

## Test: Submission

18. Commit your results and push them back to GitHub
19. Verify that your code is viewable in your GitHub repository on the internet (both in `Run_task1.R` and in `code_task1/*.R`)
20. Verify that in your GitHub repository on the internet you have 356 Excel files and 356 graphs in `results_task1/municip/`
21. Verify that in your GitHub repository on the internet you have graph(s) and/or table(s) in `results_task1/creative_assignment/`
22. Send GryMarysol.Groneng@fhi.no and Beatriz.ValcarcelSalamanca@fhi.no an email with the link to your repository
