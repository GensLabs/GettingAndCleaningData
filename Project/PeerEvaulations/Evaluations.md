Peer evaluations
================

Assignment
----------

> The purpose of this project is to demonstrate your ability to collect, work
> with, and clean a data set. The goal is to prepare tidy data that can be used
> for later analysis. You will be graded by your peers on a series of yes/no
> questions related to the project. You will be required to submit: 1) a tidy data
> set as described below, 2) a link to a Github repository with your script for
> performing the analysis, and 3) a code book that describes the variables, the
> data, and any transformations or work that you performed to clean up the data
> called CodeBook.md. You should also include a README.md in the repo with your
> scripts. This repo explains how all of the scripts work and how they are
> connected.
> 
> One of the most exciting areas in all of data science right now is wearable
> computing - see for example this article . Companies like Fitbit, Nike, and
> Jawbone Up are racing to develop the most advanced algorithms to attract new
> users. The data linked to from the course website represent data collected from
> the accelerometers from the Samsung Galaxy S smartphone. A full description is
> available at the site where the data was obtained:
> 
> http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
> 
> Here are the data for the project: 
> 
> https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 
> 
> You should create one R script called run_analysis.R that does the following.
> Merges the training and the test sets to create one data set. Extracts only the
> measurements on the mean and standard deviation for each measurement.  Uses
> descriptive activity names to name the activities in the data set Appropriately
> labels the data set with descriptive activity names.  Creates a second,
> independent tidy data set with the average of each variable for each activity
> and each subject.
> 
> Good luck!


Preliminaries
-------------

Load packages.


```r
packages <- c("data.table")
sapply(packages, require, character.only = TRUE, quietly = TRUE)
```

```
## data.table 
##       TRUE
```


Fix URL reading for knitr. See [Stackoverflow](http://stackoverflow.com/a/20003380).


```r
setInternet2(TRUE)
```



Grab datasets
-------------
My dataset. Use this to compare to peers. 


```r
f <- file.path("..", "DatasetHumanActivityRecognitionUsingSmartphones.txt")
d0 <- data.table(read.table(f, header = TRUE))
```


Student 1. 


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-da581c348504652cf6d52b48/972080/asst-3/320c2dc0ce3e11e3bb7387f31f11f941.txt"
d1 <- data.table(read.table(url, header = TRUE))
```


Student 2. 


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-cd5fa9234072b2113411a5b6/972080/asst-3/45e306b0ce6211e38f91f1adc287d0b7.txt"
d2 <- data.table(read.table(url, header = TRUE))
```


Student 3.


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-86e8069e2ecc3248b76506ff/972080/asst-3/b00980a0ce5d11e39688c70635ebeff0.txt"
d3 <- data.table(read.csv(url))
```


Student 4. 


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-bd605606b2bf83039906529c/972080/asst-3/1cf35bf0ce3b11e3a98a0101a16d1137.txt"
d4 <- data.table(read.table(url, header = TRUE))
```



GitHub repositories
-------------------

Student | URL
--------|----
Me      | https://github.com/benjamin-chan/GettingAndCleaningData/tree/master/Project
1       | https://github.com/sdrdis/getdata_peer_assessment
2       | https://github.com/alvasvoboda/PeerAssessment
3       | https://github.com/rafalohn/tidyData
4       | https://github.com/glynn/GACD_PeerAssessment


Compare dimensions of the datasets
----------------------------------


```r
dim0 <- sprintf("%.0d rows, %.0d columns", nrow(d0), ncol(d0))
dim1 <- sprintf("%.0d rows, %.0d columns", nrow(d1), ncol(d1))
dim2 <- sprintf("%.0d rows, %.0d columns", nrow(d2), ncol(d2))
dim3 <- sprintf("%.0d rows, %.0d columns", nrow(d3), ncol(d3))
dim4 <- sprintf("%.0d rows, %.0d columns", nrow(d4), ncol(d4))
```



Student | Dimensions of tidy dataset
--------|---------------------------
Me      | 11880 rows, 11 columns
1       | 180 rows, 66 columns
2       | 10299 rows, 68 columns
3       | 79 rows, 127 columns
4       | 180 rows, 81 columns


Variables in each dataset
-------------------------


```r
names(d0)
```

```
##  [1] "subject"          "activity"         "featDomain"      
##  [4] "featAcceleration" "featInstrument"   "featJerk"        
##  [7] "featMagnitude"    "featVariable"     "featAxis"        
## [10] "count"            "average"
```

```r
names(d1)
```

```
##  [1] "tBodyAcc.mean.X"           "tBodyAcc.mean.Y"          
##  [3] "tBodyAcc.mean.Z"           "tBodyAcc.std.X"           
##  [5] "tBodyAcc.std.Y"            "tBodyAcc.std.Z"           
##  [7] "tGravityAcc.mean.X"        "tGravityAcc.mean.Y"       
##  [9] "tGravityAcc.mean.Z"        "tGravityAcc.std.X"        
## [11] "tGravityAcc.std.Y"         "tGravityAcc.std.Z"        
## [13] "tBodyAccJerk.mean.X"       "tBodyAccJerk.mean.Y"      
## [15] "tBodyAccJerk.mean.Z"       "tBodyAccJerk.std.X"       
## [17] "tBodyAccJerk.std.Y"        "tBodyAccJerk.std.Z"       
## [19] "tBodyGyro.mean.X"          "tBodyGyro.mean.Y"         
## [21] "tBodyGyro.mean.Z"          "tBodyGyro.std.X"          
## [23] "tBodyGyro.std.Y"           "tBodyGyro.std.Z"          
## [25] "tBodyGyroJerk.mean.X"      "tBodyGyroJerk.mean.Y"     
## [27] "tBodyGyroJerk.mean.Z"      "tBodyGyroJerk.std.X"      
## [29] "tBodyGyroJerk.std.Y"       "tBodyGyroJerk.std.Z"      
## [31] "tBodyAccMag.mean"          "tBodyAccMag.std"          
## [33] "tGravityAccMag.mean"       "tGravityAccMag.std"       
## [35] "tBodyAccJerkMag.mean"      "tBodyAccJerkMag.std"      
## [37] "tBodyGyroMag.mean"         "tBodyGyroMag.std"         
## [39] "tBodyGyroJerkMag.mean"     "tBodyGyroJerkMag.std"     
## [41] "fBodyAcc.mean.X"           "fBodyAcc.mean.Y"          
## [43] "fBodyAcc.mean.Z"           "fBodyAcc.std.X"           
## [45] "fBodyAcc.std.Y"            "fBodyAcc.std.Z"           
## [47] "fBodyAccJerk.mean.X"       "fBodyAccJerk.mean.Y"      
## [49] "fBodyAccJerk.mean.Z"       "fBodyAccJerk.std.X"       
## [51] "fBodyAccJerk.std.Y"        "fBodyAccJerk.std.Z"       
## [53] "fBodyGyro.mean.X"          "fBodyGyro.mean.Y"         
## [55] "fBodyGyro.mean.Z"          "fBodyGyro.std.X"          
## [57] "fBodyGyro.std.Y"           "fBodyGyro.std.Z"          
## [59] "fBodyAccMag.mean"          "fBodyAccMag.std"          
## [61] "fBodyBodyAccJerkMag.mean"  "fBodyBodyAccJerkMag.std"  
## [63] "fBodyBodyGyroMag.mean"     "fBodyBodyGyroMag.std"     
## [65] "fBodyBodyGyroJerkMag.mean" "fBodyBodyGyroJerkMag.std"
```

```r
names(d2)
```

```
##  [1] "tBodyAcc_mean_X"           "tBodyAcc_mean_Y"          
##  [3] "tBodyAcc_mean_Z"           "tBodyAcc_std_X"           
##  [5] "tBodyAcc_std_Y"            "tBodyAcc_std_Z"           
##  [7] "tGravityAcc_mean_X"        "tGravityAcc_mean_Y"       
##  [9] "tGravityAcc_mean_Z"        "tGravityAcc_std_X"        
## [11] "tGravityAcc_std_Y"         "tGravitybodyAcc_std_Z"    
## [13] "tBodyAccJerk_mean_X"       "tBodyAccJerk_mean_Y"      
## [15] "tBodyAccJerk_mean_Z"       "tBodyAccJerk_std_X"       
## [17] "tBodyAccJerk_std_Y"        "tBodyAccJerk_std_Z"       
## [19] "tBodyGyro_mean_X"          "tBodyGyro_mean_Y"         
## [21] "tBodyGyro_mean_Z"          "tBodyGyro_std_X"          
## [23] "tBodyGyro_std_Y"           "tBodyGyro_std_Z"          
## [25] "tBodyGyroJerk_mean_X"      "tBodyGyroJerk_mean_Y"     
## [27] "tBodyGyroJerk_mean_Z"      "tBodyGyroJerk_std_X"      
## [29] "tBodyGyroJerk_std_Y"       "tBodyGyroJerk_std_Z"      
## [31] "tBodyAccMag_mean"          "tBodyAccMag_std"          
## [33] "tGravityAccMag_mean"       "tGravityAccMag_std"       
## [35] "tBodyAccJerkMag_mean"      "tBodyAccJerkMag_std"      
## [37] "tBodyGyroMag_mean"         "tBodyGyroMag_std"         
## [39] "tBodyGyroJerkMag_mean"     "tBodyGyroJerkMag_std"     
## [41] "fBodyAcc_mean_X"           "fBodyAcc_mean_Y"          
## [43] "fBodyAcc_mean_Z"           "fBodyAcc_std_X"           
## [45] "fBodyAcc_std_Y"            "fBodyAcc_std_Z"           
## [47] "fBodyAccJerk_mean_X"       "fBodyAccJerk_mean_Y"      
## [49] "fBodyAccJerk_mean_Z"       "fBodyAccJerk_std_X"       
## [51] "fBodyAccJerk_std_Y"        "fBodyAccJerk_std_Z"       
## [53] "fBodyGyro_mean_X"          "fBodyGyro_mean_Y"         
## [55] "fBodyGyro_mean_Z"          "fBodyGyro_std_X"          
## [57] "fBodyGyro_std_Y"           "fBodyGyro_std_Z"          
## [59] "fBodyAccMag_mean"          "fBodyAccMag_std"          
## [61] "fBodyBodyAccJerkMag_mean"  "fBodyBodyAccJerkMag_std"  
## [63] "fBodyBodyGyroMag_mean"     "fBodyBodyGyroMag_std"     
## [65] "fBodyBodyGyroJerkMag_mean" "fBodyBodyGyroJerkMag_std" 
## [67] "y_label"                   "subject_id"
```

```r
names(d3)
```

```
##   [1] "X"                      "X1.WALKING"            
##   [3] "X3.WALKING"             "X5.WALKING"            
##   [5] "X6.WALKING"             "X7.WALKING"            
##   [7] "X8.WALKING"             "X11.WALKING"           
##   [9] "X14.WALKING"            "X15.WALKING"           
##  [11] "X16.WALKING"            "X17.WALKING"           
##  [13] "X19.WALKING"            "X21.WALKING"           
##  [15] "X22.WALKING"            "X23.WALKING"           
##  [17] "X25.WALKING"            "X26.WALKING"           
##  [19] "X27.WALKING"            "X28.WALKING"           
##  [21] "X29.WALKING"            "X30.WALKING"           
##  [23] "X1.WALKING_UPSTAIRS"    "X3.WALKING_UPSTAIRS"   
##  [25] "X5.WALKING_UPSTAIRS"    "X6.WALKING_UPSTAIRS"   
##  [27] "X7.WALKING_UPSTAIRS"    "X8.WALKING_UPSTAIRS"   
##  [29] "X11.WALKING_UPSTAIRS"   "X14.WALKING_UPSTAIRS"  
##  [31] "X15.WALKING_UPSTAIRS"   "X16.WALKING_UPSTAIRS"  
##  [33] "X17.WALKING_UPSTAIRS"   "X19.WALKING_UPSTAIRS"  
##  [35] "X21.WALKING_UPSTAIRS"   "X22.WALKING_UPSTAIRS"  
##  [37] "X23.WALKING_UPSTAIRS"   "X25.WALKING_UPSTAIRS"  
##  [39] "X26.WALKING_UPSTAIRS"   "X27.WALKING_UPSTAIRS"  
##  [41] "X28.WALKING_UPSTAIRS"   "X29.WALKING_UPSTAIRS"  
##  [43] "X30.WALKING_UPSTAIRS"   "X1.WALKING_DOWNSTAIRS" 
##  [45] "X3.WALKING_DOWNSTAIRS"  "X5.WALKING_DOWNSTAIRS" 
##  [47] "X6.WALKING_DOWNSTAIRS"  "X7.WALKING_DOWNSTAIRS" 
##  [49] "X8.WALKING_DOWNSTAIRS"  "X11.WALKING_DOWNSTAIRS"
##  [51] "X14.WALKING_DOWNSTAIRS" "X15.WALKING_DOWNSTAIRS"
##  [53] "X16.WALKING_DOWNSTAIRS" "X17.WALKING_DOWNSTAIRS"
##  [55] "X19.WALKING_DOWNSTAIRS" "X21.WALKING_DOWNSTAIRS"
##  [57] "X22.WALKING_DOWNSTAIRS" "X23.WALKING_DOWNSTAIRS"
##  [59] "X25.WALKING_DOWNSTAIRS" "X26.WALKING_DOWNSTAIRS"
##  [61] "X27.WALKING_DOWNSTAIRS" "X28.WALKING_DOWNSTAIRS"
##  [63] "X29.WALKING_DOWNSTAIRS" "X30.WALKING_DOWNSTAIRS"
##  [65] "X1.SITTING"             "X3.SITTING"            
##  [67] "X5.SITTING"             "X6.SITTING"            
##  [69] "X7.SITTING"             "X8.SITTING"            
##  [71] "X11.SITTING"            "X14.SITTING"           
##  [73] "X15.SITTING"            "X16.SITTING"           
##  [75] "X17.SITTING"            "X19.SITTING"           
##  [77] "X21.SITTING"            "X22.SITTING"           
##  [79] "X23.SITTING"            "X25.SITTING"           
##  [81] "X26.SITTING"            "X27.SITTING"           
##  [83] "X28.SITTING"            "X29.SITTING"           
##  [85] "X30.SITTING"            "X1.STANDING"           
##  [87] "X3.STANDING"            "X5.STANDING"           
##  [89] "X6.STANDING"            "X7.STANDING"           
##  [91] "X8.STANDING"            "X11.STANDING"          
##  [93] "X14.STANDING"           "X15.STANDING"          
##  [95] "X16.STANDING"           "X17.STANDING"          
##  [97] "X19.STANDING"           "X21.STANDING"          
##  [99] "X22.STANDING"           "X23.STANDING"          
## [101] "X25.STANDING"           "X26.STANDING"          
## [103] "X27.STANDING"           "X28.STANDING"          
## [105] "X29.STANDING"           "X30.STANDING"          
## [107] "X1.LAYING"              "X3.LAYING"             
## [109] "X5.LAYING"              "X6.LAYING"             
## [111] "X7.LAYING"              "X8.LAYING"             
## [113] "X11.LAYING"             "X14.LAYING"            
## [115] "X15.LAYING"             "X16.LAYING"            
## [117] "X17.LAYING"             "X19.LAYING"            
## [119] "X21.LAYING"             "X22.LAYING"            
## [121] "X23.LAYING"             "X25.LAYING"            
## [123] "X26.LAYING"             "X27.LAYING"            
## [125] "X28.LAYING"             "X29.LAYING"            
## [127] "X30.LAYING"
```

```r
names(d4)
```

```
##  [1] "subject.id"                      "activity"                       
##  [3] "tBodyAcc.mean...X"               "tBodyAcc.mean...Y"              
##  [5] "tBodyAcc.mean...Z"               "tBodyAcc.std...X"               
##  [7] "tBodyAcc.std...Y"                "tBodyAcc.std...Z"               
##  [9] "tGravityAcc.mean...X"            "tGravityAcc.mean...Y"           
## [11] "tGravityAcc.mean...Z"            "tGravityAcc.std...X"            
## [13] "tGravityAcc.std...Y"             "tGravityAcc.std...Z"            
## [15] "tBodyAccJerk.mean...X"           "tBodyAccJerk.mean...Y"          
## [17] "tBodyAccJerk.mean...Z"           "tBodyAccJerk.std...X"           
## [19] "tBodyAccJerk.std...Y"            "tBodyAccJerk.std...Z"           
## [21] "tBodyGyro.mean...X"              "tBodyGyro.mean...Y"             
## [23] "tBodyGyro.mean...Z"              "tBodyGyro.std...X"              
## [25] "tBodyGyro.std...Y"               "tBodyGyro.std...Z"              
## [27] "tBodyGyroJerk.mean...X"          "tBodyGyroJerk.mean...Y"         
## [29] "tBodyGyroJerk.mean...Z"          "tBodyGyroJerk.std...X"          
## [31] "tBodyGyroJerk.std...Y"           "tBodyGyroJerk.std...Z"          
## [33] "tBodyAccMag.mean.."              "tBodyAccMag.std.."              
## [35] "tGravityAccMag.mean.."           "tGravityAccMag.std.."           
## [37] "tBodyAccJerkMag.mean.."          "tBodyAccJerkMag.std.."          
## [39] "tBodyGyroMag.mean.."             "tBodyGyroMag.std.."             
## [41] "tBodyGyroJerkMag.mean.."         "tBodyGyroJerkMag.std.."         
## [43] "fBodyAcc.mean...X"               "fBodyAcc.mean...Y"              
## [45] "fBodyAcc.mean...Z"               "fBodyAcc.std...X"               
## [47] "fBodyAcc.std...Y"                "fBodyAcc.std...Z"               
## [49] "fBodyAcc.meanFreq...X"           "fBodyAcc.meanFreq...Y"          
## [51] "fBodyAcc.meanFreq...Z"           "fBodyAccJerk.mean...X"          
## [53] "fBodyAccJerk.mean...Y"           "fBodyAccJerk.mean...Z"          
## [55] "fBodyAccJerk.std...X"            "fBodyAccJerk.std...Y"           
## [57] "fBodyAccJerk.std...Z"            "fBodyAccJerk.meanFreq...X"      
## [59] "fBodyAccJerk.meanFreq...Y"       "fBodyAccJerk.meanFreq...Z"      
## [61] "fBodyGyro.mean...X"              "fBodyGyro.mean...Y"             
## [63] "fBodyGyro.mean...Z"              "fBodyGyro.std...X"              
## [65] "fBodyGyro.std...Y"               "fBodyGyro.std...Z"              
## [67] "fBodyGyro.meanFreq...X"          "fBodyGyro.meanFreq...Y"         
## [69] "fBodyGyro.meanFreq...Z"          "fBodyAccMag.mean.."             
## [71] "fBodyAccMag.std.."               "fBodyAccMag.meanFreq.."         
## [73] "fBodyBodyAccJerkMag.mean.."      "fBodyBodyAccJerkMag.std.."      
## [75] "fBodyBodyAccJerkMag.meanFreq.."  "fBodyBodyGyroMag.mean.."        
## [77] "fBodyBodyGyroMag.std.."          "fBodyBodyGyroMag.meanFreq.."    
## [79] "fBodyBodyGyroJerkMag.mean.."     "fBodyBodyGyroJerkMag.std.."     
## [81] "fBodyBodyGyroJerkMag.meanFreq.."
```



Examine factor class variables
------------------------------


```r
sapply(d0[, sapply(d0, is.factor), with = FALSE], levels)
```

```
## $activity
## [1] "LAYING"             "SITTING"            "STANDING"          
## [4] "WALKING"            "WALKING_DOWNSTAIRS" "WALKING_UPSTAIRS"  
## 
## $featDomain
## [1] "Freq" "Time"
## 
## $featAcceleration
## [1] "Body"    "Gravity"
## 
## $featInstrument
## [1] "Accelerometer" "Gyroscope"    
## 
## $featJerk
## [1] "Jerk"
## 
## $featMagnitude
## [1] "Magnitude"
## 
## $featVariable
## [1] "Mean" "SD"  
## 
## $featAxis
## [1] "X" "Y" "Z"
```

```r
sapply(d1[, sapply(d1, is.factor), with = FALSE], levels)
```

```
## named list()
```

```r
sapply(d2[, sapply(d2, is.factor), with = FALSE], levels)
```

```
## named list()
```

```r
sapply(d3[, sapply(d3, is.factor), with = FALSE], levels)
```

```
##       X                                
##  [1,] "fBodyAcc-mean()-X"              
##  [2,] "fBodyAcc-mean()-Y"              
##  [3,] "fBodyAcc-mean()-Z"              
##  [4,] "fBodyAcc-meanFreq()-X"          
##  [5,] "fBodyAcc-meanFreq()-Y"          
##  [6,] "fBodyAcc-meanFreq()-Z"          
##  [7,] "fBodyAcc-std()-X"               
##  [8,] "fBodyAcc-std()-Y"               
##  [9,] "fBodyAcc-std()-Z"               
## [10,] "fBodyAccJerk-mean()-X"          
## [11,] "fBodyAccJerk-mean()-Y"          
## [12,] "fBodyAccJerk-mean()-Z"          
## [13,] "fBodyAccJerk-meanFreq()-X"      
## [14,] "fBodyAccJerk-meanFreq()-Y"      
## [15,] "fBodyAccJerk-meanFreq()-Z"      
## [16,] "fBodyAccJerk-std()-X"           
## [17,] "fBodyAccJerk-std()-Y"           
## [18,] "fBodyAccJerk-std()-Z"           
## [19,] "fBodyAccMag-mean()"             
## [20,] "fBodyAccMag-meanFreq()"         
## [21,] "fBodyAccMag-std()"              
## [22,] "fBodyBodyAccJerkMag-mean()"     
## [23,] "fBodyBodyAccJerkMag-meanFreq()" 
## [24,] "fBodyBodyAccJerkMag-std()"      
## [25,] "fBodyBodyGyroJerkMag-mean()"    
## [26,] "fBodyBodyGyroJerkMag-meanFreq()"
## [27,] "fBodyBodyGyroJerkMag-std()"     
## [28,] "fBodyBodyGyroMag-mean()"        
## [29,] "fBodyBodyGyroMag-meanFreq()"    
## [30,] "fBodyBodyGyroMag-std()"         
## [31,] "fBodyGyro-mean()-X"             
## [32,] "fBodyGyro-mean()-Y"             
## [33,] "fBodyGyro-mean()-Z"             
## [34,] "fBodyGyro-meanFreq()-X"         
## [35,] "fBodyGyro-meanFreq()-Y"         
## [36,] "fBodyGyro-meanFreq()-Z"         
## [37,] "fBodyGyro-std()-X"              
## [38,] "fBodyGyro-std()-Y"              
## [39,] "fBodyGyro-std()-Z"              
## [40,] "tBodyAcc-mean()-X"              
## [41,] "tBodyAcc-mean()-Y"              
## [42,] "tBodyAcc-mean()-Z"              
## [43,] "tBodyAcc-std()-X"               
## [44,] "tBodyAcc-std()-Y"               
## [45,] "tBodyAcc-std()-Z"               
## [46,] "tBodyAccJerk-mean()-X"          
## [47,] "tBodyAccJerk-mean()-Y"          
## [48,] "tBodyAccJerk-mean()-Z"          
## [49,] "tBodyAccJerk-std()-X"           
## [50,] "tBodyAccJerk-std()-Y"           
## [51,] "tBodyAccJerk-std()-Z"           
## [52,] "tBodyAccJerkMag-mean()"         
## [53,] "tBodyAccJerkMag-std()"          
## [54,] "tBodyAccMag-mean()"             
## [55,] "tBodyAccMag-std()"              
## [56,] "tBodyGyro-mean()-X"             
## [57,] "tBodyGyro-mean()-Y"             
## [58,] "tBodyGyro-mean()-Z"             
## [59,] "tBodyGyro-std()-X"              
## [60,] "tBodyGyro-std()-Y"              
## [61,] "tBodyGyro-std()-Z"              
## [62,] "tBodyGyroJerk-mean()-X"         
## [63,] "tBodyGyroJerk-mean()-Y"         
## [64,] "tBodyGyroJerk-mean()-Z"         
## [65,] "tBodyGyroJerk-std()-X"          
## [66,] "tBodyGyroJerk-std()-Y"          
## [67,] "tBodyGyroJerk-std()-Z"          
## [68,] "tBodyGyroJerkMag-mean()"        
## [69,] "tBodyGyroJerkMag-std()"         
## [70,] "tBodyGyroMag-mean()"            
## [71,] "tBodyGyroMag-std()"             
## [72,] "tGravityAcc-mean()-X"           
## [73,] "tGravityAcc-mean()-Y"           
## [74,] "tGravityAcc-mean()-Z"           
## [75,] "tGravityAcc-std()-X"            
## [76,] "tGravityAcc-std()-Y"            
## [77,] "tGravityAcc-std()-Z"            
## [78,] "tGravityAccMag-mean()"          
## [79,] "tGravityAccMag-std()"
```

```r
sapply(d4[, sapply(d4, is.factor), with = FALSE], levels)
```

```
##      activity            
## [1,] "LAYING"            
## [2,] "SITTING"           
## [3,] "STANDING"          
## [4,] "WALKING"           
## [5,] "WALKING_DOWNSTAIRS"
## [6,] "WALKING_UPSTAIRS"
```



Find a particular value
-----------------------
Find record for

* Subject: 1
* Activity: laying
* Domain: time
* Acceleration: body
* Instrument: accelerometer
* Jerk: NA
* Magnitude: NA
* Variable: mean
* Axis: X


```r
d0[subject == 1 & activity == "LAYING" & featDomain == "Time" & featAcceleration == 
    "Body" & featInstrument == "Accelerometer" & is.na(featJerk) & is.na(featMagnitude) & 
    featVariable == "Mean" & featAxis == "X"]
```

```
##    subject activity featDomain featAcceleration featInstrument featJerk
## 1:       1   LAYING       Time             Body  Accelerometer       NA
##    featMagnitude featVariable featAxis count average
## 1:            NA         Mean        X    50  0.2216
```

```r
d1[, tBodyAcc.mean.X]
```

```
##   [1] 0.2216 0.2612 0.2789 0.2773 0.2892 0.2555 0.2802 0.2706 0.2767 0.2786
##  [11] 0.2904 0.2671 0.2806 0.2766 0.2777 0.2718 0.2916 0.2638 0.2601 0.2750
##  [21] 0.2774 0.2771 0.2815 0.2730 0.2767 0.2743 0.2778 0.2759 0.2949 0.2582
##  [31] 0.2333 0.2800 0.2805 0.2720 0.2934 0.2624 0.2895 0.2729 0.2789 0.2739
##  [41] 0.2802 0.2702 0.2742 0.2808 0.2835 0.2760 0.2956 0.2560 0.2698 0.2774
##  [51] 0.2779 0.2723 0.2939 0.2526 0.2747 0.2773 0.2785 0.2739 0.2884 0.2654
##  [61] 0.2727 0.2738 0.2782 0.2739 0.2627 0.2421 0.2814 0.2771 0.2779 0.2764
##  [71] 0.2776 0.2472 0.2395 0.2780 0.2781 0.2726 0.2961 0.2521 0.2713 0.2775
##  [81] 0.2770 0.2792 0.3015 0.2652 0.2800 0.2736 0.2791 0.2789 0.2845 0.2484
##  [91] 0.2740 0.2734 0.2779 0.2732 0.2899 0.2500 0.2729 0.2735 0.2803 0.2770
## [101] 0.2886 0.2699 0.2508 0.2785 0.2780 0.2790 0.2913 0.2780 0.2716 0.2582
## [111] 0.2811 0.2793 0.2793 0.2727 0.2741 0.2739 0.2796 0.2768 0.2975 0.2658
## [121] 0.2759 0.2770 0.2778 0.2812 0.2936 0.2620 0.2873 0.2772 0.2780 0.2720
## [131] 0.2931 0.2654 0.2755 0.2572 0.2800 0.2756 0.2924 0.2608 0.2810 0.2683
## [141] 0.2771 0.2764 0.2832 0.2714 0.2636 0.2715 0.2805 0.2786 0.2800 0.2709
## [151] 0.2783 0.2737 0.2825 0.2778 0.2935 0.2685 0.2487 0.2768 0.2803 0.2837
## [161] 0.2770 0.2682 0.2502 0.2847 0.2827 0.2756 0.2803 0.2487 0.2613 0.2675
## [171] 0.2796 0.2747 0.2835 0.2589 0.2592 0.2483 0.2823 0.2785 0.2959 0.2624
```

```r
d2[subject_id == 1, list(subject_id, y_label, tBodyAcc_mean_X)]
```

```
##       subject_id y_label tBodyAcc_mean_X
##    1:          1       1          0.2820
##    2:          1       1          0.2558
##    3:          1       1          0.2549
##    4:          1       1          0.3434
##    5:          1       1          0.2762
##   ---                                   
## 1718:          1      24          0.2592
## 1719:          1      24          0.2488
## 1720:          1      24          0.2473
## 1721:          1      24          0.2771
## 1722:          1      24          0.2870
```

```r
d3[X == "tBodyAcc-mean()-X", list(X, X1.LAYING)]
```

```
##                    X X1.LAYING
## 1: tBodyAcc-mean()-X    0.2216
```

```r
d4[subject.id == 1 & activity == "LAYING", list(subject.id, activity, tBodyAcc.mean...X)]
```

```
##    subject.id activity tBodyAcc.mean...X
## 1:          1   LAYING            0.2216
```


For Student 1 and 2's tidy datasets, it's impossible to isolate the required record using the variables within the respective datasets.


Sample observations in each dataset
-----------------------------------


```r
head(d0)
```

```
##    subject activity featDomain featAcceleration featInstrument featJerk
## 1:       1   LAYING       Time               NA      Gyroscope       NA
## 2:       1   LAYING       Time               NA      Gyroscope       NA
## 3:       1   LAYING       Time               NA      Gyroscope       NA
## 4:       1   LAYING       Time               NA      Gyroscope       NA
## 5:       1   LAYING       Time               NA      Gyroscope       NA
## 6:       1   LAYING       Time               NA      Gyroscope       NA
##    featMagnitude featVariable featAxis count  average
## 1:            NA         Mean        X    50 -0.01655
## 2:            NA         Mean        Y    50 -0.06449
## 3:            NA         Mean        Z    50  0.14869
## 4:            NA           SD        X    50 -0.87354
## 5:            NA           SD        Y    50 -0.95109
## 6:            NA           SD        Z    50 -0.90828
```

```r
head(d1)
```

```
##    tBodyAcc.mean.X tBodyAcc.mean.Y tBodyAcc.mean.Z tBodyAcc.std.X
## 1:          0.2216       -0.040514         -0.1132       -0.92806
## 2:          0.2612       -0.001308         -0.1045       -0.97723
## 3:          0.2789       -0.016138         -0.1106       -0.99576
## 4:          0.2773       -0.017384         -0.1111       -0.28374
## 5:          0.2892       -0.009919         -0.1076        0.03004
## 6:          0.2555       -0.023953         -0.0973       -0.35471
##    tBodyAcc.std.Y tBodyAcc.std.Z tGravityAcc.mean.X tGravityAcc.mean.Y
## 1:       -0.83683       -0.82606            -0.2489             0.7055
## 2:       -0.92262       -0.93959             0.8315             0.2044
## 3:       -0.97319       -0.97978             0.9430            -0.2730
## 4:        0.11446       -0.26003             0.9352            -0.2822
## 5:       -0.03194       -0.23043             0.9319            -0.2666
## 6:       -0.00232       -0.01948             0.8934            -0.3622
##    tGravityAcc.mean.Z tGravityAcc.std.X tGravityAcc.std.Y
## 1:            0.44582           -0.8968           -0.9077
## 2:            0.33204           -0.9685           -0.9355
## 3:            0.01349           -0.9938           -0.9812
## 4:           -0.06810           -0.9766           -0.9713
## 5:           -0.06212           -0.9506           -0.9370
## 6:           -0.07540           -0.9564           -0.9528
##    tGravityAcc.std.Z tBodyAccJerk.mean.X tBodyAccJerk.mean.Y
## 1:           -0.8524             0.08109           0.0038382
## 2:           -0.9490             0.07748          -0.0006191
## 3:           -0.9763             0.07538           0.0079757
## 4:           -0.9477             0.07404           0.0282721
## 5:           -0.8959             0.05416           0.0296504
## 6:           -0.9124             0.10137           0.0194863
##    tBodyAccJerk.mean.Z tBodyAccJerk.std.X tBodyAccJerk.std.Y
## 1:            0.010834           -0.95848            -0.9241
## 2:           -0.003368           -0.98643            -0.9814
## 3:           -0.003685           -0.99460            -0.9856
## 4:           -0.004168           -0.11362             0.0670
## 5:           -0.010972           -0.01228            -0.1016
## 6:           -0.045563           -0.44684            -0.3783
##    tBodyAccJerk.std.Z tBodyGyro.mean.X tBodyGyro.mean.Y tBodyGyro.mean.Z
## 1:            -0.9549         -0.01655         -0.06449          0.14869
## 2:            -0.9879         -0.04535         -0.09192          0.06293
## 3:            -0.9923         -0.02399         -0.05940          0.07480
## 4:            -0.5027         -0.04183         -0.06953          0.08494
## 5:            -0.3457         -0.03508         -0.09094          0.09009
## 6:            -0.7066          0.05055         -0.16617          0.05836
##    tBodyGyro.std.X tBodyGyro.std.Y tBodyGyro.std.Z tBodyGyroJerk.mean.X
## 1:         -0.8735       -0.951090         -0.9083             -0.10727
## 2:         -0.9772       -0.966474         -0.9414             -0.09368
## 3:         -0.9872       -0.987734         -0.9806             -0.09961
## 4:         -0.4735       -0.054608         -0.3443             -0.09000
## 5:         -0.4580       -0.126349         -0.1247             -0.07396
## 6:         -0.5449        0.004105         -0.5072             -0.12223
##    tBodyGyroJerk.mean.Y tBodyGyroJerk.mean.Z tBodyGyroJerk.std.X
## 1:             -0.04152             -0.07405             -0.9186
## 2:             -0.04021             -0.04670             -0.9917
## 3:             -0.04406             -0.04895             -0.9929
## 4:             -0.03984             -0.04613             -0.2074
## 5:             -0.04399             -0.02705             -0.4870
## 6:             -0.04215             -0.04071             -0.6148
##    tBodyGyroJerk.std.Y tBodyGyroJerk.std.Z tBodyAccMag.mean
## 1:             -0.9679             -0.9578         -0.84193
## 2:             -0.9895             -0.9879         -0.94854
## 3:             -0.9951             -0.9921         -0.98428
## 4:             -0.3045             -0.4043         -0.13697
## 5:             -0.2388             -0.2688          0.02719
## 6:             -0.6017             -0.6063         -0.12993
##    tBodyAccMag.std tGravityAccMag.mean tGravityAccMag.std
## 1:        -0.79514            -0.84193           -0.79514
## 2:        -0.92708            -0.94854           -0.92708
## 3:        -0.98194            -0.98428           -0.98194
## 4:        -0.21969            -0.13697           -0.21969
## 5:         0.01988             0.02719            0.01988
## 6:        -0.32497            -0.12993           -0.32497
##    tBodyAccJerkMag.mean tBodyAccJerkMag.std tBodyGyroMag.mean
## 1:             -0.95440            -0.92825          -0.87476
## 2:             -0.98736            -0.98412          -0.93089
## 3:             -0.99237            -0.99310          -0.97649
## 4:             -0.14143            -0.07447          -0.16098
## 5:             -0.08945            -0.02579          -0.07574
## 6:             -0.46650            -0.47899          -0.12674
##    tBodyGyroMag.std tBodyGyroJerkMag.mean tBodyGyroJerkMag.std
## 1:          -0.8190               -0.9635              -0.9358
## 2:          -0.9345               -0.9920              -0.9883
## 3:          -0.9787               -0.9950              -0.9947
## 4:          -0.1870               -0.2987              -0.3253
## 5:          -0.2257               -0.2955              -0.3065
## 6:          -0.1486               -0.5949              -0.6486
##    fBodyAcc.mean.X fBodyAcc.mean.Y fBodyAcc.mean.Z fBodyAcc.std.X
## 1:        -0.93910        -0.86707         -0.8827       -0.92444
## 2:        -0.97964        -0.94408         -0.9592       -0.97641
## 3:        -0.99525        -0.97707         -0.9853       -0.99603
## 4:        -0.20279         0.08971         -0.3316       -0.31913
## 5:         0.03823         0.00155         -0.2256        0.02433
## 6:        -0.40432        -0.19098         -0.4333       -0.33743
##    fBodyAcc.std.Y fBodyAcc.std.Z fBodyAccJerk.mean.X fBodyAccJerk.mean.Y
## 1:       -0.83363       -0.81289            -0.95707            -0.92246
## 2:       -0.91728       -0.93447            -0.98660            -0.98158
## 3:       -0.97229       -0.97794            -0.99463            -0.98542
## 4:        0.05604       -0.27969            -0.17055            -0.03523
## 5:       -0.11296       -0.29793            -0.02766            -0.12867
## 6:        0.02177        0.08596            -0.47988            -0.41344
##    fBodyAccJerk.mean.Z fBodyAccJerk.std.X fBodyAccJerk.std.Y
## 1:             -0.9481           -0.96416            -0.9322
## 2:             -0.9861           -0.98749            -0.9825
## 3:             -0.9908           -0.99507            -0.9870
## 4:             -0.4690           -0.13359             0.1067
## 5:             -0.2883           -0.08633            -0.1346
## 6:             -0.6855           -0.46191            -0.3818
##    fBodyAccJerk.std.Z fBodyGyro.mean.X fBodyGyro.mean.Y fBodyGyro.mean.Z
## 1:            -0.9606          -0.8502          -0.9522         -0.90930
## 2:            -0.9883          -0.9762          -0.9758         -0.95132
## 3:            -0.9923          -0.9864          -0.9890         -0.98077
## 4:            -0.5347          -0.3390          -0.1031         -0.25594
## 5:            -0.4017          -0.3524          -0.0557         -0.03187
## 6:            -0.7260          -0.4926          -0.3195         -0.45360
##    fBodyGyro.std.X fBodyGyro.std.Y fBodyGyro.std.Z fBodyAccMag.mean
## 1:         -0.8823        -0.95123         -0.9166         -0.86177
## 2:         -0.9779        -0.96235         -0.9439         -0.94778
## 3:         -0.9875        -0.98711         -0.9823         -0.98536
## 4:         -0.5167        -0.03351         -0.4366         -0.12862
## 5:         -0.4954        -0.18141         -0.2384          0.09658
## 6:         -0.5659         0.15154         -0.5717         -0.35240
##    fBodyAccMag.std fBodyBodyAccJerkMag.mean fBodyBodyAccJerkMag.std
## 1:         -0.7983                 -0.93330                 -0.9218
## 2:         -0.9284                 -0.98526                 -0.9816
## 3:         -0.9823                 -0.99254                 -0.9925
## 4:         -0.3980                 -0.05712                 -0.1035
## 5:         -0.1865                  0.02622                 -0.1041
## 6:         -0.4163                 -0.44265                 -0.5331
##    fBodyBodyGyroMag.mean fBodyBodyGyroMag.std fBodyBodyGyroJerkMag.mean
## 1:               -0.8622              -0.8243                   -0.9424
## 2:               -0.9584              -0.9322                   -0.9898
## 3:               -0.9846              -0.9785                   -0.9948
## 4:               -0.1993              -0.3210                   -0.3193
## 5:               -0.1857              -0.3984                   -0.2820
## 6:               -0.3260              -0.1830                   -0.6347
##    fBodyBodyGyroJerkMag.std
## 1:                  -0.9327
## 2:                  -0.9870
## 3:                  -0.9947
## 4:                  -0.3816
## 5:                  -0.3919
## 6:                  -0.6939
```

```r
head(d2)
```

```
##    tBodyAcc_mean_X tBodyAcc_mean_Y tBodyAcc_mean_Z tBodyAcc_std_X
## 1:          0.2886        -0.02029         -0.1329        -0.9953
## 2:          0.2784        -0.01641         -0.1235        -0.9982
## 3:          0.2797        -0.01947         -0.1135        -0.9954
## 4:          0.2792        -0.02620         -0.1233        -0.9961
## 5:          0.2766        -0.01657         -0.1154        -0.9981
## 6:          0.2772        -0.01010         -0.1051        -0.9973
##    tBodyAcc_std_Y tBodyAcc_std_Z tGravityAcc_mean_X tGravityAcc_mean_Y
## 1:        -0.9831        -0.9135             0.9634            -0.1408
## 2:        -0.9753        -0.9603             0.9666            -0.1416
## 3:        -0.9672        -0.9789             0.9669            -0.1420
## 4:        -0.9834        -0.9907             0.9676            -0.1440
## 5:        -0.9808        -0.9905             0.9682            -0.1488
## 6:        -0.9905        -0.9954             0.9679            -0.1482
##    tGravityAcc_mean_Z tGravityAcc_std_X tGravityAcc_std_Y
## 1:            0.11537           -0.9852           -0.9817
## 2:            0.10938           -0.9974           -0.9894
## 3:            0.10188           -0.9996           -0.9929
## 4:            0.09985           -0.9966           -0.9814
## 5:            0.09449           -0.9984           -0.9881
## 6:            0.09191           -0.9990           -0.9868
##    tGravitybodyAcc_std_Z tBodyAccJerk_mean_X tBodyAccJerk_mean_Y
## 1:               -0.8776             0.07800            0.005001
## 2:               -0.9316             0.07401            0.005771
## 3:               -0.9929             0.07364            0.003104
## 4:               -0.9785             0.07732            0.020058
## 5:               -0.9787             0.07344            0.019122
## 6:               -0.9973             0.07793            0.018684
##    tBodyAccJerk_mean_Z tBodyAccJerk_std_X tBodyAccJerk_std_Y
## 1:           -0.067831            -0.9935            -0.9884
## 2:            0.029377            -0.9955            -0.9811
## 3:           -0.009046            -0.9907            -0.9810
## 4:           -0.009865            -0.9927            -0.9876
## 5:            0.016780            -0.9964            -0.9884
## 6:            0.009344            -0.9948            -0.9887
##    tBodyAccJerk_std_Z tBodyGyro_mean_X tBodyGyro_mean_Y tBodyGyro_mean_Z
## 1:            -0.9936        -0.006101         -0.03136          0.10773
## 2:            -0.9918        -0.016112         -0.08389          0.10058
## 3:            -0.9897        -0.031698         -0.10234          0.09613
## 4:            -0.9935        -0.043410         -0.09139          0.08554
## 5:            -0.9925        -0.033960         -0.07471          0.07739
## 6:            -0.9923        -0.028776         -0.07039          0.07901
##    tBodyGyro_std_X tBodyGyro_std_Y tBodyGyro_std_Z tBodyGyroJerk_mean_X
## 1:         -0.9853         -0.9766         -0.9922             -0.09917
## 2:         -0.9831         -0.9890         -0.9891             -0.11050
## 3:         -0.9763         -0.9936         -0.9864             -0.10849
## 4:         -0.9914         -0.9924         -0.9876             -0.09117
## 5:         -0.9852         -0.9924         -0.9874             -0.09077
## 6:         -0.9852         -0.9921         -0.9831             -0.09425
##    tBodyGyroJerk_mean_Y tBodyGyroJerk_mean_Z tBodyGyroJerk_std_X
## 1:             -0.05552             -0.06199             -0.9921
## 2:             -0.04482             -0.05924             -0.9899
## 3:             -0.04241             -0.05583             -0.9885
## 4:             -0.03633             -0.06046             -0.9911
## 5:             -0.03763             -0.05829             -0.9914
## 6:             -0.04336             -0.04194             -0.9916
##    tBodyGyroJerk_std_Y tBodyGyroJerk_std_Z tBodyAccMag_mean
## 1:             -0.9925             -0.9921          -0.9594
## 2:             -0.9973             -0.9939          -0.9793
## 3:             -0.9956             -0.9915          -0.9837
## 4:             -0.9966             -0.9933          -0.9865
## 5:             -0.9965             -0.9945          -0.9928
## 6:             -0.9960             -0.9931          -0.9943
##    tBodyAccMag_std tGravityAccMag_mean tGravityAccMag_std
## 1:         -0.9506             -0.9594            -0.9506
## 2:         -0.9761             -0.9793            -0.9761
## 3:         -0.9880             -0.9837            -0.9880
## 4:         -0.9864             -0.9865            -0.9864
## 5:         -0.9913             -0.9928            -0.9913
## 6:         -0.9952             -0.9943            -0.9952
##    tBodyAccJerkMag_mean tBodyAccJerkMag_std tBodyGyroMag_mean
## 1:              -0.9933             -0.9943           -0.9690
## 2:              -0.9913             -0.9917           -0.9807
## 3:              -0.9885             -0.9904           -0.9763
## 4:              -0.9931             -0.9934           -0.9821
## 5:              -0.9935             -0.9959           -0.9852
## 6:              -0.9930             -0.9954           -0.9859
##    tBodyGyroMag_std tBodyGyroJerkMag_mean tBodyGyroJerkMag_std
## 1:          -0.9643               -0.9942              -0.9914
## 2:          -0.9838               -0.9951              -0.9961
## 3:          -0.9861               -0.9934              -0.9951
## 4:          -0.9874               -0.9955              -0.9953
## 5:          -0.9891               -0.9958              -0.9953
## 6:          -0.9864               -0.9953              -0.9952
##    fBodyAcc_mean_X fBodyAcc_mean_Y fBodyAcc_mean_Z fBodyAcc_std_X
## 1:         -0.9948         -0.9830         -0.9393        -0.9954
## 2:         -0.9975         -0.9769         -0.9735        -0.9987
## 3:         -0.9936         -0.9725         -0.9833        -0.9963
## 4:         -0.9955         -0.9836         -0.9911        -0.9963
## 5:         -0.9973         -0.9823         -0.9884        -0.9986
## 6:         -0.9967         -0.9869         -0.9927        -0.9976
##    fBodyAcc_std_Y fBodyAcc_std_Z fBodyAccJerk_mean_X fBodyAccJerk_mean_Y
## 1:        -0.9831        -0.9062             -0.9923             -0.9872
## 2:        -0.9749        -0.9554             -0.9950             -0.9813
## 3:        -0.9655        -0.9770             -0.9910             -0.9816
## 4:        -0.9832        -0.9902             -0.9944             -0.9887
## 5:        -0.9801        -0.9919             -0.9963             -0.9888
## 6:        -0.9923        -0.9970             -0.9949             -0.9882
##    fBodyAccJerk_mean_Z fBodyAccJerk_std_X fBodyAccJerk_std_Y
## 1:             -0.9897            -0.9958            -0.9909
## 2:             -0.9897            -0.9967            -0.9821
## 3:             -0.9876            -0.9912            -0.9814
## 4:             -0.9914            -0.9914            -0.9869
## 5:             -0.9906            -0.9969            -0.9886
## 6:             -0.9902            -0.9952            -0.9902
##    fBodyAccJerk_std_Z fBodyGyro_mean_X fBodyGyro_mean_Y fBodyGyro_mean_Z
## 1:            -0.9971          -0.9866          -0.9818          -0.9895
## 2:            -0.9926          -0.9774          -0.9925          -0.9896
## 3:            -0.9904          -0.9754          -0.9937          -0.9868
## 4:            -0.9944          -0.9871          -0.9936          -0.9872
## 5:            -0.9929          -0.9824          -0.9930          -0.9887
## 6:            -0.9931          -0.9849          -0.9928          -0.9808
##    fBodyGyro_std_X fBodyGyro_std_Y fBodyGyro_std_Z fBodyAccMag_mean
## 1:         -0.9850         -0.9739         -0.9940          -0.9522
## 2:         -0.9849         -0.9872         -0.9898          -0.9809
## 3:         -0.9766         -0.9934         -0.9873          -0.9878
## 4:         -0.9928         -0.9916         -0.9887          -0.9875
## 5:         -0.9860         -0.9920         -0.9879          -0.9936
## 6:         -0.9853         -0.9917         -0.9854          -0.9948
##    fBodyAccMag_std fBodyBodyAccJerkMag_mean fBodyBodyAccJerkMag_std
## 1:         -0.9561                  -0.9937                 -0.9938
## 2:         -0.9759                  -0.9903                 -0.9920
## 3:         -0.9890                  -0.9893                 -0.9909
## 4:         -0.9867                  -0.9928                 -0.9917
## 5:         -0.9901                  -0.9955                 -0.9944
## 6:         -0.9953                  -0.9947                 -0.9952
##    fBodyBodyGyroMag_mean fBodyBodyGyroMag_std fBodyBodyGyroJerkMag_mean
## 1:               -0.9801              -0.9613                    0.3746
## 2:               -0.9883              -0.9833                   -0.7200
## 3:               -0.9893              -0.9860                   -0.8719
## 4:               -0.9894              -0.9878                   -0.5112
## 5:               -0.9914              -0.9891                   -0.8307
## 6:               -0.9905              -0.9859                   -0.7273
##    fBodyBodyGyroJerkMag_std y_label subject_id
## 1:                  -0.9920       1          5
## 2:                  -0.9959       1          5
## 3:                  -0.9950       1          5
## 4:                  -0.9952       1          5
## 5:                  -0.9951       1          5
## 6:                  -0.9951       1          5
```

```r
head(d3)
```

```
##                    X X1.WALKING X3.WALKING X5.WALKING X6.WALKING
## 1: tBodyAcc-mean()-X    0.27733    0.27557    0.27784     0.2837
## 2: tBodyAcc-mean()-Y   -0.01738   -0.01718   -0.01729    -0.0169
## 3: tBodyAcc-mean()-Z   -0.11115   -0.11267   -0.10774    -0.1103
## 4:  tBodyAcc-std()-X   -0.28374   -0.36036   -0.29410    -0.2965
## 5:  tBodyAcc-std()-Y    0.11446   -0.06991    0.07675     0.1642
## 6:  tBodyAcc-std()-Z   -0.26003   -0.38741   -0.45702    -0.5043
##    X7.WALKING X8.WALKING X11.WALKING X14.WALKING X15.WALKING X16.WALKING
## 1:    0.27559    0.27469     0.27182     0.27196     0.27390     0.27602
## 2:   -0.01865   -0.01866    -0.01665    -0.02178    -0.01708    -0.02043
## 3:   -0.11091   -0.10725    -0.10610    -0.10676    -0.10762    -0.10880
## 4:   -0.32724   -0.17361    -0.42284    -0.40264    -0.32796    -0.40469
## 5:   -0.07726    0.38078    -0.05221    -0.05361     0.13891    -0.31457
## 6:    0.15963   -0.14209    -0.53063     0.05188    -0.51893    -0.15980
##    X17.WALKING X19.WALKING X21.WALKING X22.WALKING X23.WALKING X25.WALKING
## 1:     0.27234     0.27393     0.27918    0.278865     0.27321     0.27899
## 2:    -0.01849    -0.01918    -0.01816   -0.016721    -0.01836    -0.01865
## 3:    -0.10979    -0.12274    -0.10432   -0.107113    -0.11338    -0.10874
## 4:    -0.31950    -0.04890    -0.29781   -0.008659    -0.31352    -0.59601
## 5:    -0.01758     0.18180     0.05409    0.100384    -0.11902    -0.16185
## 6:    -0.26582    -0.13948    -0.16869   -0.213352     0.16422    -0.43713
##    X26.WALKING X27.WALKING X28.WALKING X29.WALKING X30.WALKING
## 1:     0.27926     0.27685     0.28123     0.27200     0.27641
## 2:    -0.01543    -0.01665    -0.01568    -0.01629    -0.01759
## 3:    -0.10893    -0.11282    -0.10371    -0.10663    -0.09862
## 4:    -0.34019    -0.34848    -0.29300    -0.17434    -0.34639
## 5:    -0.13635    -0.18730    -0.11789    -0.09175    -0.17355
## 6:    -0.33349    -0.29732    -0.30093    -0.24283    -0.12048
##    X1.WALKING_UPSTAIRS X3.WALKING_UPSTAIRS X5.WALKING_UPSTAIRS
## 1:             0.25546             0.26082             0.26846
## 2:            -0.02395            -0.03241            -0.03253
## 3:            -0.09730            -0.11006            -0.10747
## 4:            -0.35471            -0.31312            -0.04572
## 5:            -0.00232             0.01163             0.18502
## 6:            -0.01948            -0.36975            -0.30894
##    X6.WALKING_UPSTAIRS X7.WALKING_UPSTAIRS X8.WALKING_UPSTAIRS
## 1:             0.26823             0.24871             0.25888
## 2:            -0.02724            -0.02756            -0.02824
## 3:            -0.12208            -0.14377            -0.11512
## 4:            -0.05014            -0.29487            -0.17171
## 5:             0.18928            -0.32616             0.34877
## 6:            -0.35352            -0.14567             0.12119
##    X11.WALKING_UPSTAIRS X14.WALKING_UPSTAIRS X15.WALKING_UPSTAIRS
## 1:              0.26378              0.26242             0.270188
## 2:             -0.03032             -0.02044            -0.028752
## 3:             -0.10680             -0.11228            -0.116952
## 4:             -0.23880             -0.30940            -0.026097
## 5:             -0.10317              0.30720            -0.004065
## 6:             -0.20337              0.60902            -0.379717
##    X16.WALKING_UPSTAIRS X17.WALKING_UPSTAIRS X19.WALKING_UPSTAIRS
## 1:              0.25599              0.25260               0.2421
## 2:             -0.01437             -0.02286              -0.0304
## 3:             -0.12407             -0.12129              -0.1511
## 4:             -0.39708             -0.06295              -0.1287
## 5:             -0.16373             -0.01317               0.1763
## 6:             -0.12928             -0.23022              -0.1905
##    X21.WALKING_UPSTAIRS X22.WALKING_UPSTAIRS X23.WALKING_UPSTAIRS
## 1:              0.26519              0.24839              0.25000
## 2:             -0.02372             -0.02686             -0.03238
## 3:             -0.12541             -0.11755             -0.12689
## 4:             -0.24081              0.08357             -0.24389
## 5:              0.10979              0.25007              0.04484
## 6:             -0.21971             -0.10961              0.10080
##    X25.WALKING_UPSTAIRS X26.WALKING_UPSTAIRS X27.WALKING_UPSTAIRS
## 1:              0.27800              0.27269              0.26577
## 2:             -0.02699             -0.02816             -0.02010
## 3:             -0.12621             -0.12194             -0.12353
## 4:             -0.45978             -0.16901             -0.29544
## 5:             -0.22309             -0.04917             -0.09501
## 6:             -0.29652             -0.40460             -0.23071
##    X28.WALKING_UPSTAIRS X29.WALKING_UPSTAIRS X30.WALKING_UPSTAIRS
## 1:              0.26201              0.26542              0.27142
## 2:             -0.02794             -0.02995             -0.02533
## 3:             -0.12151             -0.11800             -0.12470
## 4:             -0.24206             -0.08677             -0.35050
## 5:             -0.14675             -0.12213             -0.12731
## 6:             -0.28599              0.09954              0.02495
##    X1.WALKING_DOWNSTAIRS X3.WALKING_DOWNSTAIRS X5.WALKING_DOWNSTAIRS
## 1:              0.289188               0.29242              0.293544
## 2:             -0.009919              -0.01936             -0.008501
## 3:             -0.107566              -0.11614             -0.100320
## 4:              0.030035              -0.05741              0.275046
## 5:             -0.031936              -0.03315              0.090763
## 6:             -0.230434              -0.36224             -0.325891
##    X6.WALKING_DOWNSTAIRS X7.WALKING_DOWNSTAIRS X8.WALKING_DOWNSTAIRS
## 1:               0.27705               0.28031               0.28348
## 2:              -0.01954              -0.01663              -0.02111
## 3:              -0.10721              -0.09694              -0.10760
## 4:               0.38368               0.06608               0.02414
## 5:               0.36016              -0.13818               0.34383
## 6:              -0.32020              -0.06364               0.13372
##    X11.WALKING_DOWNSTAIRS X14.WALKING_DOWNSTAIRS X15.WALKING_DOWNSTAIRS
## 1:                0.29161                0.29342                0.28020
## 2:               -0.01781               -0.02001               -0.00563
## 3:               -0.11107               -0.09283               -0.11053
## 4:                0.14246                0.01783                0.40647
## 5:                0.07081                0.37887                0.18653
## 6:               -0.32403                0.45158               -0.28198
##    X16.WALKING_DOWNSTAIRS X17.WALKING_DOWNSTAIRS X19.WALKING_DOWNSTAIRS
## 1:                0.29559                0.29392                0.26269
## 2:               -0.01839               -0.01674               -0.01459
## 3:               -0.11987               -0.08924               -0.13370
## 4:                0.20725                0.18772                0.62692
## 5:               -0.14699               -0.06079                0.51482
## 6:                0.01144               -0.23106                0.04932
##    X21.WALKING_DOWNSTAIRS X22.WALKING_DOWNSTAIRS X23.WALKING_DOWNSTAIRS
## 1:                0.30146                 0.2845                0.28990
## 2:               -0.01732                -0.0198               -0.01621
## 3:               -0.09817                -0.1074               -0.09881
## 4:                0.22434                 0.3486                0.04414
## 5:                0.30134                 0.2398                0.10826
## 6:               -0.07179                -0.3257                0.22990
##    X25.WALKING_DOWNSTAIRS X26.WALKING_DOWNSTAIRS X27.WALKING_DOWNSTAIRS
## 1:                0.29133                0.27928                0.29754
## 2:               -0.02102               -0.01263               -0.01356
## 3:               -0.10719               -0.10644               -0.11284
## 4:               -0.25376                0.17395                0.16741
## 5:               -0.14054               -0.01163               -0.09797
## 6:               -0.45797               -0.28481               -0.24004
##    X28.WALKING_DOWNSTAIRS X29.WALKING_DOWNSTAIRS X30.WALKING_DOWNSTAIRS
## 1:                0.29364                0.29314                0.28319
## 2:               -0.02202               -0.01494               -0.01744
## 3:               -0.10859               -0.09813               -0.09998
## 4:                0.12571                0.16738               -0.05777
## 5:                0.15650               -0.12246               -0.02726
## 6:               -0.34103               -0.22318               -0.21728
##    X1.SITTING X3.SITTING X5.SITTING X6.SITTING X7.SITTING X8.SITTING
## 1:   0.261238   0.257198   0.273694    0.27678    0.28467   0.267491
## 2:  -0.001308  -0.003503  -0.009901   -0.01459   -0.01461  -0.006726
## 3:  -0.104544  -0.098358  -0.108540   -0.11013   -0.12246  -0.104461
## 4:  -0.977229  -0.971010  -0.980945   -0.98016   -0.97267  -0.979026
## 5:  -0.922619  -0.856618  -0.904335   -0.92368   -0.90945  -0.927332
## 6:  -0.939586  -0.875110  -0.926095   -0.92580   -0.85653  -0.939553
##    X11.SITTING X14.SITTING X15.SITTING X16.SITTING X17.SITTING X19.SITTING
## 1:     0.27659    0.279991     0.27290     0.28077     0.27736     0.27383
## 2:    -0.01492   -0.008706    -0.01172    -0.01025    -0.01416    -0.01674
## 3:    -0.11284   -0.100401    -0.11366    -0.08914    -0.11362    -0.10871
## 4:    -0.98280   -0.976302    -0.98708    -0.98683    -0.99444    -0.97636
## 5:    -0.92140   -0.914936    -0.92241    -0.95161    -0.96186    -0.95039
## 6:    -0.96837   -0.922775    -0.94934    -0.93967    -0.96573    -0.95193
##    X21.SITTING X22.SITTING X23.SITTING X25.SITTING X26.SITTING X27.SITTING
## 1:      0.2775     0.27358     0.27335     0.27854    0.258244     0.27394
## 2:     -0.0144    -0.01235    -0.01339    -0.01477   -0.007134    -0.01553
## 3:     -0.1121    -0.10583    -0.10381    -0.10916   -0.097445    -0.10552
## 4:     -0.9918    -0.97847    -0.98761    -0.99189   -0.979769    -0.98858
## 5:     -0.9669    -0.92806    -0.93493    -0.94746   -0.940799    -0.97158
## 6:     -0.9596    -0.91756    -0.90558    -0.96514   -0.950004    -0.96586
##    X28.SITTING X29.SITTING X30.SITTING X1.STANDING X3.STANDING X5.STANDING
## 1:     0.27698     0.27718    0.268336     0.27892     0.28005    0.282544
## 2:    -0.01854    -0.01663   -0.008047    -0.01614    -0.01434   -0.007004
## 3:    -0.11152    -0.11041   -0.099515    -0.11060    -0.10162   -0.102171
## 4:    -0.98327    -0.99075   -0.983623    -0.99576    -0.96674   -0.968592
## 5:    -0.93991    -0.96322   -0.937857    -0.97319    -0.89345   -0.869440
## 6:    -0.93667    -0.96806   -0.950654    -0.97978    -0.91142   -0.869327
##    X6.STANDING X7.STANDING X8.STANDING X11.STANDING X14.STANDING
## 1:     0.28035     0.28272     0.27962       0.2777      0.28055
## 2:    -0.01812    -0.01457    -0.01481      -0.0172     -0.01521
## 3:    -0.11217    -0.09978    -0.10611      -0.1087     -0.10382
## 4:    -0.98176    -0.97931    -0.98882      -0.9950     -0.97327
## 5:    -0.92149    -0.92340    -0.93848      -0.9642     -0.92853
## 6:    -0.92569    -0.91706    -0.92607      -0.9864     -0.92228
##    X15.STANDING X16.STANDING X17.STANDING X19.STANDING X21.STANDING
## 1:      0.27892       0.2835      0.27794      0.27817      0.27695
## 2:     -0.01835      -0.0166     -0.01741     -0.01542     -0.01671
## 3:     -0.10591      -0.1037     -0.11143     -0.10904     -0.11042
## 4:     -0.98895      -0.9891     -0.99116     -0.98991     -0.98142
## 5:     -0.93190      -0.9603     -0.96818     -0.94443     -0.94360
## 6:     -0.95167      -0.9535     -0.97046     -0.95260     -0.94612
##    X22.STANDING X23.STANDING X25.STANDING X26.STANDING X27.STANDING
## 1:      0.27905      0.27790      0.27801      0.28113      0.27957
## 2:     -0.01586     -0.01775     -0.01636     -0.01666     -0.01659
## 3:     -0.10497     -0.11060     -0.10735     -0.11024     -0.10784
## 4:     -0.98500     -0.98542     -0.99197     -0.99307     -0.99195
## 5:     -0.92269     -0.95844     -0.95457     -0.94947     -0.95517
## 6:     -0.94220     -0.92941     -0.96464     -0.96299     -0.96241
##    X28.STANDING X29.STANDING X30.STANDING X1.LAYING X3.LAYING X5.LAYING
## 1:      0.27780      0.27797      0.27711   0.22160   0.27552    0.2783
## 2:     -0.01726     -0.01726     -0.01702  -0.04051  -0.01896   -0.0183
## 3:     -0.10658     -0.10866     -0.10876  -0.11320  -0.10130   -0.1079
## 4:     -0.97775     -0.99607     -0.97756  -0.92806  -0.98278   -0.9659
## 5:     -0.87569     -0.96930     -0.89165  -0.83683  -0.96206   -0.9693
## 6:     -0.90512     -0.98021     -0.91285  -0.82606  -0.96369   -0.9686
##    X6.LAYING X7.LAYING X8.LAYING X11.LAYING X14.LAYING X15.LAYING
## 1:   0.24866   0.25018   0.26125    0.28059    0.23328    0.28948
## 2:  -0.01025  -0.02044  -0.02123   -0.01766   -0.01134   -0.01663
## 3:  -0.13312  -0.10136  -0.10225   -0.10879   -0.08683   -0.11853
## 4:  -0.93405  -0.93651  -0.94304   -0.98478   -0.91750   -0.97226
## 5:  -0.92464  -0.92626  -0.93489   -0.97220   -0.90970   -0.96276
## 6:  -0.92522  -0.95298  -0.93249   -0.97131   -0.90033   -0.92959
##    X16.LAYING X17.LAYING X19.LAYING X21.LAYING X22.LAYING X23.LAYING
## 1:    0.27423    0.26978    0.27265    0.27133    0.27996    0.27404
## 2:   -0.01661   -0.01685   -0.01714   -0.01842   -0.01426   -0.02166
## 3:   -0.10731   -0.10701   -0.10898   -0.10325   -0.11080   -0.10426
## 4:   -0.97369   -0.97296   -0.96502   -0.95502   -0.94774   -0.95676
## 5:   -0.94306   -0.94479   -0.97345   -0.95696   -0.91328   -0.97631
## 6:   -0.96549   -0.95348   -0.98467   -0.94565   -0.94295   -0.97322
##    X25.LAYING X26.LAYING X27.LAYING X28.LAYING X29.LAYING X30.LAYING
## 1:    0.25079    0.27165    0.27410    0.27591     0.2873    0.28103
## 2:   -0.01889   -0.01919   -0.01799   -0.01675    -0.0172   -0.01945
## 3:   -0.10043   -0.10500   -0.10770   -0.10834    -0.1095   -0.10366
## 4:   -0.90912   -0.96945   -0.97846   -0.96889    -0.9842   -0.97636
## 5:   -0.69177   -0.98323   -0.98374   -0.94539    -0.9902   -0.95420
## 6:   -0.71726   -0.98450   -0.98664   -0.95645    -0.9873   -0.96704
```

```r
head(d4)
```

```
##    subject.id activity tBodyAcc.mean...X tBodyAcc.mean...Y
## 1:          1  WALKING            0.2773          -0.01738
## 2:          2  WALKING            0.2764          -0.01859
## 3:          3  WALKING            0.2756          -0.01718
## 4:          4  WALKING            0.2786          -0.01484
## 5:          5  WALKING            0.2778          -0.01729
## 6:          6  WALKING            0.2837          -0.01690
##    tBodyAcc.mean...Z tBodyAcc.std...X tBodyAcc.std...Y tBodyAcc.std...Z
## 1:           -0.1111          -0.2837          0.11446          -0.2600
## 2:           -0.1055          -0.4236         -0.07809          -0.4253
## 3:           -0.1127          -0.3604         -0.06991          -0.3874
## 4:           -0.1114          -0.4408         -0.07883          -0.5863
## 5:           -0.1077          -0.2941          0.07675          -0.4570
## 6:           -0.1103          -0.2965          0.16421          -0.5043
##    tGravityAcc.mean...X tGravityAcc.mean...Y tGravityAcc.mean...Z
## 1:               0.9352             -0.28217            -0.068103
## 2:               0.9130             -0.34661             0.084727
## 3:               0.9365             -0.26199            -0.138108
## 4:               0.9640             -0.08585             0.127764
## 5:               0.9726             -0.10044             0.002476
## 6:               0.9581             -0.21469             0.033189
##    tGravityAcc.std...X tGravityAcc.std...Y tGravityAcc.std...Z
## 1:             -0.9766             -0.9713             -0.9477
## 2:             -0.9727             -0.9721             -0.9721
## 3:             -0.9778             -0.9624             -0.9521
## 4:             -0.9838             -0.9680             -0.9630
## 5:             -0.9793             -0.9616             -0.9646
## 6:             -0.9778             -0.9642             -0.9572
##    tBodyAccJerk.mean...X tBodyAccJerk.mean...Y tBodyAccJerk.mean...Z
## 1:               0.07404              0.028272            -4.168e-03
## 2:               0.06181              0.018249             7.895e-03
## 3:               0.08147              0.010059            -5.623e-03
## 4:               0.07835              0.002956            -7.677e-04
## 5:               0.08459             -0.016319             8.322e-05
## 6:               0.06996             -0.016483            -7.389e-03
##    tBodyAccJerk.std...X tBodyAccJerk.std...Y tBodyAccJerk.std...Z
## 1:              -0.1136             0.067003              -0.5027
## 2:              -0.2775            -0.016602              -0.5861
## 3:              -0.2687            -0.044962              -0.5295
## 4:              -0.2970            -0.221165              -0.7514
## 5:              -0.3029            -0.091040              -0.6129
## 6:              -0.1328             0.008089              -0.5758
##    tBodyGyro.mean...X tBodyGyro.mean...Y tBodyGyro.mean...Z
## 1:           -0.04183           -0.06953            0.08494
## 2:           -0.05303           -0.04824            0.08283
## 3:           -0.02564           -0.07792            0.08135
## 4:           -0.03180           -0.07269            0.08057
## 5:           -0.04889           -0.06901            0.08154
## 6:           -0.02551           -0.07445            0.08388
##    tBodyGyro.std...X tBodyGyro.std...Y tBodyGyro.std...Z
## 1:           -0.4735          -0.05461           -0.3443
## 2:           -0.5616          -0.53845           -0.4811
## 3:           -0.5719          -0.56379           -0.4767
## 4:           -0.5009          -0.66539           -0.6626
## 5:           -0.4909          -0.50462           -0.3187
## 6:           -0.4460          -0.33170           -0.3831
##    tBodyGyroJerk.mean...X tBodyGyroJerk.mean...Y tBodyGyroJerk.mean...Z
## 1:               -0.09000               -0.03984               -0.04613
## 2:               -0.08188               -0.05383               -0.05149
## 3:               -0.09524               -0.03879               -0.05036
## 4:               -0.11532               -0.03935               -0.05512
## 5:               -0.08884               -0.04496               -0.04827
## 6:               -0.08789               -0.03623               -0.05396
##    tBodyGyroJerk.std...X tBodyGyroJerk.std...Y tBodyGyroJerk.std...Z
## 1:               -0.2074               -0.3045               -0.4043
## 2:               -0.3895               -0.6341               -0.4355
## 3:               -0.3859               -0.6391               -0.5367
## 4:               -0.4923               -0.8074               -0.6405
## 5:               -0.3577               -0.5714               -0.1577
## 6:               -0.1826               -0.4164               -0.1667
##    tBodyAccMag.mean.. tBodyAccMag.std.. tGravityAccMag.mean..
## 1:            -0.1370           -0.2197               -0.1370
## 2:            -0.2904           -0.4225               -0.2904
## 3:            -0.2547           -0.3284               -0.2547
## 4:            -0.3121           -0.5277               -0.3121
## 5:            -0.1583           -0.3772               -0.1583
## 6:            -0.1668           -0.2667               -0.1668
##    tGravityAccMag.std.. tBodyAccJerkMag.mean.. tBodyAccJerkMag.std..
## 1:              -0.2197                -0.1414              -0.07447
## 2:              -0.4225                -0.2814              -0.16415
## 3:              -0.3284                -0.2800              -0.13992
## 4:              -0.5277                -0.3667              -0.31692
## 5:              -0.3772                -0.2883              -0.28224
## 6:              -0.2667                -0.1951              -0.07060
##    tBodyGyroMag.mean.. tBodyGyroMag.std.. tBodyGyroJerkMag.mean..
## 1:             -0.1610            -0.1870                 -0.2987
## 2:             -0.4465            -0.5530                 -0.5479
## 3:             -0.4664            -0.5615                 -0.5661
## 4:             -0.4978            -0.5531                 -0.6813
## 5:             -0.3559            -0.4922                 -0.4445
## 6:             -0.2812            -0.3656                 -0.3213
##    tBodyGyroJerkMag.std.. fBodyAcc.mean...X fBodyAcc.mean...Y
## 1:                -0.3253           -0.2028           0.08971
## 2:                -0.5578           -0.3460          -0.02190
## 3:                -0.5674           -0.3166          -0.08130
## 4:                -0.7301           -0.4267          -0.14940
## 5:                -0.4892           -0.2878           0.00946
## 6:                -0.3647           -0.1879           0.14078
##    fBodyAcc.mean...Z fBodyAcc.std...X fBodyAcc.std...Y fBodyAcc.std...Z
## 1:           -0.3316          -0.3191          0.05604          -0.2797
## 2:           -0.4538          -0.4577         -0.16922          -0.4552
## 3:           -0.4124          -0.3793         -0.12403          -0.4230
## 4:           -0.6310          -0.4472         -0.10180          -0.5942
## 5:           -0.4903          -0.2975          0.04260          -0.4831
## 6:           -0.4985          -0.3452          0.10170          -0.5505
##    fBodyAcc.meanFreq...X fBodyAcc.meanFreq...Y fBodyAcc.meanFreq...Z
## 1:               -0.2075              0.113094               0.04973
## 2:               -0.1458              0.198586               0.06890
## 3:               -0.2466              0.171743               0.07485
## 4:               -0.1386              0.012348              -0.07879
## 5:               -0.3224             -0.002041               0.02474
## 6:               -0.1968              0.022073               0.18511
##    fBodyAccJerk.mean...X fBodyAccJerk.mean...Y fBodyAccJerk.mean...Z
## 1:               -0.1705              -0.03523               -0.4690
## 2:               -0.3046              -0.07876               -0.5550
## 3:               -0.3047              -0.14051               -0.5141
## 4:               -0.3589              -0.27955               -0.7290
## 5:               -0.3450              -0.18106               -0.5905
## 6:               -0.1509              -0.07537               -0.5414
##    fBodyAccJerk.std...X fBodyAccJerk.std...Y fBodyAccJerk.std...Z
## 1:              -0.1336             0.106740              -0.5347
## 2:              -0.3143            -0.015333              -0.6159
## 3:              -0.2966            -0.005615              -0.5435
## 4:              -0.2973            -0.209900              -0.7724
## 5:              -0.3214            -0.054521              -0.6334
## 6:              -0.1927             0.031445              -0.6086
##    fBodyAccJerk.meanFreq...X fBodyAccJerk.meanFreq...Y
## 1:                  -0.20926                   -0.3862
## 2:                  -0.07271                   -0.2636
## 3:                  -0.21604                   -0.2587
## 4:                  -0.13528                   -0.3859
## 5:                  -0.35937                   -0.5340
## 6:                  -0.17830                   -0.4663
##    fBodyAccJerk.meanFreq...Z fBodyGyro.mean...X fBodyGyro.mean...Y
## 1:                   -0.1855            -0.3390            -0.1031
## 2:                   -0.2548            -0.4297            -0.5548
## 3:                   -0.3469            -0.4378            -0.5615
## 4:                   -0.3257            -0.3734            -0.6885
## 5:                   -0.3442            -0.3727            -0.5140
## 6:                   -0.1041            -0.2397            -0.3414
##    fBodyGyro.mean...Z fBodyGyro.std...X fBodyGyro.std...Y
## 1:            -0.2559           -0.5167          -0.03351
## 2:            -0.3967           -0.6041          -0.53305
## 3:            -0.4181           -0.6151          -0.56889
## 4:            -0.6014           -0.5426          -0.65466
## 5:            -0.2131           -0.5294          -0.50268
## 6:            -0.2036           -0.5153          -0.33201
##    fBodyGyro.std...Z fBodyGyro.meanFreq...X fBodyGyro.meanFreq...Y
## 1:           -0.4366                0.01478               -0.06577
## 2:           -0.5599                0.00728               -0.04270
## 3:           -0.5459                0.03376               -0.03799
## 4:           -0.7165               -0.12715               -0.27467
## 5:           -0.4204               -0.04586               -0.01919
## 6:           -0.5122                0.09124                0.04163
##    fBodyGyro.meanFreq...Z fBodyAccMag.mean.. fBodyAccMag.std..
## 1:              0.0007733            -0.1286           -0.3980
## 2:              0.1397521            -0.3243           -0.5771
## 3:             -0.0445080            -0.2900           -0.4564
## 4:              0.1498515            -0.4508           -0.6512
## 5:              0.1674578            -0.3050           -0.5196
## 6:              0.3028749            -0.2014           -0.4217
##    fBodyAccMag.meanFreq.. fBodyBodyAccJerkMag.mean..
## 1:                 0.1906                   -0.05712
## 2:                 0.3932                   -0.16906
## 3:                 0.1135                   -0.18676
## 4:                 0.3821                   -0.31859
## 5:                 0.1499                   -0.26948
## 6:                 0.2001                   -0.05540
##    fBodyBodyAccJerkMag.std.. fBodyBodyAccJerkMag.meanFreq..
## 1:                  -0.10349                       0.093822
## 2:                  -0.16409                       0.207501
## 3:                  -0.08985                      -0.117164
## 4:                  -0.32046                       0.111486
## 5:                  -0.30569                      -0.004973
## 6:                  -0.09650                      -0.009229
##    fBodyBodyGyroMag.mean.. fBodyBodyGyroMag.std..
## 1:                 -0.1993                -0.3210
## 2:                 -0.5307                -0.6518
## 3:                 -0.5698                -0.6326
## 4:                 -0.6093                -0.5939
## 5:                 -0.4843                -0.5897
## 6:                 -0.3297                -0.5106
##    fBodyBodyGyroMag.meanFreq.. fBodyBodyGyroJerkMag.mean..
## 1:                     0.26884                     -0.3193
## 2:                     0.30528                     -0.5832
## 3:                     0.18095                     -0.6078
## 4:                     0.06973                     -0.7243
## 5:                     0.25062                     -0.5481
## 6:                     0.29311                     -0.3665
##    fBodyBodyGyroJerkMag.std.. fBodyBodyGyroJerkMag.meanFreq..
## 1:                    -0.3816                         0.19066
## 2:                    -0.5581                         0.12634
## 3:                    -0.5491                         0.04576
## 4:                    -0.7578                         0.26536
## 5:                    -0.4557                         0.05273
## 6:                    -0.4081                         0.10092
```

