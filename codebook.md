# Cleaning human activity data

## run_analysis.R cleans UCI HAR Dataset and outputs a tidy dataset. 

### Pre-requiremets:

* `UCI HAR Dataset/` must be your working folder.
* It uses `ddply()` form library `plyr`.


## The following points details the script code:

### 1. loads library plyr
    library(plyr)


### 2. read training, testing data and their labels and store them in the following variables.

    train_data = read.table("train/X_train.txt")
    train_labels = read.table("train/y_train.txt")
    train_subjects = read.table("train/subject_train.txt")
    test_data = read.table("test/X_test.txt")
    test_labels = read.table("test/y_test.txt")
    test_subjects = read.table("test/subject_test.txt")
    activity_labels = read.table("activity_labels.txt")
    colnames(activity_labels) = c("activity_number", "activity")
    features = read.table("features.txt", stringsAsFactors=FALSE)

### 3. combined training data, test data, and assign each row with an activity number
    combined_data = data.frame(data=rbind(train_data, test_data), activity=rbin(train_labels, test_labels), subjects=rbind(train_subjects, test_subjects))


### 4. give data columns descriptive names
    colnames(combined_data) = c(features$V2, "activity_number", "subject")


### 5. replace activity number with descriptive names
    combined_data= merge(combined_data, activity_labels, by.x="activity_number", by.y="activity_number",all)


### 6. find column index of measurements that are either mean or std of the measurements, and include subject and activity column. Extract the data into another variable

    index = grep("(mean|std|activity$|subject)",colnames(combined_data))
    extracted_data = combined_data[,index]

### 7. find the mean of each measurement for each subject and each activity.
    tidy_data = ddply(extracted_data, .(subject, activity), numcolwise(mean))

### 8. save tidy data as tidy_data.txt
    write.table(tidy_data, "tidy_data.txt", quote=F, sep="\t", col.names=T, row.names=F)
