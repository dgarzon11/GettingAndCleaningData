Code Book
=========

This file describes the variables, the data, and any transformations or work that I performed to clean up the data

 A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

#1 Merges the training and the test sets to create one data set.
#Read all the files
features = read.table("features.txt")
X_train = read.table("train/X_train.txt")
X_test = read.table("test/X_test.txt")
subject_train = read.table("train/subject_train.txt")
subject_test = read.table("test/subject_test.txt")
Y_train = read.table("train/Y_train.txt")
Y_test = read.table("test/Y_test.txt")

data <- rbind(X_train,X_test)

#2 Extracts only the measurements on the mean and standard deviation for each measurement. 
#set table headers using features
colnames(data) <- features[,2]
#filter mean and standard deviation columns
mean_and_std <- data[,grep("mean\\(\\)|std\\(\\)", names(data), value=TRUE)]

#3 Uses descriptive activity names to name the activities in the data set
label <- rbind(Y_train,Y_test)

activity <- read.table("activity_labels.txt")
activity[, 2] <- gsub("_", " ", activity[, 2])
activity[, 2] <- tolower(activity[, 2])
activity_label <- activity[label[, 1], 2]
label[, 1] <- activity_label
subject <- rbind(subject_train,subject_test)

#4 Appropriately labels the data set with descriptive variable names. 
names(label) <- "activity"
names(subject) <- "subject"
data_set <- cbind(subject, label, mean_and_std)

#5 From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.
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
