run_analysis
============
Last updated 2014-04-12 01:44:43 using R version 3.0.2 (2013-09-25).

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


Seperate features from `featureName` using the helper function `grepthis`.


```r
grepthis <- function(regex) {
    grepl(regex, dt$featureName)
}
## Features with 2 categories
n <- 2
y <- matrix(seq(1, n), nrow = n)
x <- matrix(c(grepthis("^t"), grepthis("^f")), ncol = nrow(y))
dt$domain <- factor(x %*% y, labels = c("Time", "Freq"))
x <- matrix(c(grepthis("Acc"), grepthis("Gyro")), ncol = nrow(y))
dt$instrument <- factor(x %*% y, labels = c("Accelerometer", "Gyroscope"))
x <- matrix(c(grepthis("BodyAcc"), grepthis("GravityAcc")), ncol = nrow(y))
dt$acceleration <- factor(x %*% y, labels = c(NA, "Body", "Gravity"))
x <- matrix(c(grepthis("mean()"), grepthis("std()")), ncol = nrow(y))
dt$variable <- factor(x %*% y, labels = c("Mean", "SD"))
## Features with 1 category
dt$jerk <- factor(grepthis("Jerk"), labels = c(NA, "Jerk"))
dt$magnitude <- factor(grepthis("Mag"), labels = c(NA, "Magnitude"))
## Features with 3 categories
n <- 3
y <- matrix(seq(1, n), nrow = n)
x <- matrix(c(grepthis("-X"), grepthis("-Y"), grepthis("-Z")), ncol = nrow(y))
dt$axis <- factor(x %*% y, labels = c(NA, "X", "Y", "Z"))
```


Check.


```r
r1 <- nrow(dt[, .N, by = c("featureName")])
r2 <- nrow(dt[, .N, by = c("domain", "acceleration", "instrument", "jerk", "magnitude", 
    "variable", "axis")])
r1 == r2
```

```
## [1] TRUE
```


Yes, I accounted for all possible combinations. `featureName` is now redundant.



Create a tidy data set
----------------------

Create a data set with the average of each variable for each activity and each subject.


```r
setkey(dt, subject, activityName, domain, acceleration, instrument, jerk, magnitude, 
    variable, axis)
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
domain        | Time domain signal or frequency domain signal (Time or Freq)
instrument    | Measuring instrument (Accelerometer or Gyroscope)
acceleration  | Acceleration signal (Body or Gravity)
variable      | Variable (Mean or SD)
jerk          | Jerk signal
magnitude     | Magnitude of the signals calculated using the Euclidean norm
axis          | 3-axial signals in the X, Y and Z directions (X, Y, or Z)
count         | Count of data points used to compute `average`
average       | Average of each variable for each activity and each subject

Dataset structure.


```r
str(dtTidy)
```

```
## Classes 'data.table' and 'data.frame':	11880 obs. of  11 variables:
##  $ subject     : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activityName: Factor w/ 6 levels "LAYING","SITTING",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ domain      : Factor w/ 2 levels "Time","Freq": 1 1 1 1 1 1 1 1 1 1 ...
##  $ acceleration: Factor w/ 3 levels NA,"Body","Gravity": 1 1 1 1 1 1 1 1 1 1 ...
##  $ instrument  : Factor w/ 2 levels "Accelerometer",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ jerk        : Factor w/ 2 levels NA,"Jerk": 1 1 1 1 1 1 1 1 2 2 ...
##  $ magnitude   : Factor w/ 2 levels NA,"Magnitude": 1 1 1 1 1 1 2 2 1 1 ...
##  $ variable    : Factor w/ 2 levels "Mean","SD": 1 1 1 2 2 2 1 2 1 1 ...
##  $ axis        : Factor w/ 4 levels NA,"X","Y","Z": 2 3 4 2 3 4 1 1 2 3 ...
##  $ count       : int  50 50 50 50 50 50 50 50 50 50 ...
##  $ average     : num  -0.0166 -0.0645 0.1487 -0.8735 -0.9511 ...
##  - attr(*, "sorted")= chr  "subject" "activityName" "domain" "acceleration" ...
##  - attr(*, ".internal.selfref")=<externalptr>
```


List the key variables in the data table.


```r
key(dtTidy)
```

```
## [1] "subject"      "activityName" "domain"       "acceleration"
## [5] "instrument"   "jerk"         "magnitude"    "variable"    
## [9] "axis"
```


Show a few rows of the dataset.


```r
dtTidy
```

```
##        subject     activityName domain acceleration    instrument jerk
##     1:       1           LAYING   Time           NA     Gyroscope   NA
##     2:       1           LAYING   Time           NA     Gyroscope   NA
##     3:       1           LAYING   Time           NA     Gyroscope   NA
##     4:       1           LAYING   Time           NA     Gyroscope   NA
##     5:       1           LAYING   Time           NA     Gyroscope   NA
##    ---                                                                
## 11876:      30 WALKING_UPSTAIRS   Freq         Body Accelerometer Jerk
## 11877:      30 WALKING_UPSTAIRS   Freq         Body Accelerometer Jerk
## 11878:      30 WALKING_UPSTAIRS   Freq         Body Accelerometer Jerk
## 11879:      30 WALKING_UPSTAIRS   Freq         Body Accelerometer Jerk
## 11880:      30 WALKING_UPSTAIRS   Freq         Body Accelerometer Jerk
##        magnitude variable axis count  average
##     1:        NA     Mean    X    50 -0.01655
##     2:        NA     Mean    Y    50 -0.06449
##     3:        NA     Mean    Z    50  0.14869
##     4:        NA       SD    X    50 -0.87354
##     5:        NA       SD    Y    50 -0.95109
##    ---                                       
## 11876:        NA       SD    X    65 -0.56157
## 11877:        NA       SD    Y    65 -0.61083
## 11878:        NA       SD    Z    65 -0.78475
## 11879: Magnitude     Mean   NA    65 -0.54978
## 11880: Magnitude       SD   NA    65 -0.58088
```


Summary of variables.


```r
summary(dtTidy)
```

```
##     subject                 activityName   domain      acceleration 
##  Min.   : 1.0   LAYING            :1980   Time:7200   NA     :4680  
##  1st Qu.: 8.0   SITTING           :1980   Freq:4680   Body   :5760  
##  Median :15.5   STANDING          :1980               Gravity:1440  
##  Mean   :15.5   WALKING           :1980                             
##  3rd Qu.:23.0   WALKING_DOWNSTAIRS:1980                             
##  Max.   :30.0   WALKING_UPSTAIRS  :1980                             
##          instrument     jerk          magnitude    variable    axis     
##  Accelerometer:7200   NA  :7200   NA       :8640   Mean:5940   NA:3240  
##  Gyroscope    :4680   Jerk:4680   Magnitude:3240   SD  :5940   X :2880  
##                                                                Y :2880  
##                                                                Z :2880  
##                                                                         
##                                                                         
##      count         average       
##  Min.   :36.0   Min.   :-0.9977  
##  1st Qu.:49.0   1st Qu.:-0.9621  
##  Median :54.5   Median :-0.4699  
##  Mean   :57.2   Mean   :-0.4844  
##  3rd Qu.:63.2   3rd Qu.:-0.0784  
##  Max.   :95.0   Max.   : 0.9745
```


List all possible combinations of features.


```r
dtTidy[, .N, by = c("domain", "acceleration", "instrument", "jerk", "magnitude", 
    "variable", "axis")]
```

```
##     domain acceleration    instrument jerk magnitude variable axis   N
##  1:   Time           NA     Gyroscope   NA        NA     Mean    X 180
##  2:   Time           NA     Gyroscope   NA        NA     Mean    Y 180
##  3:   Time           NA     Gyroscope   NA        NA     Mean    Z 180
##  4:   Time           NA     Gyroscope   NA        NA       SD    X 180
##  5:   Time           NA     Gyroscope   NA        NA       SD    Y 180
##  6:   Time           NA     Gyroscope   NA        NA       SD    Z 180
##  7:   Time           NA     Gyroscope   NA Magnitude     Mean   NA 180
##  8:   Time           NA     Gyroscope   NA Magnitude       SD   NA 180
##  9:   Time           NA     Gyroscope Jerk        NA     Mean    X 180
## 10:   Time           NA     Gyroscope Jerk        NA     Mean    Y 180
## 11:   Time           NA     Gyroscope Jerk        NA     Mean    Z 180
## 12:   Time           NA     Gyroscope Jerk        NA       SD    X 180
## 13:   Time           NA     Gyroscope Jerk        NA       SD    Y 180
## 14:   Time           NA     Gyroscope Jerk        NA       SD    Z 180
## 15:   Time           NA     Gyroscope Jerk Magnitude     Mean   NA 180
## 16:   Time           NA     Gyroscope Jerk Magnitude       SD   NA 180
## 17:   Time         Body Accelerometer   NA        NA     Mean    X 180
## 18:   Time         Body Accelerometer   NA        NA     Mean    Y 180
## 19:   Time         Body Accelerometer   NA        NA     Mean    Z 180
## 20:   Time         Body Accelerometer   NA        NA       SD    X 180
## 21:   Time         Body Accelerometer   NA        NA       SD    Y 180
## 22:   Time         Body Accelerometer   NA        NA       SD    Z 180
## 23:   Time         Body Accelerometer   NA Magnitude     Mean   NA 180
## 24:   Time         Body Accelerometer   NA Magnitude       SD   NA 180
## 25:   Time         Body Accelerometer Jerk        NA     Mean    X 180
## 26:   Time         Body Accelerometer Jerk        NA     Mean    Y 180
## 27:   Time         Body Accelerometer Jerk        NA     Mean    Z 180
## 28:   Time         Body Accelerometer Jerk        NA       SD    X 180
## 29:   Time         Body Accelerometer Jerk        NA       SD    Y 180
## 30:   Time         Body Accelerometer Jerk        NA       SD    Z 180
## 31:   Time         Body Accelerometer Jerk Magnitude     Mean   NA 180
## 32:   Time         Body Accelerometer Jerk Magnitude       SD   NA 180
## 33:   Time      Gravity Accelerometer   NA        NA     Mean    X 180
## 34:   Time      Gravity Accelerometer   NA        NA     Mean    Y 180
## 35:   Time      Gravity Accelerometer   NA        NA     Mean    Z 180
## 36:   Time      Gravity Accelerometer   NA        NA       SD    X 180
## 37:   Time      Gravity Accelerometer   NA        NA       SD    Y 180
## 38:   Time      Gravity Accelerometer   NA        NA       SD    Z 180
## 39:   Time      Gravity Accelerometer   NA Magnitude     Mean   NA 180
## 40:   Time      Gravity Accelerometer   NA Magnitude       SD   NA 180
## 41:   Freq           NA     Gyroscope   NA        NA     Mean    X 180
## 42:   Freq           NA     Gyroscope   NA        NA     Mean    Y 180
## 43:   Freq           NA     Gyroscope   NA        NA     Mean    Z 180
## 44:   Freq           NA     Gyroscope   NA        NA       SD    X 180
## 45:   Freq           NA     Gyroscope   NA        NA       SD    Y 180
## 46:   Freq           NA     Gyroscope   NA        NA       SD    Z 180
## 47:   Freq           NA     Gyroscope   NA Magnitude     Mean   NA 180
## 48:   Freq           NA     Gyroscope   NA Magnitude       SD   NA 180
## 49:   Freq           NA     Gyroscope Jerk Magnitude     Mean   NA 180
## 50:   Freq           NA     Gyroscope Jerk Magnitude       SD   NA 180
## 51:   Freq         Body Accelerometer   NA        NA     Mean    X 180
## 52:   Freq         Body Accelerometer   NA        NA     Mean    Y 180
## 53:   Freq         Body Accelerometer   NA        NA     Mean    Z 180
## 54:   Freq         Body Accelerometer   NA        NA       SD    X 180
## 55:   Freq         Body Accelerometer   NA        NA       SD    Y 180
## 56:   Freq         Body Accelerometer   NA        NA       SD    Z 180
## 57:   Freq         Body Accelerometer   NA Magnitude     Mean   NA 180
## 58:   Freq         Body Accelerometer   NA Magnitude       SD   NA 180
## 59:   Freq         Body Accelerometer Jerk        NA     Mean    X 180
## 60:   Freq         Body Accelerometer Jerk        NA     Mean    Y 180
## 61:   Freq         Body Accelerometer Jerk        NA     Mean    Z 180
## 62:   Freq         Body Accelerometer Jerk        NA       SD    X 180
## 63:   Freq         Body Accelerometer Jerk        NA       SD    Y 180
## 64:   Freq         Body Accelerometer Jerk        NA       SD    Z 180
## 65:   Freq         Body Accelerometer Jerk Magnitude     Mean   NA 180
## 66:   Freq         Body Accelerometer Jerk Magnitude       SD   NA 180
##     domain acceleration    instrument jerk magnitude variable axis   N
```

