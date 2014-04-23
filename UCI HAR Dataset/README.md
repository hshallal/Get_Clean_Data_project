## Make sure you have the following files in your working directory:
## ("activity_labels.txt","features.txt","subject_test.txt","subject_train.txt","X_test.txt","X_train.txt","y_test.txt","y_train.txt")        

## 1- Merges the training and the test sets to create one data set.

## Merge three files of either the test set or the train set using cbind after reading each respective

## file into to a dataframe

> subject_test <- read.table ("subject_test.txt")
> y_test <- read.table ("y_test.txt")
> X_test <- read.table ("X_test.txt")
> test <-  cbind(subject_test, y_test, X_test)

## test is a dataframe of the test set

> subject_train<- read.table ("subject_train.txt")
> y_train <- read.table ("y_train.txt")
> X_train <- read.table ("X_train.txt")
> train <-  cbind(subject_train, y_train, X_train) 

## train is a dataframe of the train set

## Combine the two dataframes into one dataframe called total

> total <- rbind(train, test)

## Change the column names of the total dataframe:

##  Assign the subject variable which takes any of the values 1 to 30 (1:30)

> colnames(total)[1] <- "subject" 

## Assign the activity label variable which takes any of the values 1 to 6 (1:6)

> colnames(total)[2] <- "activitylabel" 

## Assign the features using the features.txt as follow:

> features <- read.table("features.txt")
> features_names <- (features[,2]) 

## features_names is a factor, we need a character vector

> features_Names <- as.character(features_names) 

## features_Names is a character vector

> colnames(total)[3:563] <- features_Names ## 

## At this point, we have a total dataframe combining both train and test sets and has appropriate column names as can
## be easily checked by names(total) or head(total, 3) or viewing total dataframe using Rstudio. It's advised to have 
## a parallel dataframe called total2 to use for testing commands.

## 2- Extract only the measurements on the mean and standard deviation for each measurement.

## We now extract a dataframe called "extracted" from total by sing regular expressions to
## reatin the subject, activitylabel, and any column of mean or standard deviation.

> extracted <- total[, grep("subject|activitylabel|mean|std", colnames(total))]

## 3- Uses descriptive activity names to name the activities in the data set.
## 4- Appropriately labels the data set with descriptive activity names. 

##  Read the activity labels from the respective txt file as a dataframe

> descriptiveactivitylabels <- read.table("activity_labels.txt") 

## Replace the values in the activity label column if extracted dataframe using the following:

> extracted$activitylabel[extracted$activitylabel %in% "1"] <- "WALKING"
> extracted$activitylabel[extracted$activitylabel %in% "2"] <- "WALKING_UPSTAIRS"
> extracted$activitylabel[extracted$activitylabel %in% "3"] <- "WALKING_DOWNSTAIRS"
> extracted$activitylabel[extracted$activitylabel %in% "4"] <- "SITTING"
> extracted$activitylabel[extracted$activitylabel %in% "5"] <- "STANDING"
> extracted$activitylabel[extracted$activitylabel %in% "6"] <- "LAYING"

## 5- Creates a second, independent tidy data set with the average of each variable for each activity and each subject.  

## Convert the subject and activitylabel columns in to factors.

> extracted$subject <- as.factor(extracted$subject) 

## factor with 30 levels

> extracted$activitylabel <- as.factor(extracted$activitylabel) 

## factor with 6 levels

## Create an interaction between the two levels; subject and activitylabel

> interaction <- interaction(extracted$subject, extracted$activitylabel)

## Add the interaction vector as a column to the dataframe extracted

> extracted <- cbind(interaction, extracted)

## Calculate the mean of each variable using aggregation by the interaction column

> aggregated <- aggregate(extracted[,4:82], list(interaction=extracted$interaction), mean)

## The final dataframe, aggregated, has 180 observations corresponding to 6 activity positions of
## 30 subjects and the mean of 80 variables or features.


