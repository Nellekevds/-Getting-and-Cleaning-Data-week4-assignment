```{r}
################
## Assignment Course 3 - Week 4
## Nelleke van der Steen
## 15-12-2016
## happy christmas
#################

library(reshape2)
library(data.table)

## Load data

# read column names and activity names

names <- read.table("C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/features.txt")
activitys <- read.table("C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/activity_labels.txt")
##TRAINING
# read subject ID
subject_train <- read.table(
    "C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/train/subject_train.txt")

# read training variables
X_train <- read.table(
    "C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/train/X_train.txt")

# read type of activity
Y_train <- read.table(
    "C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/train/Y_train.txt")

## use column bind, to make one table and add columnnames
data_train <- cbind(subject_train,Y_train,X_train)
colnames(data_train) <- c("subjectID", "activity", as.character(names$V2))
##TEST

# read subject ID
subject_test <- read.table(
    "C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/test/subject_test.txt")

# read test variables
X_test <- read.table(
    "C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/test/X_test.txt")

# read type of activity
Y_test <- read.table(
    "C:/Users/nvandersteen/Documents/Data Scientist Johns Hopkins/Course 3/Week 4/UCI HAR Dataset/test/Y_test.txt")

## use column bind, to make one table and add columnnames
data_test <- cbind(subject_test,Y_test,X_test)
colnames(data_test) <- c("subjectID", "activity", as.character(names$V2))

#### Merges the training and the test sets to create one data set

data <- rbind(data_test,data_train)

#### Extracts only the measurements on the mean and standard deviation for each measurement.
## select columns with str or std in column name and the first two columns with ID and activity

cols <- grep("mean|std", colnames(data))

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

```
