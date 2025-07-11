In this notebook we will cover the R basics that you need to know in order to do anything in R:

Data types

Data structures

Working with data frames

Let's get started right away.

Data Types

Numeric

In R, the numeric data type refers to real numbers, which can be either positive or negative and can have decimal places.

{r}
a <- 10.37
print(a)

In this example, a is a numeric variable. R automatically assigns the numeric data type when you use numbers with decimals.

You can use the class() function to check the data type of any variable.

{r}
class(a)

You can use various arithmetic operations with numeric values.

{r}
b <- a + 9.44
print(b)

For future reference, don't hesitate to create new code chunks at any point in an R notebook to test your own code! Just trying things out and seeing what happens is still the most effective way to learn. You can create new code chunks with Cmd + Option + I on macOS and Ctrl + Alt + I on Windows.

Integer

Integers are whole numbers (without decimal places). In R, you can explicitly define a variable as an integer by adding an L after the number.

{r}
c <- 7L
print(c)

Adding the L tells R that this value is an integer rather than a numeric (which is typically a floating-point number).

{r}
class(c)

What would be the class of c if you didn't add an L after the number? Write and execute some code to infer the answer to this question.

{r}
# Your code here
c_new <- 7
class (c_new)

You can convert a numeric value to an integer using the as.integer() function.

{r}
d <- 27.8
e <- as.integer(d)
print(e)

Note that when converting from numeric to integer, R truncates the decimal part.

When to Use Integer Instead of Numeric?

Using integer instead of numeric can be beneficial in certain scenarios. Integer values take up less memory compared to numeric values, which can be important when working with large datasets. Additionally, when working with functions or algorithms that require discrete values, using integer values ensures accuracy and avoids potential floating-point rounding errors.

Logical

The logical data type represents TRUE or FALSE values. Logical values are particularly useful when performing comparisons, controlling the flow of a program, or subsetting data frames.

{r}
f <- 5 > 3
print(f)

{r}
g <- 12 < 3
print(g)

{r}
h <- 100 == 100
print(h)

{r}
class(h)

Logical Operations

You can perform logical operations such as AND (&), OR (|), and NOT (!).

{r}
i <- TRUE
j <- FALSE

# AND
print(i & j)

# OR
print(i | j)

# NOT
print(!i)
print(!j)
print(i & !j)

Based on the outputs above, can you infer why each example gave either TRUE or FALSE? What is the logic behind these logical operators?

{r}
# Your written response here

# i & j → TRUE & FALSE → FALSE

#AND (&) returns TRUE only if both operands are TRUE.

#Here, since j is FALSE, the result is FALSE.

#i | j → TRUE | FALSE → TRUE

##Since i is TRUE, the result is TRUE.

#!i → !TRUE → FALSE

#NOT (!) negates the logical value.

#!TRUE becomes FALSE.

#!j → !FALSE → TRUE

#!FALSE becomes TRUE.

#i & !j → TRUE & TRUE → TRUE

#!j is TRUE, and i is also TRUE.

#TRUE & TRUE gives TRUE.

Character

The character data type is used to store text or strings. Character values must be enclosed in either single (') or double (") quotation marks.

{r}
course_name <- "Bare Minimum R"
print(course_name)

{r}
class(course_name)

Characters can be concatenated using the paste() function.

{r}
first_word <- "Bare"
second_word <- "Minimum"
third_word <- "R"
all_words <- paste(first_word, second_word, third_word)
print(all_words)

paste0() is a special version of paste() that does not automatically insert a space between each character being concatenated.

{r}
first_word <- "Bare"
second_word <- "Minimum"
third_word <- "R"
all_words <- paste0(first_word, second_word, third_word)
print(all_words)

Exercises

Create a numeric variable and convert it to an integer.

{r}
# Your code here
k <- 3.14
class (k)

k_new <- as.integer(k)
class(k_new)
print(k_new)

Create a logical variable that checks if 15 is greater than 20.

{r}
# Your code here
l <- 15 > 20
print(l)

Concatenate the strings "I", "will", "succeed." into a single variable and print it.

{r}
# Your code here
m <- "I"
n <- "will"
o <- "succeed."
p <- paste(m,n,o)

print (p)

Create an integer variable and use class() to confirm its type.

{r}
# Your code here
class(l) # logical/boolean
class(k) # numeric
class(k_new) # interger
class(p) # character/string

Write a logical expression to check if 100 is equal to 100.

{r}
# Your code here
q <- 100 ==100
print(q)

Convert a numeric value (e.g., 25.9) to an integer and observe the output.

{r}
# Your code here
r <- as.integer(25.9)
print(r)

Predict whether each of the following logical operations yield a TRUE or FALSE. Then run the code block below to check your answer.

{r}
x <- TRUE
y <- FALSE
z <- TRUE

print(x & z)      # TRUE
print(x & y)      # FALSE
print(x | z)      # TRUE
print(x | y)      # TRUE
print(x & y & z)  # FALSE
print(x | y | z)  # TRUE
print(x & !y & z) # TRUE
print(x | !z)     # TRUE
print(x & !z)     # FALSE
print(x & !y)     # TRUE

Predict whether each of the following logical operations yield a TRUE or FALSE. Then run the code block below to check your answer.

{r}
x <- "Bare Minimum R" == "BareMinimumR" #FALSE
y <- 17*3 == 51 #TRUE
z <- 17 != 18 #TRUE

print(x & z)      # F & T #FALSE
print(x & y)      # F & T #FALSE
print(x | z)      # TRUE
print(x | y)      # TRUE
print(x & y & z)  # FALSE
print(x | y | z)  # TRUE
print(x & !y & z) # FALSE
print(x | !z)     # FALSE
print(x & !z)     # FALSE
print(x & !y)     # FALSE

Data Structures

Vectors

A vector is a sequence of elements of the same data type. It can be a sequence of numbers, characters, or logical values. Vectors are created using the c() function.

{r}
# Numeric vector
num_vector <- c(1, 2, 3, 4, 5)

# Character vector
char_vector <- c("apple", "banana", "cherry")

# Logical vector
logical_vector <- c(TRUE, FALSE, TRUE)

print(num_vector)
print(char_vector)
print(logical_vector)

Vectors are one of the most commonly used data structures in R. You can access elements of a vector using square brackets.

{r}
# Accessing the second element of char_vector
char_vector[2]

# Accessing the second and third element of num_vector
num_vector[2:3]

Access the second element of logical_vector.

{r}
# Your code here
logical_vector[2]

Access the second through fifth element of num_vector.

{r}
# Your code here
num_vector[2:5]

Matrices

A matrix is a two-dimensional collection of elements of the same data type. You can think of it as a table with rows and columns. You can create a matrix using the matrix() function.

{r}
my_matrix <- matrix(1:9, nrow = 3, ncol = 3)
print(my_matrix)

The matrix() function takes a sequence of values (in this case, 1:9) and arranges them in a specified number of rows. You can access elements of a matrix using row and column indices.

{r}
# Accessing the third column
my_matrix[,3]

{r}
# Accessing the element in the second row and third column
my_matrix[2, 3]

Arrays

An array is similar to a matrix but can have more than two dimensions. You can create an array using the array() function.

{r}
my_array <- array(1:12, dim = c(3, 2, 2))
print(my_array)

Here, the dim argument specifies the dimensions of the array: 3 rows, 2 columns, and 2 layers.

You can access elements of an array using row, column, and layer indices.

{r}
# Accessing the second layer
my_array[, , 2]

{r}
# Accessing the first column in the second layer
my_array[, 1, 2]

{r}
# Accessing the element in the second row, first column, and second layer
my_array[2, 1, 2]

Lists

A list is a collection of elements that can be of different data types. Lists are very versatile because they can contain numbers, strings, vectors, or even other lists.

{r}
my_list <- list(name = "Alice", age = 30, scores = c(85, 90, 88))
print(my_list)

You can access elements of a list using the $ operator or double square brackets:

{r}
# Accessing elements of a list
my_list$name
my_list[["age"]]

How do you access the third element of scores in my_list?

{r}
# Your code here
my_list$scores[3]

Data Frames

A data frame is a two-dimensional data structure that can hold different data types. You can think of it as a table where each column can be of a different data type (but elements of the same column should still be of the same data type).

Data frames are created using the data.frame() function.

{r}
my_data_frame <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric = c(85, 90, 88)
)
print(my_data_frame)

You can see that different columns of the same data frame have be of different data types.

{r}
class(my_data_frame[,1])
class(my_data_frame[,2])

Of all of the data structures we have covered so far, vectors, lists, and data frames especially are going to be used very often.

Structure

Dimensionality

Homogeneous

Heterogeneous

Use Case

Vector

1D

✅

❌

Simple lists of same-type data

Matrix

2D

✅

❌

Tabular numeric/char data

Array

nD (≥1)

✅

❌

Higher-dimensional homogeneous data

List

1D

❌

✅

Grouping diverse objects

Data Frame

2D

❌

✅ (per column)

Tabular data with mixed types

Let's learn a bit more about what we can do with data frames.

Working with data frames

When we work with biological data as data frames, it is good practice to default to having each row correspond to an observation (patient, sample, cell, etc.) and each column correspond to a description/feature (height, age, expression level of Gene X, etc.) of the observations. So when we have a dataset with 1000 samples and 20 features, we should be envisioning a data frame with 1000 rows and 20 columns. If we use the first column to store the sample IDs, then we would a data frame with 1000 rows and 21 columns.

Let's create a new data frame to work with for this section.

{r}
df <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric1 = c(85, 90, 88)
)
print(df)

Accessing elements within a data frame is essential for data manipulation and analysis. There are multiple ways to access individual elements, rows, columns, or 2-dimensional subsets of a data frame.

Accessing Individual Elements

You can access individual elements of a data frame using row and column indices.

{r}
# Accessing the element in the first row and third column
element1 <- df[1,3]
print(element1)

{r}
# Accessing the element in the third row and column named "metric1"
element2 <- df[3, "metric1"]
print(element2)

Accessing Columns

Use the $ operator to access a single column.

{r}
time_points <- df$time_point
print(time_points)

Use double brackets [[ ]] with the column name to access a single column.

{r}
time_points <- df[["time_point"]]
print(time_points)

Use single brackets [ ] with the column number to access a single column.

{r}
time_points <- df[,2]
print(time_points)

Use single brackets [ ] with the column name to access a single column.

{r}
time_points <- df["time_point"]
print(time_points)

Note: Notice how of all the ways to access a data frame column above, using single brackets [ ] with the column name is the only way that returns a single-column data frame. Every other way returns a vector.

Access multiple columns.

{r}
subset_cols <- df[ ,c(2, 3)]
print(subset_cols)

Accessing Rows

Use single brackets [ ] with the row number to access a single row.

{r}
second_row <- df[2, ]
print(second_row)

Access multiple rows.

{r}
subset_rows <- df[c(1, 3), ]
print(subset_rows)

Accessing a 2-Dimensional Subset

To access a 2-dimensional subset of a data frame, specify both the row and column indices.

{r}
# Accessing the first and third rows and the second and third columns of the data frame
subset_df <- df[c(1,3), c(2,3)]
print(subset_df)

Adding and Removing Columns

Add a new column to the data frame called metric2.

{r}
df$metric2 <- c(2, 17, 9)
print(df)

Remove the metric2 column by assigning NULL to it.

{r}
df$metric2 <- NULL
print(df)

Concatenating Data Frames

Combining multiple data frames is a common task, especially when dealing with data from different sources or splitting data into manageable chunks. R provides several functions for concatenating data frames.

rbind() combines data frames by rows, stacking them vertically. Ensure that both data frames have the same columns with the same names before using rbind().

{r}
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

cbind() combines data frames by columns, placing them side by side. Ensure that both data frames have the same number of rows before using cbind().

{r}
df1 <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric1 = c(85, 90, 88)
)

df2 <- data.frame(
  metric2 = c(165, 175, 180),
  metric3 = c(0.7, 0.4, 0.2)
)

combined_df <- cbind(df1, df2)
print(combined_df)

Handling NAs

Missing values, NAs, are common in real-world biological datasets. Detecting and handling them properly will prevent your data frame manipulations from throwing unexpected errors.

{r}
df_with_na <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3", NA),
  time_point = c(4, NA, 16, 8),
  metric1 = c(85, 90, NA, 75)
)
print(df_with_na)

Taking the average of the time_point column returns an NA because one of the four values there is an NA. When we are not aware of NAs present in our data frame, we run into issues like this.

{r}
mean(df_with_na$time_point)

Use is.na() to identify NA's in a data frame.

{r}
na_matrix <- is.na(df_with_na)
print(na_matrix)

Remove rows with any NAs using na.omit().

{r}
df_no_na <- na.omit(df_with_na)
print(df_no_na)

Remove rows for which a specified column is NA.

{r}
# Remove rows where 'time_point' is NA
df_no_time_point_na <- df_with_na[!is.na(df_with_na$time_point), ]
print(df_no_time_point_na)

Replace NAs with more meaningful values.

{r}
df_with_na$sample_id[is.na(df_with_na$sample_id)] <- "Sample4"
print(df_with_na)

The %in% Operator

The %in% operator in R is used to test if elements of one vector are contained in another vector. It returns a logical vector (TRUE/FALSE) indicating whether each element of the first vector is found in the second vector.

In the following example, you can see that the second and third elements of vector1 are in vector2, while the first and fourth elements of vector1 are not in vector2.

{r}
vector1 <- c(1, 3, 5, 7)
vector2 <- c(2, 3, 5, 8, 10)

vector1 %in% vector2

Predict the logical vector outputted by the code block below. Then run the code block to check your predictions.

{r}
# c(F, T, F, T)
fruits1 <- c("apple", "banana", "cherry", "date")
fruits2 <- c("banana", "date")
fruits1 %in% fruits2

The %in% operator is very useful for subsetting data frames. In the code block below, we use it to select the exact rows of the data frame we want to keep.

{r}
df <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric1 = c(85, 90, 88)
)

samples_of_interest <- c("Sample1", "Sample3")

df_subset <- df[df$sample_id %in% samples_of_interest, ]
print(df_subset)

The same approach can of course also be used to subset specific columns of a data frame.

{r}
df <- data.frame(
  sample_id = c("Sample1", "Sample2", "Sample3"),
  time_point = c(4, 8, 16),
  metric1 = c(85, 90, 88)
)

columns_of_interest <- c("sample_id", "metric1")

df_subset <- df[,colnames(df) %in% columns_of_interest]
print(df_subset)

Next

We have not covered many other topics that are also fundamental to data analysis in R: factors, conditional statements, regular expressions, loops, and functions. They are not immediately necessary for this course and can be learned separately.

This concludes the notebook on R basics. Now you are ready to learn how to use dplyr for more ways to manipulate data in R. Proceed to "2_dplyr.qmd".
