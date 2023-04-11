---
layout: post
title: Count repeated values in a dataframe
---

Lately I've been exploring large datasets by querying metadata files. In the metadata, each individual and has a unique identifier in the column `sample_id`. Some individuals are in the metadata multiple times, and I was curious about the distribution of those frequencies - are most people only in it once? What's the max? How many people are in it a large number of times? 

First, here's some R code to simulate a dataframe that mimics the metadata file I'm working with: 

```
# Set seed for reproducibility
set.seed(123)

# Create a vector of 20 unique random identifiers
sample_ids <- replicate(20, paste0(sample(LETTERS, 10, replace=TRUE), collapse=""))

# Sample 100 identifiers with replacement from the vector of 20 identifiers
sampled_ids <- sample(sample_ids, 100, replace=TRUE)

# Create a data frame with 100 rows and 4 columns
df <- data.frame(sample_id = sampled_ids,
                 val1 = runif(100),
                 val2 = runif(100),
                 val3 = runif(100))

# Print the first few rows of the data frame
head(df)
```

To see how many times an individual is in the dataset, in R, `table(df$sample_id)` will report each unique identifier in the column sample_id, along with the number of times it appears in the dataframe: 

```
DAHTYPXVKP GULOJMGIIJ HZGJISDNQK IGBPMSXTOG JVLTNQNVCH KYWHNUMBKM LYNCNGCWVZ MRAYYFUOIO NFYHLZDMNU NSOXQWKGOW NVYZESYYIC OSNCJRVKET OUEHSJRJLB QVRQBDMEVS THCDTLVQJT 
         7          3          6          5          2          1          9          5          5          6          6          6          4          5          5 
WKDLNSYGVZ WUGUFYBEHL YTVYNYWCHP ZFNGJEFPXU ZPTFKHVVGP 
         1          5          8          3          8 
```

This is useful by eye, as I can see for example that the individual with sample_id `DAHTYPXVKP` appears in the dataframe 7 times. But I also want to see how many individuals were in the dataset once, twice, 10 times, etc. To do that, I can do an additional table wrapper function: `table(table(df$sample_id))`. This returns the integer counts of appearances in the dataframe, along with the number of individuals making that number of appearances: 

```
1 2 3 4 5 6 7 8 9 
2 1 2 1 6 4 1 2 1 
```

In this example, we have two individuals present in the dataset one time; one individual present in the dataset four times; and six individuals present in the dataset 5 times. We also see that the maximum number of appearnces in the dataset was 9, and that it was only one individual who made 9 appearances in the dataframe. 

The most common number of appearances was 5: there are six individuals who appear in the dataframe five times. If we want to see those six individuals, we can execute `which(table(df$sample_id) == 5)`, which gives 1) each `sample_id` that appeared in the dataframe 5 times and 2) the position of that `sample_id` in the initial table command. Executing `which(table(df$sample_id) == 5)` yields: 
```
IGBPMSXTOG MRAYYFUOIO NFYHLZDMNU QVRQBDMEVS THCDTLVQJT WUGUFYBEHL 
         4          8          9         14         15         17 
```

If we just view `head(table(df$sample_id))` we'll see that the identifier `IGBPMSXTOG` is the fourth one: 
```
DAHTYPXVKP GULOJMGIIJ HZGJISDNQK IGBPMSXTOG JVLTNQNVCH KYWHNUMBKM 
         7          3          6          5          2          1
```

I'll use the nested `table()` command to investigate how many times individuals were present in a dataframe, including the minimum and maximum number of appearances for any individual. 
