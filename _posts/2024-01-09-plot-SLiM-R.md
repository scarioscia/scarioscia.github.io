---
layout: post
title: Plotting outputs from SLiM simulations in R
---

Some of the most fun part of any analysis is data visualization! Once you've written your SLiM outputs to file (see [post #1](https://scarioscia.github.io/2024-01-05/writing-SLiM-output)) and defined your variables via the command line (see [post #2](https://scarioscia.github.io/2024-01-07/command-line-SLiM)), you can investigate the trends in your simulations, determine the effects of changing parameters, and visualize these outcomes through a scripting language such as R. 

The basic units of an R script are detailed on our course [site](https://andrew-bortvin.github.io/slimNotes/week-4-assignment.html). This page also includes a tutorial in the major package for plotting in R, `ggplot`. 

The first time you use a package in R, you'll need to install it using the `install.packages()` command. Once it's installed on your machine, you can then load the package using the `library()` command. For example, if you're interested in the data visualization package `ggplot`, you would execute `install.packages("ggplot2")` and then `library(ggplot2)`.

To get started, install the packages if necessary and then load `dplyr` and `ggplot`:

```
# load libraries
# package for the `bind_rows` function, dplyr
library(dplyr)
# package for data visualization, ggplot2
library(ggplot2)
```

To begin interacting with your data in R, first read it into your script. You'll need to include both the filename and its filepath (either absolute on your machine, or relative to where you're writing your script). If you were interested in reading in just a single file from your simulation, you could read it in via: 
```
myfile <- read.csv("slim/p1_AF_trial1.csv")
```

However, you may want to generate numerous simulation outputs and compare them all on a single plot. For example, perhaps you ran the command `slim -d trialNumber=1 scratch.slim` four times, changing the `trialNumber` variable to values from 1 through 4. To load these files, you can read them in to a single data structure (here called a `dataframe`) through a `for loop`:

```
# Create an empty data frame to store the combined data
combined_data <- data.frame()

# Loop through the files and read them into the list
for (i in 1:4) {
    fname <- paste0("slim/p1_AF_trial", i, ".csv")
    varname <- read.csv(fname)
    combined_data <- bind_rows(combined_data, varname)
}
```

From there, you can then plot the data in any structure (line plot, scatter plot, etc.) that you think will best fit your information and answer your question of interest. You can visualize the different trial results by color or by shape; check out the `ggplot` tutorial on our course page for other possibilities. 

```
# Plot the four trials together 
# color by trial, line type by population 
ggplot(data = combined_data, 
       aes(x = Generation, y = Allele.Frequency, color = as.factor(Trial))) + 
    geom_line(aes(linetype=as.factor(Population)))
```

Good luck, and let us know what you think! 
