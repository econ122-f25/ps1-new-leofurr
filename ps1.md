Problem Set 1
================
Leo Furr
2025-09-11

### Introduction

This problem set is designed to test your understanding of data
wrangling concepts using the **`tidyverse`**, specifically **`dplyr`**
and **`tidyr`**, as well as foundational R concepts. You’ll primarily
work with the `mtcars`, a simulated sales dataset, and new simulated
student demographic/grade datasets, applying core verbs and functions to
prepare data for analysis. This activity should take approximately 60
minutes.

### Instructions

- Read each task carefully.
- Write your R code in the provided code chunks.
- Run the code to see your output and verify your results.
- `mtcars` is a built-in R dataset (or part of the `tidyverse`
  installation).

------------------------------------------------------------------------

## Part 0: R Fundamentals (10 minutes)

This section will test your understanding of basic R syntax, data types,
and vector operations.

### Task 0.1: Data Types and Logical Operations

1.  Create a variable `course_name` and assign it the string
    `"Introduction to Data Science"`.
2.  Create a variable `num_students` and assign it the integer value
    `45`.
3.  Check the data type (`class()`) of `course_name` and `num_students`.
4.  Create a numeric variable `pi_value` with the value $3.14159$.
5.  Create a logical variable `is_active` and set it to `TRUE`.
6.  Evaluate the following logical expressions and print their results:
    - Is `num_students` greater than or equal to `50`?
    - Is `course_name` exactly equal to `"introduction to data science"`
      (case-sensitive)?

``` r
# Your code here
# 1
course_name <- "Introduction to Data Science"
# 2 
num_students <- 45L
# 3 
class(course_name) == "character"
```

    ## [1] TRUE

``` r
class(num_students) == "integer"
```

    ## [1] TRUE

``` r
# 4 
pi_value <- 3.14159
#5 
is_active <- TRUE 
#6 
# is num_students >= 50?
check_num_students <- num_students >= 50 
print(check_num_students)
```

    ## [1] FALSE

``` r
# is course_name exactly equal to "introduction to data science"?
check_course_name <- course_name == "introduction to data science"
print(check_course_name)
```

    ## [1] FALSE

------------------------------------------------------------------------

### Task 0.2: Working with Vectors

1.  Create a numeric vector named `temperatures` with the values
    $22, 25, 19, 28, 23$.
2.  Calculate the `sum()` and `mean()` of the `temperatures` vector.
3.  Add a new temperature, $30$, to the `temperatures` vector (re-assign
    the variable).
4.  Create a new logical vector named `is_hot` that is `TRUE` for
    temperatures greater than $25$ and `FALSE` otherwise.

``` r
# Your code here

# 1 
temperatures <- c(22, 25, 19, 28, 23)
#2 
sum(temperatures)
```

    ## [1] 117

``` r
mean(temperatures)
```

    ## [1] 23.4

``` r
#3 
temperatures <- c(22, 25, 19, 28, 23, 30)
#4 
is_hot <- temperatures >= 25
print(is_hot)
```

    ## [1] FALSE  TRUE FALSE  TRUE FALSE  TRUE

------------------------------------------------------------------------

## Part 1: `dplyr` Fundamentals with `mtcars` (25 minutes)

This section focuses on applying core `dplyr` verbs to the classic
`mtcars` dataset. The `mtcars` dataset contains information about
various car models from 1973-74.

### Task 1.1: Filtering and Arranging

1.  The `mtcars` dataset has been pre-processed for you to include
    `car_model` as a column.
2.  Filter the dataset to include only cars with `cyl` (number of
    cylinders) equal to `4` or `6` AND `mpg` (miles per gallon) greater
    than `20`.
3.  Arrange the results first by `mpg` in ascending order, and then by
    `cyl` in descending order.

``` r
# Pre-processing step: Convert rownames to car_model
mtcars_processed <- mtcars %>%
  rownames_to_column("car_model")

mtcars
```

    ##                      mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## Mazda RX4           21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## Mazda RX4 Wag       21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## Datsun 710          22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## Hornet 4 Drive      21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## Hornet Sportabout   18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
    ## Valiant             18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    ## Duster 360          14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
    ## Merc 240D           24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## Merc 230            22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## Merc 280            19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    ## Merc 280C           17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    ## Merc 450SE          16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
    ## Merc 450SL          17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
    ## Merc 450SLC         15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
    ## Cadillac Fleetwood  10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
    ## Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
    ## Chrysler Imperial   14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
    ## Fiat 128            32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## Honda Civic         30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## Toyota Corolla      33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## Toyota Corona       21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## Dodge Challenger    15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
    ## AMC Javelin         15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
    ## Camaro Z28          13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
    ## Pontiac Firebird    19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
    ## Fiat X1-9           27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## Porsche 914-2       26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## Lotus Europa        30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## Ford Pantera L      15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## Ferrari Dino        19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## Maserati Bora       15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
    ## Volvo 142E          21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

``` r
mtcars_processed
```

    ##              car_model  mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## 1            Mazda RX4 21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## 2        Mazda RX4 Wag 21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## 3           Datsun 710 22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## 4       Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## 5    Hornet Sportabout 18.7   8 360.0 175 3.15 3.440 17.02  0  0    3    2
    ## 6              Valiant 18.1   6 225.0 105 2.76 3.460 20.22  1  0    3    1
    ## 7           Duster 360 14.3   8 360.0 245 3.21 3.570 15.84  0  0    3    4
    ## 8            Merc 240D 24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## 9             Merc 230 22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## 10            Merc 280 19.2   6 167.6 123 3.92 3.440 18.30  1  0    4    4
    ## 11           Merc 280C 17.8   6 167.6 123 3.92 3.440 18.90  1  0    4    4
    ## 12          Merc 450SE 16.4   8 275.8 180 3.07 4.070 17.40  0  0    3    3
    ## 13          Merc 450SL 17.3   8 275.8 180 3.07 3.730 17.60  0  0    3    3
    ## 14         Merc 450SLC 15.2   8 275.8 180 3.07 3.780 18.00  0  0    3    3
    ## 15  Cadillac Fleetwood 10.4   8 472.0 205 2.93 5.250 17.98  0  0    3    4
    ## 16 Lincoln Continental 10.4   8 460.0 215 3.00 5.424 17.82  0  0    3    4
    ## 17   Chrysler Imperial 14.7   8 440.0 230 3.23 5.345 17.42  0  0    3    4
    ## 18            Fiat 128 32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## 19         Honda Civic 30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## 20      Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
    ## 21       Toyota Corona 21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## 22    Dodge Challenger 15.5   8 318.0 150 2.76 3.520 16.87  0  0    3    2
    ## 23         AMC Javelin 15.2   8 304.0 150 3.15 3.435 17.30  0  0    3    2
    ## 24          Camaro Z28 13.3   8 350.0 245 3.73 3.840 15.41  0  0    3    4
    ## 25    Pontiac Firebird 19.2   8 400.0 175 3.08 3.845 17.05  0  0    3    2
    ## 26           Fiat X1-9 27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## 27       Porsche 914-2 26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## 28        Lotus Europa 30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## 29      Ford Pantera L 15.8   8 351.0 264 4.22 3.170 14.50  0  1    5    4
    ## 30        Ferrari Dino 19.7   6 145.0 175 3.62 2.770 15.50  0  1    5    6
    ## 31       Maserati Bora 15.0   8 301.0 335 3.54 3.570 14.60  0  1    5    8
    ## 32          Volvo 142E 21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2

``` r
# Your code starts here: Filter and arrange mtcars_processed
# Your code here

mtcars_processed %>% filter(cyl <= 6 & mpg >= 20) %>% 
  arrange(mpg) %>% arrange(desc(cyl))
```

    ##         car_model  mpg cyl  disp  hp drat    wt  qsec vs am gear carb
    ## 1       Mazda RX4 21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
    ## 2   Mazda RX4 Wag 21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
    ## 3  Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
    ## 4      Volvo 142E 21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
    ## 5   Toyota Corona 21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
    ## 6      Datsun 710 22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
    ## 7        Merc 230 22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
    ## 8       Merc 240D 24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
    ## 9   Porsche 914-2 26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
    ## 10      Fiat X1-9 27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
    ## 11    Honda Civic 30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
    ## 12   Lotus Europa 30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
    ## 13       Fiat 128 32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
    ## 14 Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1

### Short Answer

Based on your filtered and arranged results, briefly describe the
characteristics of the cars that appear at the top of your output. What
does this tell you about the relationship between cylinders and miles
per gallon in this subset?

## Response: At the top of my output, I notice that in this subset there is not a single car with 4 cylinders that has a lower miles per gallon than a car with 6 cylinders. The relationship I notice, then, is that cars with more cylinders tend to have a lower miles per gallon.

### Task 1.2: Mutating and Selecting

Using the `mtcars_processed` dataset:

1.  Create a new column `hp_per_wt` by dividing `hp` (horsepower) by
    `wt` (weight in 1000 lbs).
2.  Create another new column `qsec_category` based on `qsec` (1/4 mile
    time):
    - `"Fast"` if `qsec < 17`
    - `"Medium"` if `qsec >= 17` and `qsec < 19`
    - `"Slow"` if `qsec >= 19`
3.  Select only the `car_model`, `mpg`, `hp`, `wt`, `hp_per_wt`, and
    `qsec_category` columns.

``` r
# Your code here

#1
mtcars_processed <- mtcars_processed %>% mutate(hp_per_wt = hp / wt)

#2
mtcars_processed <- mtcars_processed %>% mutate(qsec_category = case_when(qsec < 17 ~ "Fast", qsec >=17 & qsec < 19 ~ "Medium", qsec >= 19 ~ "Slow"))

#3
mtcars_processed %>% select(car_model, mpg, hp, wt, hp_per_wt, qsec_category)
```

    ##              car_model  mpg  hp    wt hp_per_wt qsec_category
    ## 1            Mazda RX4 21.0 110 2.620  41.98473          Fast
    ## 2        Mazda RX4 Wag 21.0 110 2.875  38.26087        Medium
    ## 3           Datsun 710 22.8  93 2.320  40.08621        Medium
    ## 4       Hornet 4 Drive 21.4 110 3.215  34.21462          Slow
    ## 5    Hornet Sportabout 18.7 175 3.440  50.87209        Medium
    ## 6              Valiant 18.1 105 3.460  30.34682          Slow
    ## 7           Duster 360 14.3 245 3.570  68.62745          Fast
    ## 8            Merc 240D 24.4  62 3.190  19.43574          Slow
    ## 9             Merc 230 22.8  95 3.150  30.15873          Slow
    ## 10            Merc 280 19.2 123 3.440  35.75581        Medium
    ## 11           Merc 280C 17.8 123 3.440  35.75581        Medium
    ## 12          Merc 450SE 16.4 180 4.070  44.22604        Medium
    ## 13          Merc 450SL 17.3 180 3.730  48.25737        Medium
    ## 14         Merc 450SLC 15.2 180 3.780  47.61905        Medium
    ## 15  Cadillac Fleetwood 10.4 205 5.250  39.04762        Medium
    ## 16 Lincoln Continental 10.4 215 5.424  39.63864        Medium
    ## 17   Chrysler Imperial 14.7 230 5.345  43.03087        Medium
    ## 18            Fiat 128 32.4  66 2.200  30.00000          Slow
    ## 19         Honda Civic 30.4  52 1.615  32.19814        Medium
    ## 20      Toyota Corolla 33.9  65 1.835  35.42234          Slow
    ## 21       Toyota Corona 21.5  97 2.465  39.35091          Slow
    ## 22    Dodge Challenger 15.5 150 3.520  42.61364          Fast
    ## 23         AMC Javelin 15.2 150 3.435  43.66812        Medium
    ## 24          Camaro Z28 13.3 245 3.840  63.80208          Fast
    ## 25    Pontiac Firebird 19.2 175 3.845  45.51365        Medium
    ## 26           Fiat X1-9 27.3  66 1.935  34.10853        Medium
    ## 27       Porsche 914-2 26.0  91 2.140  42.52336          Fast
    ## 28        Lotus Europa 30.4 113 1.513  74.68605          Fast
    ## 29      Ford Pantera L 15.8 264 3.170  83.28076          Fast
    ## 30        Ferrari Dino 19.7 175 2.770  63.17690          Fast
    ## 31       Maserati Bora 15.0 335 3.570  93.83754          Fast
    ## 32          Volvo 142E 21.4 109 2.780  39.20863        Medium

### Short Answer

Why might creating `hp_per_wt` and `qsec_category` be useful metrics
when analyzing car performance, beyond just raw horsepower and weight?

For `hp_per_wt`, having a heavier car naturally requires more horsepower
to move it, so this metric allows consideration for how much horsepower
a car has for their weight. And for qsec_category, being aware of very
small differences in the quarter-mile time doesn’t matter as much
compared to being aware if they can complete it quickly or slowly, so
making the variable easier to comprehend at a glance is helpful.

------------------------------------------------------------------------

### Task 1.3: Grouped Summaries

Using the `mtcars_processed` dataset:

1.  Group the data by `cyl` (number of cylinders) and `am` (transmission
    type: 0 for automatic, 1 for manual).
2.  For each group, calculate the **average `mpg`**, the **median
    `hp`**, and the **number of cars** (`n()`).
3.  Arrange the final result first by `cyl` (ascending) and then by
    `average_mpg` (descending).

``` r
# Your code here

#1, 2, 3
mtcars_processed %>% group_by(cyl, am) %>% summarize(avg_mpg = mean(mpg), median_hp = median(hp), count = n()) %>% arrange(cyl) %>% arrange(desc(avg_mpg))
```

    ## # A tibble: 6 × 5
    ## # Groups:   cyl [3]
    ##     cyl    am avg_mpg median_hp count
    ##   <dbl> <dbl>   <dbl>     <dbl> <int>
    ## 1     4     1    28.1      78.5     8
    ## 2     4     0    22.9      95       3
    ## 3     6     1    20.6     110       3
    ## 4     6     0    19.1     116.      4
    ## 5     8     1    15.4     300.      2
    ## 6     8     0    15.0     180      12

### Short Answer

Based on your grouped summary, what general trends do you observe
regarding average `mpg` and median `hp` across different combinations of
`cylinders` and `transmission` types? How do these summaries help
differentiate car characteristics?

Response: The general trends I notice are that mpg tends to be higher
for cars with less cylinders and for manual cars, and that horsepower
tends to be higher for cars with more cylinders. A trend between
horsepower and transmission types is hard to be certain about, though,
because the data seems to indicate that automatic cars have more
horsepower except for cars with eight cylinders. However, there are only
two cars that are manual and have eight cylinders, so more data may be
required for this data set. My intuition tells me that the cars in this
subset may be supercars, which explains why their horsepower is so high.
These summaries help differentiate car characteristics by controlling
for certain variables that would make broader differentiation unhelpful.

------------------------------------------------------------------------

## Part 2: Reshaping and Joining Data (25 minutes)

For this part, we’ll explore reshaping and joining using a simulated
sales dataset and new simulated student enrollment data.

**Run this code chunk first to set up the data:**

``` r
# Part 2 Data Setup
product_sales_wide <- tibble(
  product = c("Laptop", "Monitor", "Keyboard"),
  `2020` = c(1000, 500, 800),
  `2021` = c(1100, 550, 850),
  `2022` = c(1250, 600, 900)
) %>%
  rename(Year_2020 = `2020`, Year_2021 = `2021`, Year_2022 = `2022`) # Rename to avoid issues with non-syntactic names

# Simulated Student and Grade Data
students_demographics <- tibble(
  student_id = c("S001", "S002", "S003", "S004", "S005", "S007"), # S007 is a new student, no grades yet
  student_name = c("Alice", "Bob", "Charlie", "David", "Eve", "Frank"),
  major = c("CS", "Math", "Physics", "CS", "Biology", "History"),
  enrollment_year = c(2020, 2021, 2020, 2022, 2021, 2023)
)

student_grades <- tibble(
  student_id = c("S001", "S002", "S003", "S001", "S006", "S004"), # S006 has grades but no demographic info
  course_id = c("CS101", "MA201", "PH301", "CS102", "BI101", "CS205"),
  semester = c("Fall 2020", "Spring 2022", "Fall 2021", "Spring 2021", "Fall 2021", "Spring 2023"),
  grade_score = c(92, 85, 88, 78, 75, 90) # Assuming numeric scores for easier calculation
)

# Display the dataframes
print(product_sales_wide)
```

    ## # A tibble: 3 × 4
    ##   product  Year_2020 Year_2021 Year_2022
    ##   <chr>        <dbl>     <dbl>     <dbl>
    ## 1 Laptop        1000      1100      1250
    ## 2 Monitor        500       550       600
    ## 3 Keyboard       800       850       900

``` r
print(students_demographics)
```

    ## # A tibble: 6 × 4
    ##   student_id student_name major   enrollment_year
    ##   <chr>      <chr>        <chr>             <dbl>
    ## 1 S001       Alice        CS                 2020
    ## 2 S002       Bob          Math               2021
    ## 3 S003       Charlie      Physics            2020
    ## 4 S004       David        CS                 2022
    ## 5 S005       Eve          Biology            2021
    ## 6 S007       Frank        History            2023

``` r
print(student_grades)
```

    ## # A tibble: 6 × 4
    ##   student_id course_id semester    grade_score
    ##   <chr>      <chr>     <chr>             <dbl>
    ## 1 S001       CS101     Fall 2020            92
    ## 2 S002       MA201     Spring 2022          85
    ## 3 S003       PH301     Fall 2021            88
    ## 4 S001       CS102     Spring 2021          78
    ## 5 S006       BI101     Fall 2021            75
    ## 6 S004       CS205     Spring 2023          90

------------------------------------------------------------------------

### Task 2.1: New Variable Creation: Wide vs. Long (`product_sales` dataset)

This task demonstrates how creating new variables that depend on
previous time periods is much easier in a long (tidy) format.

1.  **Analysis in Wide Format:** Using the `product_sales_wide` dataset,
    create new columns for the **year-over-year growth rate** for 2021
    and 2022.
    - `Growth_2021`: `(Year_2021 - Year_2020) / Year_2020 * 100`
    - `Growth_2022`: `(Year_2022 - Year_2021) / Year_2021 * 100`

``` r
# Your code here
product_sales_wide %>% mutate(Growth_2021 = (Year_2021 - Year_2020) / Year_2020 * 100, Growth_2022 = (Year_2022 - Year_2021) / Year_2021 * 100)
```

    ## # A tibble: 3 × 6
    ##   product  Year_2020 Year_2021 Year_2022 Growth_2021 Growth_2022
    ##   <chr>        <dbl>     <dbl>     <dbl>       <dbl>       <dbl>
    ## 1 Laptop        1000      1100      1250       10          13.6 
    ## 2 Monitor        500       550       600       10           9.09
    ## 3 Keyboard       800       850       900        6.25        5.88

2.  **Reshape to Long Format:** Reshape `product_sales_wide` into a long
    format called `product_sales_long`. Pivot the year columns
    (`Year_2020`, `Year_2021`, `Year_2022`) into two new columns: `Year`
    and `Sales`. Make sure `Year` is numeric.

``` r
# Your code here
product_sales_long <- product_sales_wide %>% pivot_longer(cols = Year_2020:Year_2022, names_to = "Year", values_to = "Sales")
product_sales_long <- product_sales_long %>% mutate(Year = case_when(Year == "Year_2020" ~ 2020, Year == "Year_2021" ~ 2021, Year == "Year_2022" ~ 2022))
print(product_sales_long)
```

    ## # A tibble: 9 × 3
    ##   product   Year Sales
    ##   <chr>    <dbl> <dbl>
    ## 1 Laptop    2020  1000
    ## 2 Laptop    2021  1100
    ## 3 Laptop    2022  1250
    ## 4 Monitor   2020   500
    ## 5 Monitor   2021   550
    ## 6 Monitor   2022   600
    ## 7 Keyboard  2020   800
    ## 8 Keyboard  2021   850
    ## 9 Keyboard  2022   900

3.  **Analysis in Long Format:** Using the `product_sales_long` dataset,
    calculate a single `Growth_Rate` column that represents the
    year-over-year growth for each product. You should use `group_by()`
    and `lag()`. The formula for growth rate is
    `(current_year_sales - previous_year_sales) / previous_year_sales * 100`.

``` r
# Your code here

product_sales_long %>% group_by(product) %>% mutate(Growth_Rate = (Sales - lag(Sales)) / lag(Sales) * 100)
```

    ## # A tibble: 9 × 4
    ## # Groups:   product [3]
    ##   product   Year Sales Growth_Rate
    ##   <chr>    <dbl> <dbl>       <dbl>
    ## 1 Laptop    2020  1000       NA   
    ## 2 Laptop    2021  1100       10   
    ## 3 Laptop    2022  1250       13.6 
    ## 4 Monitor   2020   500       NA   
    ## 5 Monitor   2021   550       10   
    ## 6 Monitor   2022   600        9.09
    ## 7 Keyboard  2020   800       NA   
    ## 8 Keyboard  2021   850        6.25
    ## 9 Keyboard  2022   900        5.88

### Short Answer

Compare the code required to calculate the year-over-year growth rate in
the wide format versus the long format. Which approach is more concise
and scalable if you had many more years of data? Why is the long format
generally preferred for this type of time-series calculation?

Response: The long-format approach is much more scalable because it only
requires one function to calculate for all of the years, whereas if you
took the wide-format approach, you would have to recreate the function
for each year. Having the year all as one variable allows you to make
calculations using the year so that, for instance, you can utilize the
lag function.

------------------------------------------------------------------------

### Task 2.2: Mutating Joins: Student Demographics and Grades

This task explores combining student demographic information with their
academic performance. Pay close attention to how different join types
handle students who may or may not have corresponding grade records.

1.  **Inner Join:** Perform an `inner_join()` to combine
    `students_demographics` with `student_grades` based on `student_id`.
    This will show students who have **both** demographic information
    and at least one grade record.
2.  **Left Join:** Perform a `left_join()` to combine
    `students_demographics` (left table) with `student_grades` based on
    `student_id`. This will keep **all students** from the demographic
    data and add their grade records where available.
3.  **Advanced Mutate (after Left Join):** After performing the left
    join, calculate the **average `grade_score` for each student**. This
    will require grouping by `student_id` and `student_name`.

``` r
# Your code for inner_join and its explanation
```

``` r
# Your code for left_join and its explanation
```

``` r
# Your code for advanced mutate after left join
```

### Short Answer

Compare the number of rows and the content of the `inner_join()` and
`left_join()` results. Specifically, identify which student(s) are
present in one join but not the other, and explain why. What does the
average grade calculation reveal about students with multiple grades or
no grades?

------------------------------------------------------------------------

### Task 2.3: Filtering Joins: Identifying Student Enrollment Status

This task focuses on using filtering joins to identify different
categories of students based on their enrollment and grade records.

1.  **Enrolled Students:** Use a `semi_join()` to identify which
    students from `students_demographics` are actually enrolled in and
    have a grade record in `student_grades`. This should return only
    columns from `students_demographics`.
2.  **Students Without Grades:** Use an `anti_join()` to identify which
    students from `students_demographics` are in the system but
    currently do *not* have any grade records in `student_grades` (e.g.,
    new students, or those who haven’t completed courses yet). This
    should return only columns from `students_demographics`.
3.  **Grades Without Students:** Use an `anti_join()` to identify any
    grade records in `student_grades` that do *not* correspond to an
    existing student in `students_demographics` (e.g., a data entry
    error or a record for a past student no longer in the demographic
    system). This should return only columns from `student_grades`.

``` r
# Your code for question 1 (semi_join)
```

``` r
# Your code for question 2 (anti_join on students_demographics)
```

``` r
# Your code for question 3 (anti_join on student_grades)
```

### Short Answer

Describe the distinct insights gained from each of the three filtering
joins in this task. How do these joins help in data validation and
understanding the completeness of your student records?
