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
- cols: Selected columns witn 'mean' or 'std' in columnname
- data_sub: Selection of data from column 1,2 and all columns form cols
- data_add: The sub selection (data_sub) is merged with activity data on activity code
- Column with the activity code is replace with activity names
- data_add: Data with last column removed (column with activity codes)
- data_melt: Data melted on Subject ID and activity 
- df_melt: Data frame of data_melt
- data_tidy: With **dcast** the tidy data is created in a table with subjectID and variable versus activity form the df_melt data frame
- data is writen to a .txt file
