---
title: "CodeBook"
author: "JD"
date: "16 de abril de 2015"
output: html_document
---

CodeBook
=========================================================================================



## Variables

1. There are many variables (563 in the train and test datasets), they are for the combination of many measures of 2 sources: accelerometer and gyroscope 3-axial.

2. The acceleration signal was separated in the rat data into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ)

3. The body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). 
The magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

4. Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag.

5. The subfix of this varaibles correspond to aditional calculations such as mean, standard deviation, etc.

## Transformations

1. After loading the different data sets found on the link descibed above, i named the variables from **x_train** and **x_test** as the the vector given in features.txt

````
names(x_train)<-features[,2]
names(x_test)<-features[,2]

````

2. Subsequently, i *combined by columns* the train's data and similarly for the test's data, each train and test with its similar activity:

````
train<-cbind(subject_train,y_train,x_train)
test<-cbind(subject_test,y_test,x_test)
````

3. The next step was *combining by row* both, test and train data once they have the same number and of columns. Previously i named the columns "subject" and "activity" if they are so.

```
data<-rbind(test,train)
```

4. In this point i created a dataset "data2" only with the variables wich contain the word "mean" or "std", aditionally i combined those columns by columns with vectors indicating subject and the one indicating activity. Afterwards i named both last (again because the name was lost in the merge). 

````
data2<-cbind(data[,1],data[,2],data[,grepl("mean",names(data))|grepl("std",names(data))])
names(data2)[1]<-"subject"
names(data2)[2]<-"activity"

````

5. To add the name of the activity on the database was necesary to load the **activity_labels** file, and then name each observation with the corresponding activity name. To do so, i have to create a variable "activityname" with 0's and then fill it with activity names with a loop.
Then i made all the activity names lower case and made them "factor" to match the standards of the course.

````
data2$activityname<-0

for ( i in seq_along(data[,1])){
  data2$activityname[i]<-as.character(activitylabels[,2][grepl(data2$activity[i],activitylabels[,1])])
}

data2$activityname<-tolower(data2$activityname)

data2$activityname<-as.factor(data2$activityname)

````

6. To appropriately labels the data set with descriptive variable names i used gsub(), and replaced the improper characters like "-", "()" with proper labels. I initially used uderscores to separate words, (even if its against the starndard of the course, because i think its clearer), but then i added a command that substitute this character for "" just in case its not allowed.

```
names(data2)<-gsub("-","_",names(data2))   
names(data2)<-gsub("\\()","",names(data2))   #To eliminate parenthesis of the names
names(data2)<-gsub("Acc","_accelerometer",names(data2))
names(data2)<-gsub("Gyro","_gyroscope",names(data2))
names(data2)<-gsub("Mag","_magnitude",names(data2))
names(data2)<-gsub("std","standarddeviation",names(data2))

names(data2)<-tolower(names(data2))       # To make lower case all the names
names(data2)<-gsub("^f","frequency_",names(data2)) #Used "^" to indicate its at the beginnig of the word
names(data2)<-gsub("^t","time_",names(data2))
names(data2)<-gsub("jerk","_jerk",names(data2))
names(data2)<-gsub("freq","frequency",names(data2))
names(data2)<-gsub("bodybody","body",names(data2)) #Cleanning for repeated words
names(data2)<-gsub("x$","axisx",names(data2))    #Used "$" to indicate its at the end of the word
names(data2)<-gsub("_y$","_axisy",names(data2))  #Used "_" because without it the command was taken the "y" of "frecuency
names(data2)<-gsub("z$","axisz",names(data2))

names(data2)<-gsub("_","",names(data2))   # If you want to eliminate "_" for ""

```


7. To obtain the mean of each variable for each activity for each subject the **'dplyr'** package was loaded and then grouped by activity and subject. Finally funs(mean) is used to calculate the average asked.

````
library(dplyr)
data3<-group_by(data2,activityname,subject)
prueba<-summarise_each(data3,funs(mean))
````

8. Make tidy data 
