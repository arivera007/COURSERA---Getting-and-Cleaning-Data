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

Read features data and set labes for X data
```
features <- read.table("./features.txt")
names(X) <- features[,2]
```

Set names for 'y' and 'subject' data
```
names(y) <- "activity"
names(subject)<- "subject"
```

Merge 'subject', 'X', and 'y'
```
data <- cbind(subject,X,y)
```

### 2. Extracts only the measurements on the mean and standard deviation for each measurement.

Remove all columns that do NOT contain '-mean' or '-std', except column 1 (subject) and column 563 (y).


```
rlist = vector()
for (i in 2:(dim(data)[2]-1)) {
  name = names(data)[i]
  if ( grepl("-mean\\(",name) == FALSE & grepl("-std\\(",name) == FALSE ) {
    rlist <- c(rlist,i)
  }
}
data <- data[,-rlist]
```

### 3. Uses descriptive activity names to name the activities in the data set

Read the activity labels and change the activities in column 'y' accordingly

```
activityLabels <- read.table("./activity_labels.txt")
data$activity <- activityLabels[data$activity,2]
```


### 4. Appropriately labels the data set with descriptive variable or feature (column) names

Change the column names to be more descriptive
```
require(seqinr)
# short-cut to sanitized column names: (coerce out `-` and `(` and `)`)
data <- data.frame(data, check.rows=TRUE)

names(data) <- gsub("\\.","_",names(data))
names(data) <- trimSpace(names(data),space="_")
names(data) <- gsub("(_)\\1+","_", names(data))

names(data) <- gsub("_a","A", names(data))
names(data) <- gsub("_b","B", names(data))
names(data) <- gsub("_c","C", names(data))
names(data) <- gsub("_e","E", names(data))
names(data) <- gsub("_g","G", names(data))
names(data) <- gsub("_i","I", names(data))
names(data) <- gsub("_m","M", names(data))
names(data) <- gsub("_s","S", names(data))
names(data) <- gsub("_t","T", names(data))
```

### 5. Creates a second, independent tidy data set with the average of each variable for each activity and each subject.

Use melt and dcast to create a new data set containing for each pair of activity and subject
the average (mean) of each variable

```
require(reshape2)
dataMelt <- melt(data, id=c("activity","subject"))

dataCast <- dcast(dataMelt, activity + subject ~ variable,mean)
```

Write the data set to file 'tidyDataSet.txt;
```
write.table(dataCast,"./tidyDataSet.txt")
```
