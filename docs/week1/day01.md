
In this lesson, we will explore the basics of R, including its usage and advantages. You’ll also gain hands-on experience by writing code in your first RStudio notebook. Remember, as you progress through this lesson and the upcoming ones, that the best way to learn a new programming language is through trial and error and practical experience. Don’t worry if you encounter challenges with certain coding aspects—just being aware of what can be accomplished, even if you need to research how to do it, is a significant step forward!

## How and Why Do We Use R?

We are introducing R because it’s essential for your training as a bioinformatician. Over the past decade, R has gained immense popularity as a language for statistical computing and visualization, making it invaluable for various data analyses in biology and other fields.

Using R will facilitate collaboration with fellow bioinformaticians, as it has become a common tool in the community. Additionally, there’s a wealth of online support available to assist you, including blog posts and community forums. For instance, the R-bloggers site offers numerous blogs and tutorials to help you learn R effectively.

In summary, here are some of the top reasons why we use R:

- **Open Source and Cross-Platform**: R is freely available and can be used on various operating systems, making it accessible to a wide range of users.
- **Widely Used in the Scientific Community**: R has become a standard tool in many scientific fields, particularly in data analysis and statistics, which enhances collaboration among researchers and professionals.
- **Highly Extendable**: R offers a vast array of add-on packages that cater to different scientific tasks, allowing users to customize their analysis tools according to their specific needs.
- **Demand in the Job Market**: Employers are increasingly seeking candidates with R programming skills, as it is a key requirement for many data science roles

## Introduction to the R language

Much like Python, R is a general-purpose, interpreted language. This means it is not limited to solving a specific set of problems, and it does not require an additional compilation step.

You can run R code in several ways:

- **Directly in the Interpreter**: You can enter commands directly in an interactive R session.
- **From Saved Text Files (R Scripts)**: You can write and save your code in text files with a .R extension.
- **Using an Interface**: Tools like Jupyter Notebook or RStudio Notebooks provide a user-friendly environment for writing and executing R code.

In this course, we will primarily use RStudio Notebooks to guide you through learning the language and gaining practical experience. These notebooks offer structured work and enable you to receive timely feedback on your progress.

**Prerequisites**

Please download and install the following tools, selecting the appropriate version for your operating system (macOS, Windows, or Linux/UNIX):

1. **Install R**
   RStudio requires R version 3.6.0 or higher. Please download and install the version of R that matches your operating system:
   <https://cran.rstudio.com/>

2. **Install RStudio**
   Once R is installed, download and install RStudio:
   <https://posit.co/download/rstudio-desktop/>

All of the exercise data is stored in a GitHub repository. To download it:

1. Open your terminal.

2. Clone the repository by running:

```bash
git clone https://github.com/gzhoubioinf/IDE_RTutorial.git
```

3. Change to the tutorial directory:

```bash
cd IDE_Rtutorial
```

You’re now ready to start the exercises using the files you’ve just downloaded.

## Fundamentals of R Data Storage and Manipulation

One of the most fundamental aspects of computer programming is understanding how a programming language enables a computer to store and manipulate data. Like what you’ve learned in Python, data can take various forms, such as:

- **Numerical**: This includes integers (whole numbers) and decimal (floating-point) numbers.
- **Text**: Commonly referred to as “string” data.
- **Graphical Data**: Visual representations of data.
- **Program Data**: The actual code that you want to run.

The way this data is stored in the computer’s memory and accessed is quite similar between Python and R. Therefore, everything you’ve learned about data types, data objects, variables, and their values is applicable here. Like in Python, you can define variables in your code and assign values to them using the appropriate operators. However, you will notice that the syntax (the set of rules governing how your code is structured) often differs slightly between R and Python.

## Storing and Manipulating Data in R

Storing and manipulating data in R is quite straightforward. You can create and store an object using the assignment operator `<-` as follows:

```r
my_integer <- 7
my_decimal <- 10.2
my_string <- "hello"
```
In this code, each line creates an object in memory, sets its value, and assigns a label (or variable name) to it. The above examples illustrate some basic data types available in R: an integer (whole number), a floating-point (decimal) number, and a string (text).

### Assignment in R

In R, variable assignment is conventionally done using the leftward operator `<-`. You can think of the arrow as “pushing” the value into the variable. While you can also use the equals operator `=` (as in Python), it’s recommended to use `<-` for variable assignment in R.

### Naming Variables

You can choose almost any name for your variables, provided they contain only letters, numbers, and underscores (no spaces or punctuation). Like Python, R is case-sensitive, meaning `myvar` and `MyVar` are considered different variables, although both are valid.

To manipulate your stored information in R, you have access to several operators:

### Operators

- **Assignment**: `a <- b`
  - Assigns the value of `b` to variable `a`.
- **Arithmetic Operations**:
  - `a + b`: Addition
  - `a - b`: Subtraction
  - `a * b`: Multiplication
  - `a / b`: Division
- **Exponentiation**:
  - `a ** b`: Returns `a` raised to the power of `b`.
- **Modulus**:
  - `a %% b`: Returns the remainder after dividing `a` by `b`.
- **Array Indexing**:
  - `a[b]`: Returns the `b`th element of the collection of objects given by `a`.

### Printing Variables

You can print out variables using the `print()` function, like Python:

```r
my_val <- 7
print(my_val)
```
However, you can also just list the variable by name, and when you run the code, R will automatically print its value.

### Storing Results

You can store the results of operations and calculations using assignment and chain them together as needed:

```r
myval <- 7
myval  # This will print the value of myval

myval2 <- myval * 10 * myval
myval3 <- myval2 / 5

# Print the values
myval
myval2
myval3
```
In this example, `myval` is assigned the value of 7, and the subsequent operations calculate `myval2` and `myval3`. Each variable can be printed by simply typing its name, allowing you to view the results directly in your R session.

## Introduction to RStudio and R Notebooks

Now you can start using RStudio and writing your first code with the R language. This includes how to fill out your RStudio notebook with a combination of code and informative text, how to run the code in your notebook, and how to save your work.

## Key Takeaways

- RStudio is a user-friendly environment in which to develop your code.
- You can develop R scripts (`*.R`) to build analyses that are permanent and repeatable.
- R Notebooks (`*.Rmd`) provide a convenient way to develop your code in a way that is permanent, repeatable, annotated, and shareable.

## Collections of Objects in R

In R, there are several important data types that serve as collections or containers for storing and manipulating data. Here’s a brief overview of five key data types and how to access their elements:

### 1. Vector

- **Description**: A one-dimensional set of items of the same type.

- Creating a Vector

```r
my_vector <- c(1, 2, 3, 4)
```
- Accessing Elements

```r
my_vector[1]  # Access the first element
my_vector[c(1,4)] # Takes the 1st and 4th elements of a vector.
my_vector[2,4] 
```

### 2. Matrix

- **Description**: A two-dimensional set of items of the same type.

- Creating a Matrix

```r
my_matrix <- matrix(1:6, nrow = 2, ncol = 3)
```
- Accessing Elements

```r
my_matrix[1, 2]  # Access the element in the first row and second column
```

### 3. List

- **Description**: A one-dimensional set of items that may be of different types.

- Creating a List

```r
my_list <- list(name = "Alice", age = 25, scores = c(90, 85, 88))
```
- Accessing Elements

```r
my_list$name  # Access the 'name' element
my_list$scores
my_list[[3]]
my_list[["name"]]
```

### 4. Array

- **Description**: An any-dimensional matrix.

- Creating an Array

```r
my_array <- array(1:12, dim = c(2, 3, 2))  # 2x3x2 array
```
- Accessing Elements

```r
my_array[1, 2, 2]  # Access the element in the first row, second column, second layer
```

### 5. Data Frame

- **Description**: Tables of items of different types, which can be given names (similar to a spreadsheet).

- Creating a Data Frame

```r
my_data_frame <- data.frame(Name = c("Alice", "Bob"), Age = c(25, 30))
```
- Accessing Elements

```r
my_data_frame$Name  # Access the 'Name' column
my_data_frame[1, ]  # Access the first row
```

# Large Data Frame and Subsetting  
y <- matrix(1:60000, nrow = 200, ncol = 300)  
df <- data.frame(y)  
head(df)  
df$X1  
df[, 1]  
df[1, ]  
df[20:23, c(3, 5)]

These data types allow you to organize and manipulate data effectively in R, making it easier to perform analyses and visualize results.

#### Key Difference Between R and Python

One of the most important distinctions between R and Python is **indexing**:

- **R**: R uses **one-based indexing**, meaning that the first element in a collection is accessed with index **1**.

```r
my_vector <- c(10, 20, 30)
my_vector[1]  # This returns 10
```
- **Python**: Python uses **zero-based indexing**, meaning that the first element is accessed with index **0**.

```python
my_list = [10, 20, 30]
my_list[0]  # This returns 10
```

### Checking Data Types in R

R provides several functions to check the type of your data and obtain useful information about it. Here’s how to use these functions effectively.

```r
# Check if my_matrix is a matrix (returns TRUE)
is.matrix(my_matrix)

# Get the class of my_matrix (returns "matrix" or "array")
class(my_matrix)

# Get the type of data inside my_matrix (returns "double")
typeof(my_matrix)

# Get summary statistics for my_matrix (min, max, mean, etc.)
summary(my_matrix)
```

#### Functions Explained

- `is.matrix()`: Checks if the object is a matrix and returns `TRUE` or `FALSE`.
- `class()`: Returns the class of the object, which tells you what kind of R object you are working with, such as `"matrix"`, `"data.frame"`, or `"list"`.
- `typeof()`: Provides the internal type of the data contained in the object, such as `"double"`, `"integer"`, or `"character"`.
- `summary()`: Generates a summary of the data, including statistics like minimum, maximum, and mean.

### Converting Data Types

If you find that the data type isn’t what you expected, you can convert it using coercion functions:

```r
# Convert a double to an integer (output is a single number)
as.integer(3.4)  # Output: [1] 3

# Turn a matrix into a data frame
my_new_frame <- as.data.frame(my_matrix)
as.Date("21/03/2016", format = "%d/%m/%Y")
```

#### Key Points

- **Coercion**: Use functions like `as.integer()` and `as.data.frame()` to convert data types, which is essential for ensuring compatibility in data analysis.
- **Understanding Data**: Checking the data type and structure helps in data manipulation and ensures that the functions you plan to apply will work correctly.

These tools are fundamental for effective data management and analysis in R, allowing you to work confidently with different data types.

### Using External Code in R

Reusing code is a vital programming skill, and R has various ways to leverage external code, which can greatly enhance your efficiency and productivity. Here are some methods to use external code in R effectively.

#### Methods to Use External Code

1. **Packages**:

```r
- R has a rich ecosystem of packages available through CRAN (Comprehensive R Archive Network).
- You can install and load packages that contain pre-written functions for specific tasks.

# Install a package 
install.packages("tidyverse")

# Load the package into your R session
library(tidyverse)
```
2. **Source Files**:

```r
- You can save R code in a script file (e.g., `my_script.R`) and run it from your R session.

# Source an external R script
source("my_script.R")
```
3. **Functions**:

- You can define your own functions in one R file and reuse them across different projects by sourcing the file.
### In-built Functions in R

In R, functions are fundamental building blocks that allow you to perform specific tasks without rewriting code. You can call these functions to execute predefined operations, making your code more efficient and easier to read.

#### Calling Functions

To call a function in R, you use the function’s name followed by parentheses. If the function requires arguments, you provide them within the parentheses, separated by commas. If no arguments are needed, the parentheses are still required.

**Calling a Function with Arguments**:

```r
# Calculate the mean of a numeric vector
my_vector <- c(1, 2, 3, 4, 5)
mean_value <- mean(my_vector)  # Calls the mean function
print(mean_value)  # Output: 3
```
**Calling a Function Without Arguments**:

```r
# Generate a random number
random_number <- runif(1)  # Calls the runif function to generate one random number
print(random_number)
```

#### Example of a Custom Function

You can also create your own functions in R:

```r
# Function to Multiply Input by 2
multiplier <- function(x) {
  return(x * 2)
}
multiplier(3)

# Function to Double Data Frame and Return Last Row
report_the_last_double <- function(df) {
  df <- df * 2
  return(tail(df, 1))
}

df <- data.frame(matrix(1:6, 2, 3))
last <- report_the_last_double(df)
print(last)
```

# Data Analysis in R

## Reproducible Data Analysis in R

In this lesson, we will focus on preparing, importing, and manipulating a dataset in R. We will be using the Compensation dataset, which describes the yield of apples based on different grafting and grazing conditions.

### Preparing Your Data

Before importing data into R, ensure that it is in a compatible format. Common formats include CSV (Comma-Separated Values) and Excel files. For this example, we’ll assume the dataset is in CSV format.

### Importing Data into R

To import data, you can use the `read.csv()` function for CSV files. Here’s how to do it:

```r
# Read the Compensation dataset from a CSV file
compensation_data <- read.csv("path/to/your/Compensation.csv")

# Display the first few rows of the dataset
head(compensation_data)
```

## Basic Data Manipulation

Once the data is imported, you can perform various manipulations to prepare it for analysis. Here are some common operations:

1. **Inspecting the Data**: Use functions to understand the structure and summary of the dataset.

```r
# Check the structure of the dataset
str(compensation_data)

# Get summary statistics
summary(compensation_data)
```
2. **Filtering Data**: You can filter rows based on specific conditions. For example, if you want to analyze yields from grazed orchards:

```r
grazed_data <- compensation_data[compensation_data$Grazing == "Grazed", ]
```
3. **Creating New Variables**: You can create new variables based on existing ones. For example, if you want to calculate Fruit yield per Root:

```r
compensation_data$Fruit_per_Root <- compensation_data$Fruit / compensation_data$Root
```
4. **Aggregating Data**: Use functions like `aggregate()` to summarize data.

```r
# Calculate average yield for each rootstock width
average_yield <- aggregate(Fruit ~ Grazing, data = compensation_data, FUN = mean)
```

### How to Import Your Data into R

Importing data into R can be accomplished through several methods, but in this course, we will focus on using a couple of lines of code with the `readr` package. Here’s a step-by-step guide on how to do it.

#### Method Overview

1. **Import Dataset Tool in RStudio**: A graphical interface to import data.
2. `file.choose()` **Function**: Allows you to interactively choose a file.
3. **Set Working Directory in RStudio**: Specify the folder where your data files are located.
4. **Same Location for Data File and Script**: This is the method we will use in this course.

### Steps to Import Data Using `readr`

#### 1. Load the Necessary Library

Before importing your data, you must load the `readr` library, which provides functions for reading data files.

```r
# Load the library needed for reading in files
install.packages('readr')
library(readr)
```

#### 2. Import Your Data

Assuming your CSV file is in the same directory as your R script, you can read the data using `read_csv()`:

```r
# Read in your chosen file and assign it to a variable name
my_data <- read_csv("Compensation.csv")
```

### Checking Your Imported Data in R

After importing your data into R, it’s crucial to verify that everything has been read correctly. Just because there are no error messages doesn’t guarantee that the data is as expected. Here’s a checklist of things to assess, along with useful R functions to help you.

#### 1. Verify Data Frame Structure

First, ensure that your data has been correctly imported as a data frame:

```r
# Check if the imported data is a data frame
is.data.frame(my_data)  # Should return TRUE
```

#### 2. Check Dimensions

Confirm that your data has the expected number of rows and columns:

```r
# Get the number of rows and columns
dim(my_data)  # Returns a vector: c(number of rows, number of columns)
```
You can also use:

```r
# Check the number of rows and columns separately
nrow(my_data)  # Number of rows
ncol(my_data)  # Number of columns
```

#### 3. Inspect the Data

To quickly view the first few rows of your dataset, use:

```r
# View the first few rows of the dataset
head(my_data)  # Displays the first 6 rows by default
```
If you want to see more rows, you can specify a number:

```r
# View the first 10 rows
head(my_data, 10)
```

#### 4. Check Variable Types

Ensure that your variables are recognized correctly (e.g., numerical, categorical):

```r
# Check the structure of the dataset
str(my_data)  # Displays the data types of each column
```

#### 5. Check Levels of Categorical Variables

For categorical variables, verify that the number of levels is as expected:

```r
# Check the levels of a categorical variable
levels(as.factor(my_data$YourCategoricalVariable))
```
Replace `YourCategoricalVariable` with the name of your categorical column.

#### 6. Summary Statistics

Get a summary of your data to check for anomalies, such as unexpected NA values:

```r
# Get summary statistics for each variable
summary(my_data)
```

#### 7. Check Column Names

To see the names of each column (your variable names):

```r
# Check the names of each column
names(my_data)
```

#### 8. View Data in RStudio

For a more user-friendly view, you can open your dataset in a new tab in RStudio:

```r
# View your data in a table format
View(my_data)
```

### Subsetting Your Data in R

Subsetting your data is an essential skill that allows you to focus on specific rows or columns that meet certain criteria. The `dplyr` package provides intuitive functions to help with this process.

#### Selecting Columns by Name

You can use the `select()` function to choose specific columns from your dataset. Here’s how:

```r
# Select specific columns by name
selected_data <- select(my_data, Column1, Column2)
```
This command returns a data frame containing only the specified columns.

#### Excluding Columns

You can also exclude certain columns using a negative index within the `select()` function. This is useful when you want to keep all columns except a few.

```r
# Select all columns except the specified one
subset_data <- select(my_data, -ColumnToExclude)
```
**Example**

```r
# Jiayi and Mohammed’s Measurements
name <- c(rep("Jiayi", 10), rep("Mohammed", 10))
clades <- rep(c(rep("CladeA", 5), rep("CladeB", 5)), 2)
measurements <- c(
  0.3, 0.8, 0.7, 0.7, 0.3,
  0.2, 0.4, 0.1, 0.4, 0.9,
  0.23, 0.4, 0.8, 0.75, 0.9,
  0.32, 0.98, 0.54, 0.2, 0.1
)

df <- data.frame(name = name, clades = clades, measurement = measurements)
write_csv(df, "measurements.csv")

# Read and process the file
df <- read_csv("measurements.csv")

# Subset vectors
vector_1 <- unlist(df[df$name == "Mohammed" & df$clades == "CladeA", 3])
vector_2 <- unlist(df[df$name == "Mohammed" & df$clades == "CladeB", 3])
vector_3 <- unlist(df[df$name == "Jiayi" & df$clades == "CladeA", 3])
vector_4 <- unlist(df[df$name == "Jiayi" & df$clades == "CladeB", 3])

# Compute means
mean_v1 <- mean(vector_1)
mean_v2 <- mean(vector_2)
mean_v3 <- mean(vector_3)
mean_v4 <- mean(vector_4)

# Statistical tests
test_result_CladeA <- wilcox.test(vector_1, vector_3)
print(ifelse(test_result_CladeA$p.value < 0.05, "Significant Difference", "Not Significant"))

test_result_CladeB <- wilcox.test(vector_2, vector_4)
print(ifelse(test_result_CladeB$p.value < 0.05, "Significant Difference", "Not Significant"))

# Visualization
ggplot(df, aes(x = clades, y = measurement, fill = name)) +
  geom_boxplot() +
  theme_minimal() +
  theme(
    axis.text.x = element_text(size = 10, angle = 45, hjust = 1),
    axis.text.y = element_text(size = 13, hjust = 1),
    axis.title.x = element_text(color = "black", size = 15, face = "bold"),
    axis.title.y = element_text(color = "black", size = 15, face = "bold"),
    strip.text.x = element_text(size = 15, color = "black", face = "bold")
  )
```
