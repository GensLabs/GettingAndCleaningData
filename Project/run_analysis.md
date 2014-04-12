run_analysis
============
Last updated 2014-04-12 00:17:29 using R version 3.0.2 (2013-09-25).

> The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis. You will be graded by your peers on a series of yes/no questions related to the project. You will be required to submit: 1) a tidy data set as described below, 2) a link to a Github repository with your script for performing the analysis, and 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. You should also include a README.md in the repo with your scripts. This repo explains how all of the scripts work and how they are connected.  
> 
> One of the most exciting areas in all of data science right now is wearable computing - see for example this article . Companies like Fitbit, Nike, and Jawbone Up are racing to develop the most advanced algorithms to attract new users. The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 
> 
> http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
> 
> Here are the data for the project: 
> 
> https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
> 
> You should create one R script called run_analysis.R that does the following. 
> 
> 1. **DONE** Merges the training and the test sets to create one data set.
> 2. **DONE** Extracts only the measurements on the mean and standard deviation for each measurement.
> 3. **DONE** Uses descriptive activity names to name the activities in the data set.
> 4. **DONE** Appropriately labels the data set with descriptive activity names.
> 5. **DONE** Creates a second, independent tidy data set with the average of each variable for each activity and each subject. 
> 
> Good luck!

**The codebook is at the end of this document.**


Preliminaries
-------------

Load packages.


```r
packages <- c("data.table", "reshape2")
sapply(packages, require, character.only = TRUE, quietly = TRUE)
```

```
## data.table   reshape2 
##       TRUE       TRUE
```


Set path.


```r
path <- getwd()
path
```

```
## [1] "C:/Users/Ben/Documents/GitHub repositories/GettingAndCleaningData/Project"
```



Get the data
------------

Download the file. Put it in the `Data` folder. **This was already done on 2014-04-11; save time and don't evaluate again.**


```r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
f <- "Dataset.zip"
if (!file.exists(path)) {
    dir.create(path)
}
download.file(url, file.path(path, f))
```


Unzip the file. **This was already done on 2014-04-11; save time and don't evaluate again.**


```r
executable <- file.path("C:", "Program Files (x86)", "7-Zip", "7z.exe")
parameters <- "x"
cmd <- paste(paste0("\"", executable, "\""), parameters, paste0("\"", file.path(path, 
    f), "\""))
system(cmd)
```


The archive put the files in a folder named `UCI HAR Dataset`. Set this folder as the input path. List the files here.


```r
pathIn <- file.path(path, "UCI HAR Dataset")
list.files(pathIn, recursive = TRUE)
```

```
##  [1] "activity_labels.txt"                         
##  [2] "features.txt"                                
##  [3] "features_info.txt"                           
##  [4] "README.txt"                                  
##  [5] "test/Inertial Signals/body_acc_x_test.txt"   
##  [6] "test/Inertial Signals/body_acc_y_test.txt"   
##  [7] "test/Inertial Signals/body_acc_z_test.txt"   
##  [8] "test/Inertial Signals/body_gyro_x_test.txt"  
##  [9] "test/Inertial Signals/body_gyro_y_test.txt"  
## [10] "test/Inertial Signals/body_gyro_z_test.txt"  
## [11] "test/Inertial Signals/total_acc_x_test.txt"  
## [12] "test/Inertial Signals/total_acc_y_test.txt"  
## [13] "test/Inertial Signals/total_acc_z_test.txt"  
## [14] "test/subject_test.txt"                       
## [15] "test/X_test.txt"                             
## [16] "test/y_test.txt"                             
## [17] "train/Inertial Signals/body_acc_x_train.txt" 
## [18] "train/Inertial Signals/body_acc_y_train.txt" 
## [19] "train/Inertial Signals/body_acc_z_train.txt" 
## [20] "train/Inertial Signals/body_gyro_x_train.txt"
## [21] "train/Inertial Signals/body_gyro_y_train.txt"
## [22] "train/Inertial Signals/body_gyro_z_train.txt"
## [23] "train/Inertial Signals/total_acc_x_train.txt"
## [24] "train/Inertial Signals/total_acc_y_train.txt"
## [25] "train/Inertial Signals/total_acc_z_train.txt"
## [26] "train/subject_train.txt"                     
## [27] "train/X_train.txt"                           
## [28] "train/y_train.txt"
```


**See the `README.txt` file in C:/Users/Ben/Documents/GitHub repositories/GettingAndCleaningData/Project for detailed information on the dataset.**

For the purposes of this project, the files in the `Inertial Signals` folders are not used.


Read the files
--------------

Read the subject files.


```r
dtSubjectTrain <- fread(file.path(pathIn, "train", "subject_train.txt"))
dtSubjectTest <- fread(file.path(pathIn, "test", "subject_test.txt"))
```


Read the activity files. For some reason, these are called *label* files in the `README.txt` documentation.


```r
dtActivityTrain <- fread(file.path(pathIn, "train", "Y_train.txt"))
dtActivityTest <- fread(file.path(pathIn, "test", "Y_test.txt"))
```


Read the data files. `fread` seems to be giving me some trouble reading files. Using a helper function, read the file with `read.table` instead, then convert the resulting data frame to a data table. Return the data table.


```r
fileToDataTable <- function(f) {
    df <- read.table(f)
    dt <- data.table(df)
}
dtTrain <- fileToDataTable(file.path(pathIn, "train", "X_train.txt"))
dtTest <- fileToDataTable(file.path(pathIn, "test", "X_test.txt"))
```



Merge the training and the test sets
------------------------------------

Concatenate the data tables.


```r
dtSubject <- rbind(dtSubjectTrain, dtSubjectTest)
setnames(dtSubject, "V1", "subject")
dtActivity <- rbind(dtActivityTrain, dtActivityTest)
setnames(dtActivity, "V1", "activityNum")
dt <- rbind(dtTrain, dtTest)
```


Merge columns.


```r
dtSubject <- cbind(dtSubject, dtActivity)
dt <- cbind(dtSubject, dt)
```


Set key.


```r
setkey(dt, subject, activityNum)
```



Extract only the mean and standard deviation
--------------------------------------------

Read the `features.txt` file. This tells which variables in `dt` are measurements for the mean and standard deviation.


```r
dtFeatures <- fread(file.path(pathIn, "features.txt"))
setnames(dtFeatures, names(dtFeatures), c("featureNum", "featureName"))
```


Subset only measurements for the mean and standard deviation.


```r
dtFeatures <- dtFeatures[grepl("mean\\(\\)|std\\(\\)", featureName)]
```


Convert the column numbers to a vector of variable names matching columns in `dt`.


```r
dtFeatures$featureCode <- dtFeatures[, paste0("V", featureNum)]
head(dtFeatures)
```

```
##    featureNum       featureName featureCode
## 1:          1 tBodyAcc-mean()-X          V1
## 2:          2 tBodyAcc-mean()-Y          V2
## 3:          3 tBodyAcc-mean()-Z          V3
## 4:          4  tBodyAcc-std()-X          V4
## 5:          5  tBodyAcc-std()-Y          V5
## 6:          6  tBodyAcc-std()-Z          V6
```

```r
dtFeatures$featureCode
```

```
##  [1] "V1"   "V2"   "V3"   "V4"   "V5"   "V6"   "V41"  "V42"  "V43"  "V44" 
## [11] "V45"  "V46"  "V81"  "V82"  "V83"  "V84"  "V85"  "V86"  "V121" "V122"
## [21] "V123" "V124" "V125" "V126" "V161" "V162" "V163" "V164" "V165" "V166"
## [31] "V201" "V202" "V214" "V215" "V227" "V228" "V240" "V241" "V253" "V254"
## [41] "V266" "V267" "V268" "V269" "V270" "V271" "V345" "V346" "V347" "V348"
## [51] "V349" "V350" "V424" "V425" "V426" "V427" "V428" "V429" "V503" "V504"
## [61] "V516" "V517" "V529" "V530" "V542" "V543"
```


Subset these variables using variable names.


```r
select <- c(key(dt), dtFeatures$featureCode)
dt <- dt[, select, with = FALSE]
```



Use descriptive activity names
------------------------------

Read `activity_labels.txt` file. This will be used to add descriptive names to the activities.


```r
dtActivityNames <- fread(file.path(pathIn, "activity_labels.txt"))
setnames(dtActivityNames, names(dtActivityNames), c("activityNum", "activityName"))
```



Label with descriptive activity names
-----------------------------------------------------------------

Merge activity labels.


```r
dt <- merge(dt, dtActivityNames, by = "activityNum", all.x = TRUE)
```


Add `activityName` as a key.


```r
setkey(dt, subject, activityNum, activityName)
```


Melt the data table to reshape it from a short and wide format to a tall and narrow format.


```r
dt <- data.table(melt(dt, key(dt), variable.name = "featureCode"))
```


Merge activity name.


```r
dt <- merge(dt, dtFeatures[, list(featureNum, featureCode, featureName)], by = "featureCode", 
    all.x = TRUE)
```


Set `activityName` and `featureName` as factors.


```r
dt$activityName <- factor(dt$activityName)
dt$featureName <- factor(dt$featureName)
```


List feature names. **TO-DO add multiple factor variables for features**


```r
dt[, .N, by = "featureName"]
```

```
##                     featureName     N
##  1:           tBodyAcc-mean()-X 10299
##  2:          tBodyGyro-mean()-X 10299
##  3:          tBodyGyro-mean()-Y 10299
##  4:          tBodyGyro-mean()-Z 10299
##  5:           tBodyGyro-std()-X 10299
##  6:           tBodyGyro-std()-Y 10299
##  7:           tBodyGyro-std()-Z 10299
##  8:      tBodyGyroJerk-mean()-X 10299
##  9:      tBodyGyroJerk-mean()-Y 10299
## 10:      tBodyGyroJerk-mean()-Z 10299
## 11:       tBodyGyroJerk-std()-X 10299
## 12:       tBodyGyroJerk-std()-Y 10299
## 13:       tBodyGyroJerk-std()-Z 10299
## 14:           tBodyAcc-mean()-Y 10299
## 15:          tBodyAccMag-mean() 10299
## 16:           tBodyAccMag-std() 10299
## 17:       tGravityAccMag-mean() 10299
## 18:        tGravityAccMag-std() 10299
## 19:      tBodyAccJerkMag-mean() 10299
## 20:       tBodyAccJerkMag-std() 10299
## 21:         tBodyGyroMag-mean() 10299
## 22:          tBodyGyroMag-std() 10299
## 23:     tBodyGyroJerkMag-mean() 10299
## 24:      tBodyGyroJerkMag-std() 10299
## 25:           fBodyAcc-mean()-X 10299
## 26:           fBodyAcc-mean()-Y 10299
## 27:           fBodyAcc-mean()-Z 10299
## 28:            fBodyAcc-std()-X 10299
## 29:            fBodyAcc-std()-Y 10299
## 30:            fBodyAcc-std()-Z 10299
## 31:           tBodyAcc-mean()-Z 10299
## 32:       fBodyAccJerk-mean()-X 10299
## 33:       fBodyAccJerk-mean()-Y 10299
## 34:       fBodyAccJerk-mean()-Z 10299
## 35:        fBodyAccJerk-std()-X 10299
## 36:        fBodyAccJerk-std()-Y 10299
## 37:        fBodyAccJerk-std()-Z 10299
## 38:            tBodyAcc-std()-X 10299
## 39:        tGravityAcc-mean()-X 10299
## 40:        tGravityAcc-mean()-Y 10299
## 41:          fBodyGyro-mean()-X 10299
## 42:          fBodyGyro-mean()-Y 10299
## 43:          fBodyGyro-mean()-Z 10299
## 44:           fBodyGyro-std()-X 10299
## 45:           fBodyGyro-std()-Y 10299
## 46:           fBodyGyro-std()-Z 10299
## 47:        tGravityAcc-mean()-Z 10299
## 48:         tGravityAcc-std()-X 10299
## 49:         tGravityAcc-std()-Y 10299
## 50:         tGravityAcc-std()-Z 10299
## 51:            tBodyAcc-std()-Y 10299
## 52:          fBodyAccMag-mean() 10299
## 53:           fBodyAccMag-std() 10299
## 54:  fBodyBodyAccJerkMag-mean() 10299
## 55:   fBodyBodyAccJerkMag-std() 10299
## 56:     fBodyBodyGyroMag-mean() 10299
## 57:      fBodyBodyGyroMag-std() 10299
## 58: fBodyBodyGyroJerkMag-mean() 10299
## 59:  fBodyBodyGyroJerkMag-std() 10299
## 60:            tBodyAcc-std()-Z 10299
## 61:       tBodyAccJerk-mean()-X 10299
## 62:       tBodyAccJerk-mean()-Y 10299
## 63:       tBodyAccJerk-mean()-Z 10299
## 64:        tBodyAccJerk-std()-X 10299
## 65:        tBodyAccJerk-std()-Y 10299
## 66:        tBodyAccJerk-std()-Z 10299
##                     featureName     N
```


Check.


```r
dt
```

```
##         featureCode subject activityNum activityName   value featureNum
##      1:          V1       1           1      WALKING  0.2820          1
##      2:          V1       1           1      WALKING  0.2558          1
##      3:          V1       1           1      WALKING  0.2549          1
##      4:          V1       1           1      WALKING  0.3434          1
##      5:          V1       1           1      WALKING  0.2762          1
##     ---                                                                
## 679730:         V86      30           6       LAYING -0.9883         86
## 679731:         V86      30           6       LAYING -0.9947         86
## 679732:         V86      30           6       LAYING -0.9959         86
## 679733:         V86      30           6       LAYING -0.9917         86
## 679734:         V86      30           6       LAYING -0.9902         86
##                  featureName
##      1:    tBodyAcc-mean()-X
##      2:    tBodyAcc-mean()-X
##      3:    tBodyAcc-mean()-X
##      4:    tBodyAcc-mean()-X
##      5:    tBodyAcc-mean()-X
##     ---                     
## 679730: tBodyAccJerk-std()-Z
## 679731: tBodyAccJerk-std()-Z
## 679732: tBodyAccJerk-std()-Z
## 679733: tBodyAccJerk-std()-Z
## 679734: tBodyAccJerk-std()-Z
```



Create a tidy data set
----------------------

Create a data set with the average of each variable for each activity and each subject.


```r
setkey(dt, subject, activityName, featureName)
dtTidy <- dt[, list(count = .N, average = mean(value)), by = key(dt)]
```



Save to file
------------

Save data table objects to an RData file.


```r
f <- file.path(path, "project.RData")
save(dt, dtTidy, file = f)
```


--------------------------------------------------------------------------------

Codebook
--------

Variable list and descriptions.

Variable name | Description
--------------|------------
subject       | ID the subject who performed the activity for each window sample. Its range is from 1 to 30.
activityName  | Activity name
featureName   | Feature name
count         | Count of data points used to compute `average`
average       | Average of each variable for each activity and each subject

Dataset structure.


```r
str(dtTidy)
```

```
## Classes 'data.table' and 'data.frame':	11880 obs. of  5 variables:
##  $ subject     : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activityName: Factor w/ 6 levels "LAYING","SITTING",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ featureName : Factor w/ 66 levels "fBodyAcc-mean()-X",..: 1 2 3 4 5 6 7 8 9 10 ...
##  $ count       : int  50 50 50 50 50 50 50 50 50 50 ...
##  $ average     : num  -0.939 -0.867 -0.883 -0.924 -0.834 ...
##  - attr(*, "sorted")= chr  "subject" "activityName" "featureName"
##  - attr(*, ".internal.selfref")=<externalptr>
```


List the key variables in the data table.


```r
key(dtTidy)
```

```
## [1] "subject"      "activityName" "featureName"
```


Show a few rows of the dataset.


```r
dtTidy
```

```
##        subject     activityName           featureName count average
##     1:       1           LAYING     fBodyAcc-mean()-X    50 -0.9391
##     2:       1           LAYING     fBodyAcc-mean()-Y    50 -0.8671
##     3:       1           LAYING     fBodyAcc-mean()-Z    50 -0.8827
##     4:       1           LAYING      fBodyAcc-std()-X    50 -0.9244
##     5:       1           LAYING      fBodyAcc-std()-Y    50 -0.8336
##    ---                                                             
## 11876:      30 WALKING_UPSTAIRS   tGravityAcc-std()-X    65 -0.9540
## 11877:      30 WALKING_UPSTAIRS   tGravityAcc-std()-Y    65 -0.9149
## 11878:      30 WALKING_UPSTAIRS   tGravityAcc-std()-Z    65 -0.8624
## 11879:      30 WALKING_UPSTAIRS tGravityAccMag-mean()    65 -0.1376
## 11880:      30 WALKING_UPSTAIRS  tGravityAccMag-std()    65 -0.3274
```


Summary of variables.


```r
summary(dtTidy)
```

```
##     subject                 activityName             featureName   
##  Min.   : 1.0   LAYING            :1980   fBodyAcc-mean()-X:  180  
##  1st Qu.: 8.0   SITTING           :1980   fBodyAcc-mean()-Y:  180  
##  Median :15.5   STANDING          :1980   fBodyAcc-mean()-Z:  180  
##  Mean   :15.5   WALKING           :1980   fBodyAcc-std()-X :  180  
##  3rd Qu.:23.0   WALKING_DOWNSTAIRS:1980   fBodyAcc-std()-Y :  180  
##  Max.   :30.0   WALKING_UPSTAIRS  :1980   fBodyAcc-std()-Z :  180  
##                                           (Other)          :10800  
##      count         average       
##  Min.   :36.0   Min.   :-0.9977  
##  1st Qu.:49.0   1st Qu.:-0.9621  
##  Median :54.5   Median :-0.4699  
##  Mean   :57.2   Mean   :-0.4844  
##  3rd Qu.:63.2   3rd Qu.:-0.0784  
##  Max.   :95.0   Max.   : 0.9745  
## 
```

