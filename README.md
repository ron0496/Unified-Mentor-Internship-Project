# Project 1: Supply Chain Management Dashboard

### About Project:
Supply chain analytics is a valuable part of data-driven decision-making in various industries such as manufacturing, retail, healthcare, and logistics. It is the process of collecting, analyzing and interpreting data related to the movement of products and services from suppliers to customers. The objective of this project is to create a comprehensive Supply Chain Management (SCM) Dashboard using Tableau. This dashboard will help monitor and optimize various aspects of the supply chain, including inventory levels, order fulfillment, supplier performance, transportation efficiency, and overall supply chain costs.

### Below are all the features in the dataset:

* Product Type
* SKU 
* Price
* Availability
* Number of products sold
* Revenue generated
* Customer demographics
* Stock levels
* Lead times 
* Order quantities 
* Shipping times 
* Shipping carriers 
* Shipping costs 
* Supplier name 
* Location 
* Lead time 
* Production volumes 
* Manufacturing lead time 
* Manufacturing costs 
* Inspection results 
* Defect rates 
* Transportation modes 
* Routes 
* Costs

[Dashboard Link](https://public.tableau.com/views/supplychaindashboard_17285713065780/SupplyChainEfficiencyDashboard?:language=en-GB&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

### Supply Chain Efficiency Dasboard:

<img width="1728" alt="Screenshot 2024-10-10 at 11 02 58 PM" src="https://github.com/user-attachments/assets/38c27a78-ec17-4aca-a2b0-2007140e4c2c">

### Product Performance Dashboard

<img width="1728" alt="Screenshot 2024-10-10 at 10 18 59 PM" src="https://github.com/user-attachments/assets/8a9f0bc9-e351-4427-8242-f3bcf031f17e">

### Customer Demographics Dashboard

<img width="1728" alt="Screenshot 2024-10-10 at 10 19 52 PM" src="https://github.com/user-attachments/assets/474e9d45-4515-4442-9e4b-43c0ef2fb72a">

### Analysis Questions:

* Which Product Type generates the highest revenue?

* Are there any significant correlations between Lead times and Order quantities?

* How do Shipping costs vary by Shipping carrier and Location?

* Which suppliers have the most efficient manufacturing processes based on Manufacturing lead time and Production volumes?

* What demographic group contributes the most to sales?

### Conclusion:

* The product type skin care generated over ₹240K revenue

* After visualising scattered graph of lead times and order quantities result shows except for supplier 3, all of them have a positive correlation

* We can see on horizontal bar graph carrier C have highest average shipping cost in Bangalore And carrier B have second high average shipping cost in Kolkata And in location Mumbai all three carriers have maintained average of ₹6

* From above visual we can see the supply 5 have worst manufacturing process and All others supplier have maintained efficient manufacturing process

* According to data the unknown identity customers have generated over ₹170K Revenue followed by female customers which have generated ₹160K

# Project 2: Iris Classification

### About Project:
The Iris Classification project involves creating a machine learning model to classify iris flowers into three species (Setosa, Versicolour, and Virginica) based on the length and width of their petals and sepals. This is a
classic problem in machine learning and is often used as an introductory example for classification algorithms.

The columns in this dataset are:

* Id

* SepalLengthCm

* SepalWidthCm

* PetalLengthCm

* PetalWidthCm

* Species

#### Problem Statement:
* The model should achieve a high level of accuracy in classifying iris species.

* The model's predictions should be consistent and reliable, as measured by cross-validation.

* The final report should provide clear and comprehensive documentation of the project, including all code, visualizations, and findings.

By achieving these objectives, the project will demonstrate the ability to apply machine learning techniques to a
classic classification problem, providing insights into the characteristics of different iris species and the
effectiveness of various algorithms for this task.

```{r, echo = TRUE, eval = TRUE, results="hide", fig.keep = "none"}
## First, we will install and load the required packages 
install.packages("tidyverse", repos="https://cloud.r-project.org/")
install.packages("readr", repos="https://cloud.r-project.org/")
install.packages("dplyr", repos="https://cloud.r-project.org/")
install.packages("ggcorrplot", repos="https://cloud.r-project.org/")
install.packages("gridExtra", repos="https://cloud.r-project.org/")
install.packages("ggplot2", repos="https://cloud.r-project.org/")
install.packages("lubridate",repos="https://cloud.r-project.org/")
install.packages("ggthemes",repos="https://cloud.r-project.org/")
install.packages("class",repos="https://cloud.r-project.org/")
install.packages("knitr",repos="https://cloud.r-project.org/")
install.packages("GGally", repos="https://cloud.r-project.org/")
install.packages("caTools", repos="https://cloud.r-project.org/")
install.packages("caret", repos="https://cloud.r-project.org/")
install.packages("lattice", repos="https://cloud.r-project.org/")
```

```{r, echo = TRUE, eval = TRUE, results="hide", fig.keep = "none"}
## Loading the libraries
library(tidyverse)
library(readr)
library(dplyr)
library(ggcorrplot)
library(ggplot2)
library(lubridate)
library(ggthemes)
library(gridExtra)
library(class)
library(knitr)
library(GGally)
library(caTools)
library(caret)
library(lattice)
```

We will now load the dataframes using `read_csv()` function 

```{r}
## Import data into R Studio 
iris <- read_csv("/Users/ronsmackbook/Desktop/Unified mentor/Iris.csv")
```

```{r}
## Let's begin exploring this dataset
## Basic Summarization functions in R
head(iris)
tail(iris)
summary(iris)
str(iris)
dim(iris)
```

The above functions provide a way for skimming through the dataset

`head()` - displays the first 6 entries

`tail()` - displays the last 6 entries

`summary()` - As stated, it summarizes the dataset

`dim()` - specifies the number of rows and columns in the dataset

`str()` - specifies the data type, variable names and the first few values

This is a better and simpler way to summarize mean of each columns grouped by species.
```{r}
iris %>% 
    group_by(Species)%>%
    summarize_if(is.numeric, mean)
```
<img width="806" alt="Screenshot 2024-10-15 at 3 42 30 PM" src="https://github.com/user-attachments/assets/aa8eaa03-aa95-41de-8696-da40b5291696">


Findings

* Setosa species has the smallest petal length and petal width. Versicolor species has average petal length and petal width. Virginica species has the highest petal length and petal width.
* Versicolor species has the smallest sepal width. Virginica species came in second and setosa species has the largest sepal width.
* Virginica species has the longest sepal length, versicolor has the second longest sepal length and setosa species has the shortest sepal length.
Let's try and understand this dataset through a few visualizations. Before that we will edit the data a bit.

```{r}
iris$Species <- as.factor(iris$Species)
```

Now, let us explore the variables by and look at the most convenient way to visualize these 4 variables? Box Plots of course!

```{r}
## Code for box plot
p1 <- iris %>% ggplot(aes(x = Species, y = SepalLengthCm)) + theme_bw() +
  geom_boxplot(aes(fill = Species)) + labs(title = "Sepal Length", xlab = 'Species',
                                           ylab = 'Sepal Length (in cm))')

p2 <- iris %>% ggplot(aes(x = Species, y = SepalWidthCm)) + theme_bw() +
  geom_boxplot(aes(fill = Species)) + labs(title = "Sepal Width", xlab = 'Species',
                                           ylab = 'Sepal Width (in cm))')
  
p3 <- iris %>% ggplot(aes(x = Species, y = PetalLengthCm)) + theme_bw() +
  geom_boxplot(aes(fill = Species)) + labs(title = "Petal Length", xlab = 'Species',
                                           ylab = 'Petal Length (in cm))')

p4 <- iris %>% ggplot(aes(x = Species, y = PetalWidthCm)) + theme_bw() +
  geom_boxplot(aes(fill = Species)) + labs(title = "Petal Width", xlab = 'Species',
                                           ylab = 'Petal Width (in cm))')
grid.arrange(p1,p2,p3,p4, ncol =2)
```
<img width="1198" alt="Screenshot 2024-10-15 at 3 28 09 PM" src="https://github.com/user-attachments/assets/786d8fd9-27dc-4a7c-8534-8352dc62e55f">

Box Plots are one very basic way to visualize the data. Another way is the violin plot and histogram. It's very similar to the box plot and it shows the distribution of points.

```{r}
head(iris %>% gather(Attributes, value, 1:4))

## Code for Violin Plot
p5 <- iris %>%
  gather(Attributes, value, 1:4) %>%
  ggplot(aes(x=reorder(Attributes, value, FUN=median), y=value, fill=Attributes)) +
  geom_violin(show.legend=FALSE) +
  geom_boxplot(width=0.05, fill="white") +
  labs(title="Iris data set",
       subtitle="Violin plot for each attribute") +
  theme_bw() +
  theme(axis.title.y=element_blank(),
        axis.title.x=element_blank())
p5
```
<img width="1195" alt="Screenshot 2024-10-15 at 3 28 38 PM" src="https://github.com/user-attachments/assets/b6749376-495d-44c2-bfd7-7808701d5751">

```{r}
## Code for Histogram
iris_grouped <- group_by(iris, Species)

p6 <- iris_grouped %>% ggplot(aes(SepalLengthCm, fill = Species)) + geom_density(alpha = 0.2) +
theme_bw()

p7 <- iris_grouped %>% ggplot(aes(SepalWidthCm, fill = Species)) + geom_density(alpha = 0.2) +
theme_bw()

p8 <- iris_grouped %>% ggplot(aes(PetalLengthCm, fill = Species)) + geom_density(alpha = 0.2) +
theme_bw()

p9 <- iris_grouped %>% ggplot(aes(PetalWidthCm, fill = Species)) + geom_density(alpha = 0.2) +
theme_bw()

grid.arrange(p6,p7,p8,p9, ncol = 2)

# Histogram for each species
p10 <- iris %>%
  gather(Attributes, Value, 1:4) %>%
  ggplot(aes(x= Value, fill= Attributes)) +
  geom_histogram(colour= "black") +
  facet_wrap(~Species) +
  theme_bw() +
  labs(x="Values", y="Frequency",
       title="Iris data set",
       subtitle="Histogram for each species") +
  theme(legend.title=element_blank(),
        legend.position="bottom")

p10
```
<img width="1196" alt="Screenshot 2024-10-15 at 3 29 10 PM" src="https://github.com/user-attachments/assets/0c3b6428-f9e8-4a15-961f-2d397646cb44">
<img width="1198" alt="Screenshot 2024-10-15 at 3 29 35 PM" src="https://github.com/user-attachments/assets/7d756683-373b-4dc1-83c1-88681155586f">

Now we will look into correlation plots

```{r}
## Code for correlation plots
iris.setosa <- iris[iris$Species == 'Iris-setosa',]
iris.versicolor <- iris[iris$Species == 'Iris-versicolor',]
iris.virginica <- iris[iris$Species == 'Iris-virginica',]

corr1 <- round(cor(iris.setosa[,1:4]),1)
ggcorrplot(corr1, method = "circle") + theme_bw() + labs(title = "Setosa")


corr2 <- round(cor(iris.versicolor[,1:4]),1)
ggcorrplot(corr2, method = "circle") + theme_bw() + labs(title = "Versicolor")


corr3 <- round(cor(iris.virginica[,1:4]),1)
ggcorrplot(corr3, method = "circle") + theme_bw() + labs(title = "Virginica")
```
<img width="1193" alt="Screenshot 2024-10-15 at 3 30 17 PM" src="https://github.com/user-attachments/assets/17996959-cb9f-45b8-8040-656812224817">
<img width="1193" alt="Screenshot 2024-10-15 at 3 30 30 PM" src="https://github.com/user-attachments/assets/0946af5c-3a21-4086-9d10-41d190b9a51c">
<img width="1193" alt="Screenshot 2024-10-15 at 3 30 40 PM" src="https://github.com/user-attachments/assets/ebef1d65-248a-418b-982b-13cfaa478962">

Now we will apply various machine algorithms for this classification problem. From the correlation plot, we can see that there is some correlation among the variables for all flower species. We also see that there is some separability among the variables based on the variables among the species. For example, Setosa has a much larger Sepal Lenth compared to the other two species. Ditto for Petal Length and Petal Width.

We get an idea from the plots that some of the classes are partially linearly separable in some dimensions, so we are expecting generally good results.

Let’s evaluate 5 different algorithms:

* Linear Discriminant Analysis (LDA) 
* Classification and Regression Trees (CART)
* k-Nearest Neighbors (kNN)
* Support Vector Machines (SVM) with a linear kernel
* Random Forest (RF)


The caret package allows you to quickly test ML algorithms before depoying them in a prodcution environment. Using the trainControl, we are telling R to perfrom 10-fold cross validation. Each model has ten results, and an accuracy distribution is obtained, which can then be compared to chose the best model. Caret does support tuning each model, but we're not doing that for this dataset

```{r}
# Set seed for reproducibility
set.seed(123)

# Splitting the data with sample.split
sample = sample.split(iris$Species, SplitRatio = 0.80)

# Subsetting the train and test data
train <- subset(iris, sample == TRUE)
test <- subset(iris, sample == FALSE)

dataset <- iris

# TrainControl setup
control <- trainControl(method="cv", number=10)
metric <- "Accuracy"

# a) linear algorithms
set.seed(90)
fit.lda <- train(Species~., data=dataset, method="lda", metric=metric, trControl=control)

# b) nonlinear algorithms
# CART
set.seed(90)
fit.cart <- train(Species~., data=dataset, method="rpart", metric=metric, trControl=control)

# kNN
set.seed(90)
fit.knn <- train(Species~., data=dataset, method="knn", metric=metric, trControl=control)

# c) advanced algorithms
# SVM
set.seed(90)
fit.svm <- train(Species~., data=dataset, method="svmRadial", metric=metric, trControl=control)

# Random Forest
set.seed(90)
fit.rf <- train(Species~., data=dataset, method="rf", metric=metric, trControl=control)

# Summarize accuracy of models
results <- resamples(list(lda=fit.lda, cart=fit.cart, knn=fit.knn, svm=fit.svm, rf=fit.rf))
summary(results)

# Plot accuracy metrics
results <- resamples(list(lda=fit.lda, cart=fit.cart, knn=fit.knn, svm=fit.svm, rf=fit.rf))
summary(results)

ggplot(data = results, mapping = NULL, metric = "Accuracy",
       output = "layered", environment = NULL)  + theme_solarized_2() +
  labs(title = "Accuracy Metrics", xlab = "Accuracy Values",
       ylab = "Method")

# Plot Kappa metrics
ggplot(data = results, mapping = NULL, metric = "Kappa",
       output = "layered", environment = NULL)  + theme_solarized_2() +
  labs(title = "Kappa", xlab = "Accuracy Values",
       ylab = "Method")
```

It turns out that LDA performs the best based on our training data. Now, let’s apply LDA to the test dataset and obtain the predictions.

```{r}
# estimate skill of LDA on the validation dataset
predictions <- predict(fit.lda, test)
# Compute confusion matrix
confusionMatrix(predictions, test$Species)
```
<img width="1174" alt="Screenshot 2024-10-15 at 3 31 05 PM" src="https://github.com/user-attachments/assets/c3411ed5-8fbc-41db-80e1-94e36c6345dc">

### Conclusion:
In this project, we successfully applied various machine learning algorithms to classify iris species, with Linear Discriminant Analysis (LDA) emerging as the most accurate model. Through comprehensive data visualization and statistical analysis, we observed significant differences between species, contributing to the model’s accuracy. With a final prediction accuracy of over 96%, the project demonstrates the effectiveness of classification techniques in solving this classic machine learning problem.

[Certificate Link](https://github.com/ron0496/Unified-Mentor-Internship-Project/blob/main/certificate.pdf)












