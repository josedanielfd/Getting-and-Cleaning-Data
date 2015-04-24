
CodeBook
=========================================================================================

## Data

The data is about experiments that have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, it has been captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The obtained dataset was partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 


## Variables

1. There are many variables (563 in the train and test datasets), they are for the combination of many measures of 2 sources: accelerometer and gyroscope 3-axial.

2. The acceleration signal was separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ)

3. The body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). 
The magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

4. Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag.

5. The subfix of this varaibles correspond to aditional calculations such as mean, standard deviation, etc.

## Transformations

1. After loading the different data sets found , i named the variables from **x_train** and **x_test** as the the vector given in features.txt

2. Subsequently, i *combined by columns* the train's data (subject, X and Y) and similarly for the test's data,

3. The next step was *combining by row* both, test and train data, once they have the same number and of columns. Previously i named the columns "subject" and "activity" if they are so.

4. In this point i created a dataset "data2" only with the variables wich contain the word "mean" or "std", aditionally i combined those columns by columns with vectors indicating subject and the one indicating activity. Afterwards i named both last (again because the name was lost in the merge). 

5. To add the name of the activity on the database was necesary to load the **activity_labels** file, and then i name each observation with the corresponding activity name. To do so, i have to create a variable "activityname" with 0's and then fill it with activity names with a loop.
Then i made all the activity names lower case and made them "factor" to match the standards of the course.

6. To appropriately labels the data set with descriptive variable names i used gsub(), and replaced the improper characters like "-", "()" with proper labels. I initially used uderscores to separate words, (even if its against the starndard of the course, because i think its clearer), but then i added a command that substitute this character for "" just in case its not allowed.
After that i added a "." to the names i consider variables in order to gather this variables on one column (time, frecuency, accelerometer, gyroscope). The parameter to do that was the words that were in all the variable names.

7. To obtain the mean of each variable for each activity for each subject the **'dplyr'** package was loaded and then grouped by activity and subject. Then funs(mean) is used to calculate the average asked.

8. To make tidy data, using **tidyr** i gather all the variables except for subject and activity in one column named "class", afterwards, i separated this column by ".", so i obtained 4 columns. Then i spread the "var" column into serveral columns because they doesnt have same "parameters" (ones are means, other std, others jerkmeans, etc). (I do not create a variable called axis because many variables do not have axis labels).

9. Finally i wrote the file with write.table() 

