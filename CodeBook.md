The run_analysis.R script was designed to generate tidy data from raw data from the Human Activity Recognition Using Smartphones Data Set. The project was conduxted by the five (5) steps required as described next. Step 1. Merges the training and the test sets to create one data set. Task 1. Dataset downloaded and extracted under the folder called "UCI HAR Dataset" Task 2.Assign each type of data data to variables features <- read.table("UCI HAR Dataset/features.txt", col.names = c("n","functions")) 561 rows, 2 columns summary(features) n functions
Min. : 1 Length:561
1st Qu.:141 Class :character
Median :281 Mode :character
Mean :281
3rd Qu.:421
Max. :561
activities <- read.table("UCI HAR Dataset/activity_labels.txt", col.names = c("code", "activity")) 6 rows, 2 columns summary(activities) code activity
Min. :1.00 Length:6
1st Qu.:2.25 Class :character
Median :3.50 Mode :character
Mean :3.50
3rd Qu.:4.75
Max. :6.00

subject_test <- read.table("UCI HAR Dataset/test/subject_test.txt", col.names = "subject") 2947 rows, 1 column summary(subject_test) subject
Min. : 2.00
1st Qu.: 9.00
Median :12.00
Mean :12.99
3rd Qu.:18.00
Max. :24.00 x_test <- read.table("UCI HAR Dataset/test/X_test.txt", col.names = features$functions)

it contains 2947 rows, 561 columns

y_test <- read.table("UCI HAR Dataset/test/y_test.txt", col.names = "code") it contains 2947 rows, 1 columns

subject_train <- read.table("UCI HAR Dataset/train/subject_train.txt", col.names = "subject") it contains 7352 rows, 1 column

x_train <- read.table("UCI HAR Dataset/train/X_train.txt", col.names = features$functions) it contains 7352 rows, 561 columns y_train <- read.table("UCI HAR Dataset/train/y_train.txt", col.names = "code") it contains 7352 rows, 1 columns

Task 3.Merges the training and the test sets to create one data set The rbind function can be used to combine several vectors, matrices and/or data frames by rows, in this case I use this function to merge the training and the test sets to create one data set X <- rbind(x_train, x_test) Y <- rbind(y_train, y_test) Subject <- rbind(subject_train, subject_test) Merged_Data <- cbind(Subject, Y, X) (X)It contains 10,299 rows, 563 total columns (Y) It contains 10,299 rows, 1 total columns Subject contains 10,299 rows, 1 total columns Merged_Data contains 10299 rows, 563 column

Step 2. Extracts only the measurements on the mean and standard deviation for each measurement. I updated the following packages library(data.table) library(magrittr) library(dplyr) TidyDataSP <- Merged_Data %>% select(subject, code, contains("mean"), contains("std")) It contains 10,299 entries(rows), 88 total columns

Step 3. Uses descriptive activity names to name the activities in the data set All numbers in code column of the "TidyDataSP" replaced with corresponding activity taken from second column of the activities variable

Step 4. Appropriately labels the data set with descriptive variable names. code column in TidyData renamed into activities Entire Acc in column’s name replaced by Accelerometer Entire Gyro in column’s name replaced by Gyroscope Entire Bo_dy in column’s name replaced by Body Entire Magn in column’s name replaced by Magnitude Entire start with character f in column’s name replaced by Frequency Entire start with character t in column’s name replaced by Time

Step 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject. FinalData contains 180 entries(rows), and 88 total columns, It was created by sumarizing TidyDataSP taking the means of each variable per activity and for each subject, (first data was groupped by subject and activity). FinalData <- TidyDataSP %>% group_by(subject, activity) %>% summarise_all(funs(mean)) write.table(total_mean,file="./tidydata.txt",row.names = FALSE,col.names = TRUE)
