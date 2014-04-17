COURSERA---Getting-and-Cleaning-Data
====================================

## run_analysis.R

### Basic usage

Execute the script run_analysis.R within R or RStudio 
in the 'UCI HAR Dataset' directory.

It is assumed that the train- and test-data are in the subdirectories
named 'train' and 'test'

The following sections describe how the script work

### 1. Merges the training and the test sets to create one data set.

read the train and test data sets and merge them.
```
X1 <- read.table("./train/X_train.txt")
X2 <- read.table("./test/X_test.txt")
X <- rbind(X1, X2)

subject1 <- read.table("./train/subject_train.txt")
subject2 <- read.table("./test/subject_test.txt")
subject <- rbind(subject1, subject2)

y1 <- read.table("./train/y_train.txt")
y2 <- read.table("./test/y_test.txt")
y <- rbind(y1, y2)

```

### 2. Extracts only the measurements on the mean and standard deviation for each measurement.
### 3. Uses descriptive activity names to name the activities in the data set
### 4. Appropriately labels the data set with descriptive activity names.
### 5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.
