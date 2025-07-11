
````markdown
# The tidyverse

The tidyverse is a collection of R packages designed for data science. These packages make it easy to import, tidy, transform, and visualize data, all with a consistent set of functions that work well together. The core packages in the tidyverse include dplyr and ggplot2. By learning how to use these two packages, you will gain essential skills for data manipulation and visualization in R and make your journey into computational biology more intuitive and enjoyable.

```r
# This simultaneously loads all the packages in the tidyverse collection, including dplyr and ggplot2.
library(tidyverse)
````

---

## dplyr

The current notebook focuses on dplyr as an extension to all the different ways to manipulate data frames introduced in the previous notebook. dplyr provides a set of intuitive and easy-to-use functions for manipulating and summarizing data in data frames.

Let's try out some of these functions with a toy dataset. `mtcars` is a toy dataset that is a part of the datasets package, which we have already loaded. `mtcars` contains information about different car models, including attributes like `mpg` (miles per gallon) and `hp` (horsepower).

```r
data <- mtcars
print(data)
```

---

### `select()` : Select specific columns of a data frame

Note: Multiple packages in R can have functions of the same name, which causes confusion. In that case, we have to specify the package that a function is from. For example, instead of using `select()`, we would use `dplyr::select()` to tell R that we are using the `select()` from dplyr and not from any other package.

```r
# Choose columns 'mpg' and 'cyl'
selected_data <- dplyr::select(data, mpg, cyl)
print(selected_data)
```

---

### `filter()` : Select specific rows of a data frame based on conditions

```r
# Choose rows where 'mpg' is greater than 20
filtered_data <- filter(data, mpg > 20)
print(filtered_data)
```

---

### `mutate()`: Add new columns by modifying existing ones

```r
# Add a new column 'kpl' (kilometers per liter) by modifying an existing column 'mpg'
mutated_data <- mutate(data, kpl = mpg * 0.425144)
print(mutated_data)
```

---

### `summarize()`: Summarize the data to get aggregate statistics

```r
# Calculate the minimum, average, and maximum 'mpg' for the entire dataset
summary <- summarize(data, min_mpg = min(mpg), avg_mpg = mean(mpg), max_mpg = max(mpg))
print(summary)
```

---

### `group_by()`: Group the data by one or more variables, usually used in conjunction with another function

```r
# Group data by 'gear'
grouped_data <- group_by(data, gear)
print(grouped_data)

# Calculate average 'mpg' for each group
summary <- summarize(grouped_data, avg_mpg = mean(mpg)) 
print(summary)
```

---

## The Pipe (`%>%`) Operator

The pipe operator `%>%` allows you to pass the output of one function directly into the next function. This makes your code more readable by eliminating the need for nested functions and by making the sequence of data manipulations more explicit.

For example, instead of writing:

```r
summarize(group_by(filter(data, mpg > 20), cyl), avg_mpg = mean(mpg))
```

You can write:

```r
filtered_data <- data %>%
  filter(mpg > 20) %>%
  group_by(cyl) %>%
  summarize(avg_mpg = mean(mpg))
print(filtered_data)
```

Using the pipe operator makes your code easier to read from top to bottom, as it mirrors the logical sequence of data transformation steps.

*A computational biologist will spend a lot more time reading than writing code, so insisting on readable code will save you a lot of headaches.*

---

## Exercises

**Select only the columns mpg and disp.**

```r
# Your code here
# head(data)
data_selected <- select(data, mpg, disp)
print(data_selected)
```

**Select only rows where cyl is equal to 6.**

```r
# Your code here
data_cyl <- data %>%
  filter(cyl == 6)
print(data_cyl)
```

**For cars with mpg greater than 20, I want to see only the mpg, cyl, and disp columns.**

```r
# Your code here
data_mpg20 <- data %>% 
  filter(mpg > 20) %>% 
  select(mpg, cyl, disp)
print(data_mpg20)
```

**Find the median disp for each value of cyl.**

```r
# Your code here
data_med_disp <- data %>% 
  group_by(cyl) %>% 
  summarize(median(disp))
print(data_med_disp)
```

**Calculate the mean mpg and the maximum hp for the entire dataset.**

```r
# Your code here
mean_mpg_hp <- data %>% 
  summarize(mean_mpg = mean(mpg),
            max_hp = max(hp))
print(mean_mpg_hp)
```

**Create a new column weight\_ton (wt / 2.205) and then select rows where weight\_ton is greater than 1.5.**

```r
# Your code here
data_weight <- data %>% 
  mutate(weight_ton = wt / 2.205) %>% 
  filter(weight_ton > 1.5)
print(data_weight)
```

**Find the average mpg for cars with hp greater than 100, grouped by cyl.**

```r
# Your code here
d <- data %>% 
  filter(hp > 100) %>% 
  group_by(cyl) %>% 
  summarize(mean_mpg = mean(mpg))
print(d)
```

**Group by cyl, create a new column hp\_to\_wt\_ratio (hp / wt) for each group, and calculate the maximum hp\_to\_wt\_ratio for each group.**

```r
# Your code here
result <- data %>% 
  group_by(cyl) %>% 
  mutate(hp_to_wt_ratio = hp / wt) %>% 
  summarize(max_hp_to_wt_ratio = max(hp_to_wt_ratio))

print(result)
```

**Group by cyl, select groups where the average mpg is greater than 20, and calculate the mean hp for the selected groups.**

```r
# Your code here
result <- data %>%
  group_by(cyl) %>%
  filter(mean(mpg) > 20) %>%
  summarize(avg_hp = mean(hp))

print(result)
```

---

## dplyr:: Short Review

* `select()` → Selects specific columns by name.
  Example: `select(data, mpg, hp)`

* `filter()` → Filters rows based on a condition.
  Example: `filter(data, mpg > 20)`

* `group_by()` → Groups the data by one or more variables, usually for use before `summarize()` or `mutate()`
  Example: `group_by(data, cyl)`

* `summarize()` → Reduces each group to summary statistics like `mean()`, `sum()`, `min()`, `max()`, etc.
  Example: `summarize(data, avg_mpg = mean(mpg))`

* `mutate()` → Adds new columns or modifies existing ones by applying transformations.
  Example: `mutate(data, hp_per_wt = hp / wt)`

---

## Next

We have not covered many other functions available in dplyr. They are not immediately necessary for this course and can be learned separately.

This concludes the notebook on dplyr. Now you are ready to learn how to use ggplot2! Proceed to `"3_ggplot2.qmd"`.

```
