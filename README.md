# Getting-and-Cleaning-Data
Course Project

# Script

- Setting WD

```
setwd("/Users/JD/Documents/Data Science Johns Hopkins/Getting and Cleaning Data/Project/UCI HAR Dataset/")
```

## 1. Merges the training and the test sets to create one data set.

- Reads features

```
features<-read.table("features.txt")
```

### Reads train

```
setwd("./train/")

subject_train<-read.table("subject_train.txt")
x_train<-read.table("X_train.txt")
y_train<-read.table("y_train.txt")

names(x_train)<-features[,2]     # naming the columns with the features names
names(y_train)<- "activity"     # naming activity the columns 

```

- Binds the "train" files  

```
train<-cbind(subject_train,y_train,x_train)  #Binding the train files
names(train)[1]<-"subject"
```

### Reads test

```
setwd("../test/")

subject_test<-read.table("subject_test.txt")
x_test<-read.table("X_test.txt")
y_test<-read.table("y_test.txt")
names(x_test)<-features[,2]         # naming the columns with the features names
names(y_test)<-"activity"           # naming activity the columns
names(subject_test)<-"subject"
```

- Binds the "test" files 
```
test<-cbind(subject_test,y_test,x_test) 
```

### Merges by columns train and test

```
data<-rbind(test,train)
```

## 2. Extracts only the measurements on the mean and standard deviation for each measurement 

 - Extracts the columns that has the column name "mean" or "std" and merginig with subject and activity columns
 - Then, names the columns subject and activity
```
data2<-cbind(data[,1],data[,2],data[,grepl("mean",names(data))|grepl("std",names(data))])    
names(data2)[1]<-"subject"
names(data2)[2]<-"activity"
```

## 3. Uses descriptive activity names to name the activities in the data set ####

- Loads "activity_labels.txt" and matched by the id number with its description
- Made lowercase and factor the descriptive activity obtained
```
setwd("../")
activitylabels<-read.table("activity_labels.txt")

data2$activityname<-0    #Created a vector of 0s that will serve to replace with activity names

for ( i in seq_along(data[,1])){     # match the activity label with its name and replace on activityname vector
  data2$activityname[i]<-as.character(activitylabels[,2][grepl(data2$activity[i],activitylabels[,1])])
}

data2$activityname<-tolower(data2$activityname)  #made lower case the activity names

data2$activityname<-as.factor(data2$activityname)    #made factor the activity names
```

### 4. Appropriately labels the data set with descriptive variable names

- Agregates complete and descriptive names to the variables, ex: replaces Acc for accelerometer, etc
- Cleans for reapeated words (ex: bodybody)
- Made lowercase
- Eliminates symbols, just keep letters

```
names(data2)<-gsub("-","_",names(data2))   #Replacing the "-" for "_"
names(data2)<-gsub("\\()","",names(data2))   #To eliminate parenthesis of the names
names(data2)<-gsub("Acc","_accelerometer",names(data2))
names(data2)<-gsub("Gyro","_gyroscope",names(data2))
names(data2)<-gsub("Mag","_magnitude",names(data2))
names(data2)<-gsub("std","standarddeviation",names(data2))
names(data2)<-tolower(names(data2))       # To make lower case all the names
names(data2)<-gsub("jerk","_jerk",names(data2))
names(data2)<-gsub("freq","frequency",names(data2))
names(data2)<-gsub("bodybody","body",names(data2)) #Cleanning for repeated words
names(data2)<-gsub("x$","axisx",names(data2))    #Used "$" to indicate its at the end of the word
names(data2)<-gsub("_y$","_axisy",names(data2))  #Used "_" because without it the command was taken the "y" of "frecuency
names(data2)<-gsub("z$","axisz",names(data2))

names(data2)<-gsub("_","",names(data2))   # If you want to eliminate "_" for ""
```
-Aagregated dots to separate different variables with *tidyr* in *5.*
```
names(data2)<-gsub("^t","time.",names(data2))
names(data2)<-gsub("^f","frequency.",names(data2))
names(data2)<-gsub("bodybody","body.",names(data2))
names(data2)<-gsub("body","body.",names(data2))
names(data2)<-gsub("gravity","gravity.",names(data2))
names(data2)<-gsub("accelerometer","accelerometer.",names(data2))
names(data2)<-gsub("gyroscope","gyroscope.",names(data2))
```

## 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

- Groups by activity and subject
- Calculated the average fort each activity and subject
```
library(dplyr)

tidy0<-group_by(data2,activityname,subject)  #Grouped by activity name and subject
tidy1<-summarise_each(tidy0,funs(mean))    #Calculated the mean of each variable by activity and subject
```
- Made shorter and tidyier the database. *For this i considered some variable names can be variables, for example domain signals: it could take two values, etc. I do not consider each part of the variable name a variable itself because not all the variable names has the same number of characteristics. For example: Some of them does not have axis (x,y,z) and i consider it will be messier naming a variable "axis" and having lots of "NAs". And the means and std clearly are variables, so thats the reason i spread them.*

```
library(tidyr)

- Tidying the data:
tidy2<-gather(tidy1,class,value,time.body.accelerometer.meanaxisx:frequency.body.gyroscope.jerkmagnitudemeanfrequency)
tidy3<-separate(tidy2, class, into = c("domainsignals","accelerationsignals","sensor","var"))
tidy4<-spread(tidy3,var,value)
```

- Writing on a file txt

```
write.table(tidy4, file="tidydataset.txt",row.name=FALSE)
```
