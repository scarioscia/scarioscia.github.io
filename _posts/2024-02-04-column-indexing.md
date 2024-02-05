---
layout: post
title: Using dplyr with variable column names
---

The `group_by()` function from the `dplyr` [package](https://dplyr.tidyverse.org/) is particularly helpful for viewing, counting, and mutating data based on a given column. Sometimes it’s useful to investigate the data based on multiple columns that each describe the data - ideally using a variable defined as each column of interest. In `dplyr`, we can leverage both base R and `tidyeval` to make this work.

To try it, we create a sample dataframe that pairs flower types and colors. We'll randomly assign each of the plants as "affected" (1) or "unaffected" (0) by some condition. 
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

There are multiple colors of each flower: the first two rows are daisies, but one is orange and one is white. There are also multiple flowers of each color: rows 4 and 6 include yellow flowers, one of which is a tulip and one of which is a sunflower. Some of these flowers are affected (1), while others are not (0). 

We might be interested in how many flowers of each color are affected. To do this, we can use functions from the `dplyr` package to group by color and display how many of each color is affected: 
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
The `by_color` table includes the number of affected flowers per color: 
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

10 of the orange flowers are affected, 4 of the pink, etc. A similar result is produced by changing `color` to `flower`: 
```
by_flower <- sample_data %>%
    group_by(flower) %>%
    summarise(num_affected = sum(affected == 1))
```
This time the number of affected plants is shown by type of flower (7 daisies were affected, 7 lillies, etc.): 
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

If there are multiple columns of descriptive data, it can help to define a function that takes the column of interest as an argument: 
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

However, running the above yields an error: 
```
Error in `group_by()`:
! Must group by variables found in `.data`.
✖ Column `column_name` is not found.
```

The function is treating the variable `column_name` as if it were the name of the actual column in the data. But there is no column called "column_name"!

To instead use the **variable** column_name (i.e., the string value that is assigned to it), you can redefine the function, placing `column_name` within curly brackets to use the `tidyverse` {% raw %}{{ {% endraw %} operator: 
{% raw %}
```
affected_by_column <- function(data, column_name) {
    output <- data %>% 
        group_by({{column_name}}) %>%
        summarise(num_affected = sum(affected == 1))
}
```
{% endraw %}

Here, the `group_by` function executes on the value assigned to the variable `column_name`, rather than the string "column_name" itself. This syntax is a `tidyeval` helper; check out the details [here](https://ggplot2.tidyverse.org/reference/tidyeval.html#:~:text=The%20curly%2Dcurly%20operator%20%7B%7B,..%20in%20the%20normal%20way.).

We anticipate calling the function as usual (`output <- affected_by_column(sample_data, "color")` will yield the same as the first example above: 10 orange, 4 pink, etc. However, `output` instead contains: 
```
> output
# A tibble: 1 × 2
  `"color"` num_affected
  <chr>            <int>
1 color               38
```

The function counted the affected flowers through all rows. We can confirm this by manually counting the number of affected flowers, without using our function: `table(sample_data$affected)` will show  62 total flowers not affected and 38 affected.

To instead make the function use the **name** of the column, we modify how we call our function, using the `as.name()` function: 
```
output <- affected_by_column(sample_data, !!as.name("color"))
```

The base R `as.name()` function creates a symbol named "color" to represent the variable name. The unquote operator, `!!`, captures the value of "color" and passes it to the function call, allowing it to be treated as a column name, rather than a string. 

This will also work if we define our variable `column_name` before we call the function: 
```
column_name <- "color"
output <- affected_by_column(sample_data, !!as.name(column_name))
```

Beyond manually defining the variable `column_name`, there are multiple options for identifying the column of interest. It can be passed to the script from the command line: 
```
# Get command line arguments
args <- commandArgs(trailingOnly = TRUE)

# Assign column name as the first argument passed to the script
column_name <- args[1]
```

The column names can also be identified directly from the dataframe itself. In this example, we know that `affected` is the last column. To grab all column names except the last one: 
```
cols_of_interest <- names(sample_data)[-length(names(sample_data))]
```
The function can then be called on each of the columns: 
```
# Create empty list for results 
output_list <- list()

# Get number of affected flowers, by each column  
for (i in cols_of_interest) {
    output <- affected_by_column(sample_data, !!as.name(i))
    output_list[[i]] <- output
}
```
The output will include one tibble for each column in the original data: 
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

This combination of the `tidyeval` curly-curly {% raw %}{{ {% endraw %} operator along with the `!!as.name(variable)` syntax is useful for considering multiple columns of data while minimizing code redundancy. Happy coding! 
