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

5 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each Generate the tidy data set with the average of each measurement for each activity and each subject.
Calculate the mean of each measurement with the corresponding combination.
Write the file out
