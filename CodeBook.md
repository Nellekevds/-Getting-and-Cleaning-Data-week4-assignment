**Data Set Information:**

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 


Variables
- names: List of names of all columns
- activitys: List of combinations activy code and activity name
- subject_train: List of subject ID's training set
- X_train: all data of sensors training set
- Y_train: List of activity code training set
- subject_test: List of subject ID's test set
- X_test: all data of sensors test set
- Y_test: List of activity code test set
- data_train: Use **cbind()** to add subject_train,Y_train and X_train together
- data_test: Use **cbind()** to add subject_test,Y_test and X_test together
- columnnames are added "subjectID", "activity" and the data from names variable
- data: Use **rbind()** to add data_test and data_train together
- cols <- grep("mean|std", colnames(data))

data_sub <- data[ , c(1,2,cols)]

#### Uses descriptive activity names to name the activities in the data set
# merges table with acitvity codes and names
data_add <- merge(data_sub,activitys,by.x="activity",by.y="V1", all=TRUE)
data_add$activity <- data_add$V2 ## replace names
data_add <- data_add[ , c(1:81)] ## remove last column
table(data_add$activity)

#LAYING            SITTING           STANDING            WALKING WALKING_DOWNSTAIRS   WALKING_UPSTAIRS 
#1944               1777               1906               1722               1406               1544 

####### From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject
# melt data set with id subjectID and activity

data_melt <- melt(data_add, id=c("subjectID", "activity"))
df_melt <- data.table(data_melt) ## make data frame

# cast the data set for tidy data

data_tidy <- dcast(df_melt, subjectID + variable ~ activity, mean, margins=FALSE)

### Write the data set to .txt file

write.table(data_tidy, "data_tidy.txt",row.name=FALSE)
