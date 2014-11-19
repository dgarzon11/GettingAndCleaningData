Code Book
=========

This file describes the variables, the data, and any transformations or work that I performed to clean up the data

 A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

Steps are the following:

1 Merges the training and the test sets to create one data set.
Read all the files
Combine data X_train and X_test

2 Extracts only the measurements on the mean and standard deviation for each measurement. 
Set table headers using features
Filter mean and standard deviation columns

3 Uses descriptive activity names to name the activities in the data set
Combine labels Y_train and Y_test
Clean up using gsub and tolower

4 Appropriately labels the data set with descriptive variable names. 
Accomplish it with names() function

5 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
subject_len <- length(table(subject)) # 30
activity_len <- dim(activity)[1] # 6
column_len <- dim(data_set)[2]
tidy <- matrix(NA, nrow=subject_len*activity_len, ncol=column_len) 
tidy <- as.data.frame(tidy)
colnames(tidy) <- colnames(data_set)
row <- 1
for(i in 1:subject_len) {
  for(j in 1:activity_len) {
    tidy[row, 1] <- sort(unique(subject)[, 1])[i]
    tidy[row, 2] <- activity[j, 2]
    bool1 <- i == data_set$subject
    bool2 <- activity[j, 2] == data_set$activity
    tidy[row, 3:column_len] <- colMeans(data_set[bool1&bool2, 3:column_len])
    row <- row + 1
  }
}

write.table(tidy, "tidy.txt",row.name=FALSE)
