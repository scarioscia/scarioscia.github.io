---
layout: post
title: Using arguments as column names in R tidyverse
---

The `dplyr` [package](https://dplyr.tidyverse.org/) is extremely useful for data analysis and manipulation. In particular, the `group_by()` function allows you to view, count, and mutate your data based on your column of interest. Sometimes it's useful to be able to consider multiple columns - like if you have two columns that each describe a trait of your data. Ideally, instead of hard-coding these column names, you could write code that uses a variable that you then define as each of your columns of interest. 

To try this, we can create a sample dataframe. Here we'll make a list of flower types and a list of colors. Our dataframe will pair these flowers and colors. Then, we'll randomly assign each of the plants as "affected" (1) or "unaffected" (0) by some condition. 
```
# Set seed for reproducibility 
set.seed(123)

# Create lists of random data
flowers <- c("Rose", "Tulip", "Daisy", "Lily", "Sunflower")
colors <- c("Red", "Yellow", "White", "Pink", "Orange")

# Choose size of dataframe 
num_rows <- 100  

# Create dataframe with flower-color pairings 
# Note whether each is affected or not 
sample_data <- data.frame(
    flower = sample(flowers, num_rows, replace = TRUE),
    color = sample(colors, num_rows, replace = TRUE),
    affected = sample(0:1, num_rows, replace = TRUE)
)
```

If we inspect our data with `head(sample_data)`, the console should display:
```
> head(sample_data)
     flower  color affected
1     Daisy Orange        1
2     Daisy  White        0
3     Tulip    Red        0
4     Tulip Yellow        0
5     Daisy    Red        0
6 Sunflower Yellow        1
```

We can see that there are multiple colors of each flower: the first two rows are daisies, but one is orange and one is white. There are also multiple flowers of each color: rows 4 and 6 include yellow flowers, one of which is a tulip and one of which is a sunflower. We can also see that some of these flowers are affected (1), while others are not (0). 

We might be interested in how many flowers of each color are affected. To do this, we can use functions from the `dplyr` package to group by color and display how many of each color flower is affected: 
```
# Install package if you haven't before
install.packages("dplyr")

# Load package once installed
library(dplyr)

# Count affected flowers, by color 
by_color <- sample_data %>%
    group_by(color) %>%
    summarise(num_affected = sum(affected == 1))
```
If we view our table by typing `by_color` we'll see: 
```
> by_color
# A tibble: 5 × 2
  color  num_affected
  <chr>         <int>
1 Orange           10
2 Pink              4
3 Red               8
4 White             7
5 Yellow            9
```

We see 10 of the orange flowers are affected, 4 of the pink, etc. We can repeat the same thing for type of flower by changing the name of the column of interest from `color` to `flower`:

```
by_flower <- sample_data %>%
    group_by(flower) %>%
    summarise(num_affected = sum(affected == 1))
```
If we view our table by typing `by_flower` we'll see: 
```
> by_flower
# A tibble: 5 × 2
  flower    num_affected
  <chr>            <int>
1 Daisy                7
2 Lily                 7
3 Rose                 9
4 Sunflower            8
5 Tulip                7
```
Again, we see the number of affected, this time shown by type of flower (7 daisies were affected, 7 lillies were affected, etc.). 

Let's imagine we have multiple columns of descriptive data - maybe in addition to flower and color, we have size, date planted, its source (farmer's market, store bought), its origin (whether it was a seed or a bulb), etc. We're interested in how many plants are affected, based on each of these categories. We might want to make a function that lets us investigate each of these descriptive variables by taking the column of interest as an argument: 

```
# Define function where our arguments are the input data 
# And the column of interest
affected_by_column <- function(data, column_name) {
    output <- data %>% 
        group_by(column_name) %>%
        summarise(num_affected = sum(affected == 1))
}

# Call your function (for example, on the flower column):
output <- affected_by_column(sample_data, "flower")
```

If you try to run this, you'll get an error: 
```
Error in `group_by()`:
! Must group by variables found in `.data`.
✖ Column `column_name` is not found.
```

The function is treating the variable `column_name` as if it were the name of the actual column in the data. There is no column called "column_name"; our only three columns are "flower", "color", and "affected".

To instead communicate to the function that we want to use the **variable** column_name (i.e., the  string value that is assigned to it), we redefine our function, placing `column_name` within double brackets `{{}}`, to use the "curly-curly" operator: 
```
affected_by_column <- function(data, column_name) {
    output <- data %>% 
        group_by(`{{column_name}}`) %>%
        summarise(num_affected = sum(affected == 1))
}
```
This allows the code to execute the `group_by` function on the value assigned to the variable `column_name`, rather than the string "column_name" itself. This syntax is a `tidyeval` helper; check out the details [here](https://ggplot2.tidyverse.org/reference/tidyeval.html#:~:text=The%20curly%2Dcurly%20operator%20%7B%7B,..%20in%20the%20normal%20way.).

If we then call our function as usual (`output <- affected_by_column(sample_data, "color")`, however, we get an unexpected result. We think we'll get the same outcome as the first example above: 10 orange, 4 pink, etc. However, if you view `output` in your console you'll instead see: 
```
> output
# A tibble: 1 × 2
  `"color"` num_affected
  <chr>            <int>
1 color               38
```

It seems that now it's just counting all the rows that were affected. We can confirm this by manually counting the number of affected flowers, without using our function. This will tell us that there were 62 total flowers not affected, and 38 affected:  
```
table(sample_data$affected)
```

Instead, we want our function to use the **name** of the column when counting the number of affected flowers. To do this, we modify how we call our function, to include the `as.name()` function: 
```
output <- affected_by_column(sample_data, !!as.name("color"))
```
First, the `!!` operator is used for unquoting. Then the base R function `as.name()` function coerces its argument to a name. This allows it to be treated as a column name, rather than a string. 

This will also work if we define our variable `column_name` before we call the function: 
```
column_name <- "flower"
output <- affected_by_column(sample_data, !!as.name(column_name))
```

Beyond manually defining our variable `column_name`, we have a ton of options for getting our column of interest. We can accept it as an argument passed to our script from the command line: 
```
# Get command line arguments
args <- commandArgs(trailingOnly = TRUE)

# Assign column name as the first argument passed to the script
column_name <- args[1]
```

Or, after reading in the data, we could get the column names directly from the dataframe itself. In this example, we know that `affected` is the last column. We therefore grab all column names except the last one: 
```
cols_of_interest <- names(sample_data)[-length(names(sample_data))]
```
We can create an empty list to store our results and then call our function on each of these column names: 
```
# Create empty list for results 
output_list <- list()

# Get number of affected flowers, by each column  
for (i in cols_of_interest) {
    output <- affected_by_column(sample_data, !!as.name(i))
    output_list[[i]] <- output
}
```
Our output will look like the below, where we have one tibble for each column in our original data: 
```
> output_list
$flower
# A tibble: 5 × 2
  flower    num_affected
  <chr>            <int>
1 Daisy                7
2 Lily                 7
3 Rose                 9
4 Sunflower            8
5 Tulip                7

$color
# A tibble: 5 × 2
  color  num_affected
  <chr>         <int>
1 Orange           10
2 Pink              4
3 Red               8
4 White             7
5 Yellow            9
```

This combination of the `tidyeval` curly-curly `{{}}` operator along with the `!!as.name(variable)` syntax is useful for writing reproducible, reusable code for considering multiple columns of data while minimizing duplication. Additional wrappers (e.g., thorugh `map()`) could be applied futher streamlining consideration of multiple variables. Happy coding! 


