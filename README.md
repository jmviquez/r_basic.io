---
title: "Introduction to R Basics"
output: html_document
editor_options: 
  chunk_output_type: inline
---

# Overview
This R Markdown file covers the basics of R programming, how to set up environments, explore data types, and execute basic functions with illustrative examples using popular R packages.

## Installation and Setup
```{r setup, include=TRUE, echo=TRUE}
# Install required packages
if (!requireNamespace("tidyverse", quietly = TRUE)) install.packages("tidyverse")
if (!requireNamespace("ggplot2", quietly = TRUE)) install.packages("ggplot2")
if (!requireNamespace("dplyr", quietly = TRUE)) install.packages("dplyr") 
```

## R Basic Data structures 


```{r, include=TRUE}
# Scalars, vectors, lists, and data frames
scalar <- 42
vector <- c(1, 2, 3, 4, 5)
list_example <- list(name = "R", version = 4.0, features = c("data", "graphics"))
data_frame_example <- data.frame(ID = 1:3, Value = c(10, 20, 30))

# Output types
class(scalar)
class(vector)
class(list_example)
class(data_frame_example)

```

## Basic arithmetic 
#### In its most basic form, R can be used as a simple calculator. Consider the following arithmetic operators:
#### **Addition**: `+`  
#### **Subtraction**: `-`  
#### **Multiplication**: `*`  
#### **Division**: `/`  
#### **Exponentiation**: `^`  
#### **Modulo**: `%%`  
```{r, include=TRUE}
addition <- 7 + 3
subtraction <- 7 - 3
multiplication <- 7 * 3
division <- 7 / 3



```

## Plots
```{r}
library(ggplot2)

# Example dataset
data <- data.frame(x = (1:10)^3, y = (1:10)^3)
#With the plot() funtion
#To change the type of plot can just need to change the letter in the type="x" with p as point and l as lines
#Scatter plot
plot(data$x,data$y,type="p")
##Line plot 
plot(data$x,data$y,type="l")
##Both
plot(data$x,data$y,type="b",col="blue")

##We can also stack the plots together
plot(data$x,data$y,type="p")
lines(data$x,data$y)
# Scatter plot
library(ggplot2)

ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  ggtitle("Scatter Plot of x and y^2")





```


```{r}
library(ggplot2)

# Example dataset
data <- data.frame(x = 1:10, y = (1:10)^2)

# Scatter plot
ggplot(data, aes(x = x, y = y)) +
  geom_point() +
  ggtitle("Scatter Plot of x and y^2")


```

### The same way we used the operators as a calculator, we can use them to perform operations on vectors and matrices.  


```{r}

## Vector Arithmetic with Visualization
# Creating vectors
vector_a <- c(1, 2, 3, 4, 5)
vector_b <- c(10, 20, 30, 40, 50)

# Element-wise operations
vector_sum <- vector_a + vector_b
vector_diff <- vector_b - vector_a
vector_prod <- vector_a * vector_b
vector_div <- vector_b / vector_a

# Scalar operations
vector_scaled <- vector_a * 2

# Combine results into a data frame for visualization
vector_results <- data.frame(
  Index = 1:length(vector_a),
  VectorA = vector_a,
  VectorB = vector_b,
  Sum = vector_sum,
  Difference = vector_diff,
  Product = vector_prod,
  Division = vector_div,
  Scaled = vector_scaled
)

vector_results
```

## Tables 

```{r}
##Create and use Table filters 


weather_data <- read.csv("weather.csv",header = T)

head(weather_data)

```

#### Tables work similarly to a matrix.  
#### We can access specific columns and rows using `[x, y]`.  

```{r}
Temperature<-weather_data[,10]
head(Temperature)
```

#### We can also use the names of the rows with the operator `$`.
```{r}
Station_state<-weather_data$Station.State
Station_prec<-weather_data$Data.Precipitation
head(Station_prec)
head(Station_state)
```

#### The vectors that we extract from the table can be formed into another table.  
#### We can do this either with `cbind()`  
#### Or, if we have different types of data, with `as.data.frame()`.  
```{r}
Extracted_dataframe<-as.data.frame(Station_prec,Station_state)
Extracted<-cbind(Station_prec,Station_state)
head(Extracted_dataframe)
head(Extracted)
```

#### The main difference is that a dataframe can handle more than one type of data without changing the data type.  
#### In a `cbind`, the data is transformed into characters, which is why the cells have quotation marks (" ").  

## Filtering Data  
#### Another option is using the `filter()` function from the **dplyr** package. With this function, we can create pivot tables based on the data we need, without the hassle of extracting rows and columns manually.  

#### Let's filter the stations that are in California.  

```{r}
library(dplyr)
Station_state<-filter(weather_data,Station.State=="California")
head(Station_state)
```

## Using Pipes for Data Manipulation

### What are Pipes?
Pipes (`%>%`) in R allow you to chain multiple operations together in a clean and readable way. They help simplify your code by removing the need for intermediate variables and nesting functions.

### Adding Pipes in RStudio
In RStudio, you can easily insert a pipe using the keyboard shortcut:
- **Windows/Linux**: `Ctrl + Shift + M`
- **Mac**: `Cmd + Shift + M`

### Benefits of Pipes
Combining pipes with functions like `filter()` from the `dplyr` package:
- Makes the code more concise and readable.
- Simplifies complex workflows.
- Avoids creating unnecessary intermediate variables.

### Example: Extracting Cities in Alaska with Precipitation > 1

Let’s use pipes to filter the `weather_data` dataset for cities in **Alaska** with precipitation values greater than `1` inch.


```{r}
library(dplyr)
high_precipitation_cities_pipe <- weather_data %>%
  filter(Data.Precipitation > 5)%>%
  filter(Station.State== "Alaska")
  

# View the filtered data
head(high_precipitation_cities_pipe)

# Bar plot of precipitation by city
ggplot(high_precipitation_cities_pipe, aes(x = reorder(Station.City, Data.Precipitation), y = Data.Precipitation, fill = Station.City)) +
  geom_bar(stat = "identity") +
  coord_flip() +  # Flip coordinates for readability
  ggtitle("Precipitation by City in Alaska (Data.Precipitation > 1)") +
  xlab("City") +
  ylab("Precipitation (inches)") +
  theme_minimal() +
  theme(legend.position = "none")  # Remove legend for clarity
```


