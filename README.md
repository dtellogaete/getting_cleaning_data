# Getting Cleaning Data

## Import the Library

`library(dplyr)`


## Read data

`features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions"))`

`activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity"))`

`subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt")`

`X_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)`

`y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code")`

`subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt")`

`X_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions)`

`y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code")`

## Merges the training and the test sets to create one data set.

`X <- rbind(X_train, X_test)`

`y <- rbind(y_train, y_test)`

`subject <- rbind(subject_train, subject_test)`

`colnames(subject) <- "subject"`

`df <- cbind(subject, y, X)`






## Extracts only the measurements on the mean and standard deviation for each measurement.

`
measurent <- df %>%
        select(subject, code, contains("mean"), contains("std"))
        `


## Uses descriptive activity names to name the activities in the data set

`measurent <- merge(measurent, activities, by = "code")`


## Appropriately labels the data set with descriptive variable names.
 See Read Data

## From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

`df <- measurent %>%
      group_by(activity, subject) %>%
      select(-code) %>%
      summarise_all(mean)`


## Write Table

`write.table(df, file = "dataset.txt", row.name=FALSE)`
