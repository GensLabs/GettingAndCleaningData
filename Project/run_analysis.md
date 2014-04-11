run_analysis
============
Last updated 2014-04-11 12:39:44 using R version 3.0.2 (2013-09-25).

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

The codebook is at the end of this document.


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
## [1] "C:/Users/chanb/Documents/Repositories/Coursera/GettingAndCleaningData/Project"
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


**See the `README.txt` file in C:/Users/chanb/Documents/Repositories/Coursera/GettingAndCleaningData/Project for detailed information on the dataset.**


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


Read the data files. `fread` seems to be giving me some trouble reading files. Use `read.table` instead, then convert the resulting data frame to data table.


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
setnames(dtActivity, "V1", "activity")
dt <- rbind(dtTrain, dtTest)
```


Merge columns.


```r
dtSubject <- cbind(dtSubject, dtActivity)
dt <- cbind(dtSubject, dt)
```


Set key.


```r
setkey(dt, subject, activity)
```



Extract only the mean and standard deviation
--------------------------------------------

Read the `features.txt` file. This tells which variables in `dt` are measurements for the mean and standard deviation.


```r
dtFeatures <- fread(file.path(pathIn, "features.txt"))
setnames(dtFeatures, names(dtFeatures), c("colNum", "featureName"))
```


Subset only measurements for the mean and standard deviation.


```r
dtFeatures <- dtFeatures[grepl("mean\\(\\)|std\\(\\)", featureName)]
```


Convert the column numbers to a vector of variable names matching columns in `dt`.


```r
dtFeatures$variable <- dtFeatures[, paste0("V", colNum)]
head(dtFeatures)
```

```
##    colNum       featureName variable
## 1:      1 tBodyAcc-mean()-X       V1
## 2:      2 tBodyAcc-mean()-Y       V2
## 3:      3 tBodyAcc-mean()-Z       V3
## 4:      4  tBodyAcc-std()-X       V4
## 5:      5  tBodyAcc-std()-Y       V5
## 6:      6  tBodyAcc-std()-Z       V6
```

```r
dtFeatures$variable
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
select <- c(key(dt), dtFeatures$variable)
dt <- dt[, select, with = FALSE]
```



Use descriptive activity names
------------------------------

Read `activity_labels.txt` file.


```r
dtActivityLabels <- fread(file.path(pathIn, "activity_labels.txt"))
setnames(dtActivityLabels, names(dtActivityLabels), c("activity", "activityLabel"))
```



Label with descriptive activity names
-----------------------------------------------------------------

Merge activity labels.


```r
dt <- merge(dt, dtActivityLabels, by = "activity", all.x = TRUE)
```


Add `activityLabel` as a key.


```r
setkey(dt, subject, activity, activityLabel)
```


Melt the data table to reshape it from a short and wide format to a tall and narrow format.


```r
dt <- data.table(melt(dt, key(dt)))
```


Merge activity name.


```r
dt <- merge(dt, dtFeatures[, list(colNum, variable, featureName)], by = "variable", 
    all.x = TRUE)
```


Check.


```r
dt
```

```
##         variable subject activity activityLabel   value colNum
##      1:       V1       1        1       WALKING  0.2820      1
##      2:       V1       1        1       WALKING  0.2558      1
##      3:       V1       1        1       WALKING  0.2549      1
##      4:       V1       1        1       WALKING  0.3434      1
##      5:       V1       1        1       WALKING  0.2762      1
##     ---                                                       
## 679730:      V86      30        6        LAYING -0.9883     86
## 679731:      V86      30        6        LAYING -0.9947     86
## 679732:      V86      30        6        LAYING -0.9959     86
## 679733:      V86      30        6        LAYING -0.9917     86
## 679734:      V86      30        6        LAYING -0.9902     86
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

Create a data set with the average of each variable for each activity and each subject


```r
setkey(dt, subject, activity, activityLabel, colNum, variable, featureName)
dtTidy <- dt[, list(count = .N, average = mean(value)), by = key(dt)]
```



Save to file
------------

Save data table objects to an RData file.


```r
f <- file.path(path, "project.RData")
save(dt, dtTidy, file = f)
```



Codebook
--------

### Tidy dataset


```r
str(dtTidy)
```

```
## Classes 'data.table' and 'data.frame':	11880 obs. of  8 variables:
##  $ subject      : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activity     : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activityLabel: chr  "WALKING" "WALKING" "WALKING" "WALKING" ...
##  $ colNum       : int  1 2 3 4 5 6 41 42 43 44 ...
##  $ variable     : chr  "V1" "V2" "V3" "V4" ...
##  $ featureName  : chr  "tBodyAcc-mean()-X" "tBodyAcc-mean()-Y" "tBodyAcc-mean()-Z" "tBodyAcc-std()-X" ...
##  $ count        : int  95 95 95 95 95 95 95 95 95 95 ...
##  $ average      : num  0.2773 -0.0174 -0.1111 -0.2837 0.1145 ...
##  - attr(*, "sorted")= chr  "subject" "activity" "activityLabel" "colNum" ...
##  - attr(*, ".internal.selfref")=<externalptr>
```

```r
dtTidy
```

```
##        subject activity activityLabel colNum variable
##     1:       1        1       WALKING      1       V1
##     2:       1        1       WALKING      2       V2
##     3:       1        1       WALKING      3       V3
##     4:       1        1       WALKING      4       V4
##     5:       1        1       WALKING      5       V5
##    ---                                               
## 11876:      30        6        LAYING    517     V517
## 11877:      30        6        LAYING    529     V529
## 11878:      30        6        LAYING    530     V530
## 11879:      30        6        LAYING    542     V542
## 11880:      30        6        LAYING    543     V543
##                        featureName count  average
##     1:           tBodyAcc-mean()-X    95  0.27733
##     2:           tBodyAcc-mean()-Y    95 -0.01738
##     3:           tBodyAcc-mean()-Z    95 -0.11115
##     4:            tBodyAcc-std()-X    95 -0.28374
##     5:            tBodyAcc-std()-Y    95  0.11446
##    ---                                           
## 11876:   fBodyBodyAccJerkMag-std()    70 -0.96809
## 11877:     fBodyBodyGyroMag-mean()    70 -0.96200
## 11878:      fBodyBodyGyroMag-std()    70 -0.95264
## 11879: fBodyBodyGyroJerkMag-mean()    70 -0.97782
## 11880:  fBodyBodyGyroJerkMag-std()    70 -0.97548
```


### Staged dataset


```r
str(dt)
```

```
## Classes 'data.table' and 'data.frame':	679734 obs. of  7 variables:
##  $ variable     : chr  "V1" "V1" "V1" "V1" ...
##  $ subject      : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activity     : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activityLabel: chr  "WALKING" "WALKING" "WALKING" "WALKING" ...
##  $ value        : num  0.282 0.256 0.255 0.343 0.276 ...
##  $ colNum       : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ featureName  : chr  "tBodyAcc-mean()-X" "tBodyAcc-mean()-X" "tBodyAcc-mean()-X" "tBodyAcc-mean()-X" ...
##  - attr(*, ".internal.selfref")=<externalptr> 
##  - attr(*, "sorted")= chr  "subject" "activity" "activityLabel" "colNum" ...
```

```r
dt
```

```
##         variable subject activity activityLabel   value colNum
##      1:       V1       1        1       WALKING  0.2820      1
##      2:       V1       1        1       WALKING  0.2558      1
##      3:       V1       1        1       WALKING  0.2549      1
##      4:       V1       1        1       WALKING  0.3434      1
##      5:       V1       1        1       WALKING  0.2762      1
##     ---                                                       
## 679730:     V543      30        6        LAYING -0.9980    543
## 679731:     V543      30        6        LAYING -0.9991    543
## 679732:     V543      30        6        LAYING -0.9992    543
## 679733:     V543      30        6        LAYING -0.9986    543
## 679734:     V543      30        6        LAYING -0.9989    543
##                        featureName
##      1:          tBodyAcc-mean()-X
##      2:          tBodyAcc-mean()-X
##      3:          tBodyAcc-mean()-X
##      4:          tBodyAcc-mean()-X
##      5:          tBodyAcc-mean()-X
##     ---                           
## 679730: fBodyBodyGyroJerkMag-std()
## 679731: fBodyBodyGyroJerkMag-std()
## 679732: fBodyBodyGyroJerkMag-std()
## 679733: fBodyBodyGyroJerkMag-std()
## 679734: fBodyBodyGyroJerkMag-std()
```

