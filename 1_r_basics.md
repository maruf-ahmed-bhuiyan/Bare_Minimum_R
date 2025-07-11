
````markdown
# 🧬 R Basics Notebook – *Bare Minimum R*

In this notebook, we will cover the R basics that you need to know in order to do anything in R:

- Data types
- Data structures
- Working with data frames

Let’s get started right away.

---

## 🔢 Data Types

### Numeric

In R, the numeric data type refers to real numbers (decimal or whole numbers).

```r
a <- 10.37
print(a)
````

Check its class:

```r
class(a)
```

You can do arithmetic:

```r
b <- a + 9.44
print(b)
```

### Integer

Use `L` to explicitly define an integer:

```r
c <- 7L
print(c)
class(c)
```

What if you don’t add the `L`?

```r
c_new <- 7
class(c_new)
```

Convert numeric to integer:

```r
d <- 27.8
e <- as.integer(d)
print(e)
```

💡 **Note**: `as.integer()` truncates the decimal part.

---

### Logical

Logical values are `TRUE` or `FALSE`.

```r
f <- 5 > 3
g <- 12 < 3
h <- 100 == 100

print(f)
print(g)
print(h)
class(h)
```

#### Logical Operators

```r
i <- TRUE
j <- FALSE

print(i & j)   # AND
print(i | j)   # OR
print(!i)      # NOT
print(!j)
print(i & !j)
```

🧠 Explanation:

```r
# i & j → FALSE
# i | j → TRUE
# !i   → FALSE
# !j   → TRUE
# i & !j → TRUE
```

---

### Character

Character data holds strings:

```r
course_name <- "Bare Minimum R"
print(course_name)
class(course_name)
```

Concatenate strings:

```r
first_word <- "Bare"
second_word <- "Minimum"
third_word <- "R"

all_words <- paste(first_word, second_word, third_word)
print(all_words)
```

Without spaces:

```r
all_words <- paste0(first_word, second_word, third_word)
print(all_words)
```

---

## 📝 Exercises

```r
# Convert numeric to integer
k <- 3.14
k_new <- as.integer(k)
print(k_new)
class(k_new)

# Logical: 15 > 20
l <- 15 > 20
print(l)

# Concatenate strings
m <- "I"
n <- "will"
o <- "succeed."
p <- paste(m, n, o)
print(p)

# Check variable types
class(l)      # logical
class(k)      # numeric
class(k_new)  # integer
class(p)      # character

# Check equality
q <- 100 == 100
print(q)

# Convert float to integer
r <- as.integer(25.9)
print(r)
```

Logical predictions:

```r
x <- TRUE
y <- FALSE
z <- TRUE

print(x & z)
print(x & y)
print(x | z)
print(x | y)
print(x & y & z)
print(x | y | z)
print(x & !y & z)
print(x | !z)
print(x & !z)
print(x & !y)
```

More logic practice:

```r
x <- "Bare Minimum R" == "BareMinimumR" # FALSE
y <- 17*3 == 51                         # TRUE
z <- 17 != 18                           # TRUE

print(x & z)
print(x & y)
print(x | z)
print(x | y)
print(x & y & z)
print(x | y | z)
print(x & !y & z)
print(x | !z)
print(x & !z)
print(x & !y)
```

---

## 📦 Data Structures

### Vectors

```r
num_vector <- c(1, 2, 3, 4, 5)
char_vector <- c("apple", "banana", "cherry")
logical_vector <- c(TRUE, FALSE, TRUE)

print(num_vector)
print(char_vector)
print(logical_vector)
```

Access vector elements:

```r
char_vector[2]
num_vector[2:3]
logical_vector[2]
num_vector[2:5]
```

---

### Matrices

```r
my_matrix <- matrix(1:9, nrow = 3, ncol = 3)
print(my_matrix)

my_matrix[,3]
my_matrix[2,3]
```

---

### Arrays

```r
my_array <- array(1:12, dim = c(3, 2, 2))
print(my_array)

my_array[,,2]
my_array[,1,2]
my_array[2,1,2]
```

---

### Lists

```r
my_list <- list(name = "Alice", age = 30, scores = c(85, 90, 88))
print(my_list)

my_list$name
my_list[["age"]]
my_list$scores[3]
```

---

### Data Frames

```r
my_data_frame <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric = c(85, 90, 88)
)
print(my_data_frame)

class(my_data_frame[,1])
class(my_data_frame[,2])
```

---

## 📊 Structure Comparison

| Structure  | Dimensionality | Homogeneous | Heterogeneous | Use Case                      |
| ---------- | -------------- | ----------- | ------------- | ----------------------------- |
| Vector     | 1D             | ✅           | ❌             | Same-type lists               |
| Matrix     | 2D             | ✅           | ❌             | Numeric/character tables      |
| Array      | nD (≥1)        | ✅           | ❌             | Higher-dimensional data       |
| List       | 1D             | ❌           | ✅             | Mixed-type collections        |
| Data Frame | 2D             | ❌           | ✅ (per col)   | Tabular data with mixed types |

---

## 🔧 Working with Data Frames

```r
df <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric1 = c(85, 90, 88)
)
```

Access individual elements:

```r
df[1, 3]
df[3, "metric1"]
```

Access columns:

```r
df$time_point
df[["time_point"]]
df[,2]
df["time_point"]
df[,c(2,3)]
```

Access rows:

```r
df[2, ]
df[c(1,3), ]
df[c(1,3), c(2,3)]
```

Add/remove columns:

```r
df$metric2 <- c(2, 17, 9)
df$metric2 <- NULL
```

Concatenate data frames:

```r
df1 <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric = c(85, 90, 88)
)

df2 <- data.frame(
  sample_id = c("Sample4", "Sample5"),
  time_point = c(16, 4),
  metric = c(75, 68)
)

combined_df <- rbind(df1, df2)
print(combined_df)
```

```r
df2 <- data.frame(
  metric2 = c(165, 175, 180),
  metric3 = c(0.7, 0.4, 0.2)
)

combined_df <- cbind(df1, df2)
print(combined_df)
```

---

## 🚫 Handling NAs

```r
df_with_na <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3", NA),
  time_point = c(4, NA, 16, 8),
  metric1 = c(85, 90, NA, 75)
)

mean(df_with_na$time_point)         # returns NA
is.na(df_with_na)                   # logical NA matrix
df_no_na <- na.omit(df_with_na)     # drop all rows with any NA
df_no_time_point_na <- df_with_na[!is.na(df_with_na$time_point), ]

df_with_na$sample_id[is.na(df_with_na$sample_id)] <- "Sample4"
```

---

## 🔍 The `%in%` Operator

```r
vector1 <- c(1, 3, 5, 7)
vector2 <- c(2, 3, 5, 8, 10)
vector1 %in% vector2
```

Example with characters:

```r
fruits1 <- c("apple", "banana", "cherry", "date")
fruits2 <- c("banana", "date")
fruits1 %in% fruits2
```

Subset data frame rows:

```r
df <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric1 = c(85, 90, 88)
)

samples_of_interest <- c("Sample1", "Sample3")
df_subset <- df[df$sample_id %in% samples_of_interest, ]
```

Subset columns:

```r
columns_of_interest <- c("sample_id", "metric1")
df_subset <- df[, colnames(df) %in% columns_of_interest]
```

---

## ▶️ Next Steps

We have not covered topics such as:

* Factors
* Conditional statements
* Loops and functions
* Regular expressions

These are useful but not essential for your first R analysis.

👉 Proceed to the next notebook: **`2_dplyr.qmd`**

````
