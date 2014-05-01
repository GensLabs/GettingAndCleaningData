Peer evaluations
================

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



My submission
-------------
My GitHub repository is https://github.com/benjamin-chan/GettingAndCleaningData.

Read my tidy dataset. Use this to compare to peers.


```r
f <- file.path("..", "DatasetHumanActivityRecognitionUsingSmartphones.txt")
dt0 <- data.table(read.table(f, header = TRUE))
```


Get structure of my dataset.


```r
str(dt0)
```

```
## Classes 'data.table' and 'data.frame':	11880 obs. of  11 variables:
##  $ subject         : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ activity        : Factor w/ 6 levels "LAYING","SITTING",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ featDomain      : Factor w/ 2 levels "Freq","Time": 2 2 2 2 2 2 2 2 2 2 ...
##  $ featAcceleration: Factor w/ 2 levels "Body","Gravity": NA NA NA NA NA NA NA NA NA NA ...
##  $ featInstrument  : Factor w/ 2 levels "Accelerometer",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ featJerk        : Factor w/ 1 level "Jerk": NA NA NA NA NA NA NA NA 1 1 ...
##  $ featMagnitude   : Factor w/ 1 level "Magnitude": NA NA NA NA NA NA 1 1 NA NA ...
##  $ featVariable    : Factor w/ 2 levels "Mean","SD": 1 1 1 2 2 2 1 2 1 1 ...
##  $ featAxis        : Factor w/ 3 levels "X","Y","Z": 1 2 3 1 2 3 NA NA 1 2 ...
##  $ count           : int  50 50 50 50 50 50 50 50 50 50 ...
##  $ average         : num  -0.0166 -0.0645 0.1487 -0.8735 -0.9511 ...
##  - attr(*, ".internal.selfref")=<externalptr>
```

```r
head(dt0)
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



Student 1
---------
GitHub repository is https://github.com/sdrdis/getdata_peer_assessment.

Read in tidy dataset.


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-da581c348504652cf6d52b48/972080/asst-3/320c2dc0ce3e11e3bb7387f31f11f941.txt"
dt1 <- data.table(read.table(url, header = TRUE))
```


Get structure of the dataset.


```r
str(dt1)
```

```
## Classes 'data.table' and 'data.frame':	180 obs. of  66 variables:
##  $ tBodyAcc.mean.X          : num  0.222 0.261 0.279 0.277 0.289 ...
##  $ tBodyAcc.mean.Y          : num  -0.04051 -0.00131 -0.01614 -0.01738 -0.00992 ...
##  $ tBodyAcc.mean.Z          : num  -0.113 -0.105 -0.111 -0.111 -0.108 ...
##  $ tBodyAcc.std.X           : num  -0.928 -0.977 -0.996 -0.284 0.03 ...
##  $ tBodyAcc.std.Y           : num  -0.8368 -0.9226 -0.9732 0.1145 -0.0319 ...
##  $ tBodyAcc.std.Z           : num  -0.826 -0.94 -0.98 -0.26 -0.23 ...
##  $ tGravityAcc.mean.X       : num  -0.249 0.832 0.943 0.935 0.932 ...
##  $ tGravityAcc.mean.Y       : num  0.706 0.204 -0.273 -0.282 -0.267 ...
##  $ tGravityAcc.mean.Z       : num  0.4458 0.332 0.0135 -0.0681 -0.0621 ...
##  $ tGravityAcc.std.X        : num  -0.897 -0.968 -0.994 -0.977 -0.951 ...
##  $ tGravityAcc.std.Y        : num  -0.908 -0.936 -0.981 -0.971 -0.937 ...
##  $ tGravityAcc.std.Z        : num  -0.852 -0.949 -0.976 -0.948 -0.896 ...
##  $ tBodyAccJerk.mean.X      : num  0.0811 0.0775 0.0754 0.074 0.0542 ...
##  $ tBodyAccJerk.mean.Y      : num  0.003838 -0.000619 0.007976 0.028272 0.02965 ...
##  $ tBodyAccJerk.mean.Z      : num  0.01083 -0.00337 -0.00369 -0.00417 -0.01097 ...
##  $ tBodyAccJerk.std.X       : num  -0.9585 -0.9864 -0.9946 -0.1136 -0.0123 ...
##  $ tBodyAccJerk.std.Y       : num  -0.924 -0.981 -0.986 0.067 -0.102 ...
##  $ tBodyAccJerk.std.Z       : num  -0.955 -0.988 -0.992 -0.503 -0.346 ...
##  $ tBodyGyro.mean.X         : num  -0.0166 -0.0454 -0.024 -0.0418 -0.0351 ...
##  $ tBodyGyro.mean.Y         : num  -0.0645 -0.0919 -0.0594 -0.0695 -0.0909 ...
##  $ tBodyGyro.mean.Z         : num  0.1487 0.0629 0.0748 0.0849 0.0901 ...
##  $ tBodyGyro.std.X          : num  -0.874 -0.977 -0.987 -0.474 -0.458 ...
##  $ tBodyGyro.std.Y          : num  -0.9511 -0.9665 -0.9877 -0.0546 -0.1263 ...
##  $ tBodyGyro.std.Z          : num  -0.908 -0.941 -0.981 -0.344 -0.125 ...
##  $ tBodyGyroJerk.mean.X     : num  -0.1073 -0.0937 -0.0996 -0.09 -0.074 ...
##  $ tBodyGyroJerk.mean.Y     : num  -0.0415 -0.0402 -0.0441 -0.0398 -0.044 ...
##  $ tBodyGyroJerk.mean.Z     : num  -0.0741 -0.0467 -0.049 -0.0461 -0.027 ...
##  $ tBodyGyroJerk.std.X      : num  -0.919 -0.992 -0.993 -0.207 -0.487 ...
##  $ tBodyGyroJerk.std.Y      : num  -0.968 -0.99 -0.995 -0.304 -0.239 ...
##  $ tBodyGyroJerk.std.Z      : num  -0.958 -0.988 -0.992 -0.404 -0.269 ...
##  $ tBodyAccMag.mean         : num  -0.8419 -0.9485 -0.9843 -0.137 0.0272 ...
##  $ tBodyAccMag.std          : num  -0.7951 -0.9271 -0.9819 -0.2197 0.0199 ...
##  $ tGravityAccMag.mean      : num  -0.8419 -0.9485 -0.9843 -0.137 0.0272 ...
##  $ tGravityAccMag.std       : num  -0.7951 -0.9271 -0.9819 -0.2197 0.0199 ...
##  $ tBodyAccJerkMag.mean     : num  -0.9544 -0.9874 -0.9924 -0.1414 -0.0894 ...
##  $ tBodyAccJerkMag.std      : num  -0.9282 -0.9841 -0.9931 -0.0745 -0.0258 ...
##  $ tBodyGyroMag.mean        : num  -0.8748 -0.9309 -0.9765 -0.161 -0.0757 ...
##  $ tBodyGyroMag.std         : num  -0.819 -0.935 -0.979 -0.187 -0.226 ...
##  $ tBodyGyroJerkMag.mean    : num  -0.963 -0.992 -0.995 -0.299 -0.295 ...
##  $ tBodyGyroJerkMag.std     : num  -0.936 -0.988 -0.995 -0.325 -0.307 ...
##  $ fBodyAcc.mean.X          : num  -0.9391 -0.9796 -0.9952 -0.2028 0.0382 ...
##  $ fBodyAcc.mean.Y          : num  -0.86707 -0.94408 -0.97707 0.08971 0.00155 ...
##  $ fBodyAcc.mean.Z          : num  -0.883 -0.959 -0.985 -0.332 -0.226 ...
##  $ fBodyAcc.std.X           : num  -0.9244 -0.9764 -0.996 -0.3191 0.0243 ...
##  $ fBodyAcc.std.Y           : num  -0.834 -0.917 -0.972 0.056 -0.113 ...
##  $ fBodyAcc.std.Z           : num  -0.813 -0.934 -0.978 -0.28 -0.298 ...
##  $ fBodyAccJerk.mean.X      : num  -0.9571 -0.9866 -0.9946 -0.1705 -0.0277 ...
##  $ fBodyAccJerk.mean.Y      : num  -0.9225 -0.9816 -0.9854 -0.0352 -0.1287 ...
##  $ fBodyAccJerk.mean.Z      : num  -0.948 -0.986 -0.991 -0.469 -0.288 ...
##  $ fBodyAccJerk.std.X       : num  -0.9642 -0.9875 -0.9951 -0.1336 -0.0863 ...
##  $ fBodyAccJerk.std.Y       : num  -0.932 -0.983 -0.987 0.107 -0.135 ...
##  $ fBodyAccJerk.std.Z       : num  -0.961 -0.988 -0.992 -0.535 -0.402 ...
##  $ fBodyGyro.mean.X         : num  -0.85 -0.976 -0.986 -0.339 -0.352 ...
##  $ fBodyGyro.mean.Y         : num  -0.9522 -0.9758 -0.989 -0.1031 -0.0557 ...
##  $ fBodyGyro.mean.Z         : num  -0.9093 -0.9513 -0.9808 -0.2559 -0.0319 ...
##  $ fBodyGyro.std.X          : num  -0.882 -0.978 -0.987 -0.517 -0.495 ...
##  $ fBodyGyro.std.Y          : num  -0.9512 -0.9623 -0.9871 -0.0335 -0.1814 ...
##  $ fBodyGyro.std.Z          : num  -0.917 -0.944 -0.982 -0.437 -0.238 ...
##  $ fBodyAccMag.mean         : num  -0.8618 -0.9478 -0.9854 -0.1286 0.0966 ...
##  $ fBodyAccMag.std          : num  -0.798 -0.928 -0.982 -0.398 -0.187 ...
##  $ fBodyBodyAccJerkMag.mean : num  -0.9333 -0.9853 -0.9925 -0.0571 0.0262 ...
##  $ fBodyBodyAccJerkMag.std  : num  -0.922 -0.982 -0.993 -0.103 -0.104 ...
##  $ fBodyBodyGyroMag.mean    : num  -0.862 -0.958 -0.985 -0.199 -0.186 ...
##  $ fBodyBodyGyroMag.std     : num  -0.824 -0.932 -0.978 -0.321 -0.398 ...
##  $ fBodyBodyGyroJerkMag.mean: num  -0.942 -0.99 -0.995 -0.319 -0.282 ...
##  $ fBodyBodyGyroJerkMag.std : num  -0.933 -0.987 -0.995 -0.382 -0.392 ...
##  - attr(*, ".internal.selfref")=<externalptr>
```

```r
head(dt1)
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



Student 2
---------
GitHub repository is https://github.com/alvasvoboda/PeerAssessment.

Read in tidy dataset.


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-cd5fa9234072b2113411a5b6/972080/asst-3/45e306b0ce6211e38f91f1adc287d0b7.txt"
dt2 <- data.table(read.table(url, header = TRUE))
```


Get structure of the dataset.


```r
str(dt2)
```

```
## Classes 'data.table' and 'data.frame':	10299 obs. of  68 variables:
##  $ tBodyAcc_mean_X          : num  0.289 0.278 0.28 0.279 0.277 ...
##  $ tBodyAcc_mean_Y          : num  -0.0203 -0.0164 -0.0195 -0.0262 -0.0166 ...
##  $ tBodyAcc_mean_Z          : num  -0.133 -0.124 -0.113 -0.123 -0.115 ...
##  $ tBodyAcc_std_X           : num  -0.995 -0.998 -0.995 -0.996 -0.998 ...
##  $ tBodyAcc_std_Y           : num  -0.983 -0.975 -0.967 -0.983 -0.981 ...
##  $ tBodyAcc_std_Z           : num  -0.914 -0.96 -0.979 -0.991 -0.99 ...
##  $ tGravityAcc_mean_X       : num  0.963 0.967 0.967 0.968 0.968 ...
##  $ tGravityAcc_mean_Y       : num  -0.141 -0.142 -0.142 -0.144 -0.149 ...
##  $ tGravityAcc_mean_Z       : num  0.1154 0.1094 0.1019 0.0999 0.0945 ...
##  $ tGravityAcc_std_X        : num  -0.985 -0.997 -1 -0.997 -0.998 ...
##  $ tGravityAcc_std_Y        : num  -0.982 -0.989 -0.993 -0.981 -0.988 ...
##  $ tGravitybodyAcc_std_Z    : num  -0.878 -0.932 -0.993 -0.978 -0.979 ...
##  $ tBodyAccJerk_mean_X      : num  0.078 0.074 0.0736 0.0773 0.0734 ...
##  $ tBodyAccJerk_mean_Y      : num  0.005 0.00577 0.0031 0.02006 0.01912 ...
##  $ tBodyAccJerk_mean_Z      : num  -0.06783 0.02938 -0.00905 -0.00986 0.01678 ...
##  $ tBodyAccJerk_std_X       : num  -0.994 -0.996 -0.991 -0.993 -0.996 ...
##  $ tBodyAccJerk_std_Y       : num  -0.988 -0.981 -0.981 -0.988 -0.988 ...
##  $ tBodyAccJerk_std_Z       : num  -0.994 -0.992 -0.99 -0.993 -0.992 ...
##  $ tBodyGyro_mean_X         : num  -0.0061 -0.0161 -0.0317 -0.0434 -0.034 ...
##  $ tBodyGyro_mean_Y         : num  -0.0314 -0.0839 -0.1023 -0.0914 -0.0747 ...
##  $ tBodyGyro_mean_Z         : num  0.1077 0.1006 0.0961 0.0855 0.0774 ...
##  $ tBodyGyro_std_X          : num  -0.985 -0.983 -0.976 -0.991 -0.985 ...
##  $ tBodyGyro_std_Y          : num  -0.977 -0.989 -0.994 -0.992 -0.992 ...
##  $ tBodyGyro_std_Z          : num  -0.992 -0.989 -0.986 -0.988 -0.987 ...
##  $ tBodyGyroJerk_mean_X     : num  -0.0992 -0.1105 -0.1085 -0.0912 -0.0908 ...
##  $ tBodyGyroJerk_mean_Y     : num  -0.0555 -0.0448 -0.0424 -0.0363 -0.0376 ...
##  $ tBodyGyroJerk_mean_Z     : num  -0.062 -0.0592 -0.0558 -0.0605 -0.0583 ...
##  $ tBodyGyroJerk_std_X      : num  -0.992 -0.99 -0.988 -0.991 -0.991 ...
##  $ tBodyGyroJerk_std_Y      : num  -0.993 -0.997 -0.996 -0.997 -0.996 ...
##  $ tBodyGyroJerk_std_Z      : num  -0.992 -0.994 -0.992 -0.993 -0.995 ...
##  $ tBodyAccMag_mean         : num  -0.959 -0.979 -0.984 -0.987 -0.993 ...
##  $ tBodyAccMag_std          : num  -0.951 -0.976 -0.988 -0.986 -0.991 ...
##  $ tGravityAccMag_mean      : num  -0.959 -0.979 -0.984 -0.987 -0.993 ...
##  $ tGravityAccMag_std       : num  -0.951 -0.976 -0.988 -0.986 -0.991 ...
##  $ tBodyAccJerkMag_mean     : num  -0.993 -0.991 -0.989 -0.993 -0.993 ...
##  $ tBodyAccJerkMag_std      : num  -0.994 -0.992 -0.99 -0.993 -0.996 ...
##  $ tBodyGyroMag_mean        : num  -0.969 -0.981 -0.976 -0.982 -0.985 ...
##  $ tBodyGyroMag_std         : num  -0.964 -0.984 -0.986 -0.987 -0.989 ...
##  $ tBodyGyroJerkMag_mean    : num  -0.994 -0.995 -0.993 -0.996 -0.996 ...
##  $ tBodyGyroJerkMag_std     : num  -0.991 -0.996 -0.995 -0.995 -0.995 ...
##  $ fBodyAcc_mean_X          : num  -0.995 -0.997 -0.994 -0.995 -0.997 ...
##  $ fBodyAcc_mean_Y          : num  -0.983 -0.977 -0.973 -0.984 -0.982 ...
##  $ fBodyAcc_mean_Z          : num  -0.939 -0.974 -0.983 -0.991 -0.988 ...
##  $ fBodyAcc_std_X           : num  -0.995 -0.999 -0.996 -0.996 -0.999 ...
##  $ fBodyAcc_std_Y           : num  -0.983 -0.975 -0.966 -0.983 -0.98 ...
##  $ fBodyAcc_std_Z           : num  -0.906 -0.955 -0.977 -0.99 -0.992 ...
##  $ fBodyAccJerk_mean_X      : num  -0.992 -0.995 -0.991 -0.994 -0.996 ...
##  $ fBodyAccJerk_mean_Y      : num  -0.987 -0.981 -0.982 -0.989 -0.989 ...
##  $ fBodyAccJerk_mean_Z      : num  -0.99 -0.99 -0.988 -0.991 -0.991 ...
##  $ fBodyAccJerk_std_X       : num  -0.996 -0.997 -0.991 -0.991 -0.997 ...
##  $ fBodyAccJerk_std_Y       : num  -0.991 -0.982 -0.981 -0.987 -0.989 ...
##  $ fBodyAccJerk_std_Z       : num  -0.997 -0.993 -0.99 -0.994 -0.993 ...
##  $ fBodyGyro_mean_X         : num  -0.987 -0.977 -0.975 -0.987 -0.982 ...
##  $ fBodyGyro_mean_Y         : num  -0.982 -0.993 -0.994 -0.994 -0.993 ...
##  $ fBodyGyro_mean_Z         : num  -0.99 -0.99 -0.987 -0.987 -0.989 ...
##  $ fBodyGyro_std_X          : num  -0.985 -0.985 -0.977 -0.993 -0.986 ...
##  $ fBodyGyro_std_Y          : num  -0.974 -0.987 -0.993 -0.992 -0.992 ...
##  $ fBodyGyro_std_Z          : num  -0.994 -0.99 -0.987 -0.989 -0.988 ...
##  $ fBodyAccMag_mean         : num  -0.952 -0.981 -0.988 -0.988 -0.994 ...
##  $ fBodyAccMag_std          : num  -0.956 -0.976 -0.989 -0.987 -0.99 ...
##  $ fBodyBodyAccJerkMag_mean : num  -0.994 -0.99 -0.989 -0.993 -0.996 ...
##  $ fBodyBodyAccJerkMag_std  : num  -0.994 -0.992 -0.991 -0.992 -0.994 ...
##  $ fBodyBodyGyroMag_mean    : num  -0.98 -0.988 -0.989 -0.989 -0.991 ...
##  $ fBodyBodyGyroMag_std     : num  -0.961 -0.983 -0.986 -0.988 -0.989 ...
##  $ fBodyBodyGyroJerkMag_mean: num  0.375 -0.72 -0.872 -0.511 -0.831 ...
##  $ fBodyBodyGyroJerkMag_std : num  -0.992 -0.996 -0.995 -0.995 -0.995 ...
##  $ y_label                  : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ subject_id               : int  5 5 5 5 5 5 5 5 5 5 ...
##  - attr(*, ".internal.selfref")=<externalptr>
```

```r
head(dt2)
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



Student 3
---------
GitHub repository is https://github.com/rafalohn/tidyData.

Read in tidy dataset.


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-86e8069e2ecc3248b76506ff/972080/asst-3/b00980a0ce5d11e39688c70635ebeff0.txt"
dt3 <- data.table(read.csv(url))
```


Get structure of the dataset.


```r
str(dt3)
```

```
## Classes 'data.table' and 'data.frame':	79 obs. of  127 variables:
##  $ X                     : Factor w/ 79 levels "fBodyAcc-mean()-X",..: 40 41 42 43 44 45 72 73 74 75 ...
##  $ X1.WALKING            : num  0.2773 -0.0174 -0.1111 -0.2837 0.1145 ...
##  $ X3.WALKING            : num  0.2756 -0.0172 -0.1127 -0.3604 -0.0699 ...
##  $ X5.WALKING            : num  0.2778 -0.0173 -0.1077 -0.2941 0.0767 ...
##  $ X6.WALKING            : num  0.2837 -0.0169 -0.1103 -0.2965 0.1642 ...
##  $ X7.WALKING            : num  0.2756 -0.0187 -0.1109 -0.3272 -0.0773 ...
##  $ X8.WALKING            : num  0.2747 -0.0187 -0.1073 -0.1736 0.3808 ...
##  $ X11.WALKING           : num  0.2718 -0.0166 -0.1061 -0.4228 -0.0522 ...
##  $ X14.WALKING           : num  0.272 -0.0218 -0.1068 -0.4026 -0.0536 ...
##  $ X15.WALKING           : num  0.2739 -0.0171 -0.1076 -0.328 0.1389 ...
##  $ X16.WALKING           : num  0.276 -0.0204 -0.1088 -0.4047 -0.3146 ...
##  $ X17.WALKING           : num  0.2723 -0.0185 -0.1098 -0.3195 -0.0176 ...
##  $ X19.WALKING           : num  0.2739 -0.0192 -0.1227 -0.0489 0.1818 ...
##  $ X21.WALKING           : num  0.2792 -0.0182 -0.1043 -0.2978 0.0541 ...
##  $ X22.WALKING           : num  0.27886 -0.01672 -0.10711 -0.00866 0.10038 ...
##  $ X23.WALKING           : num  0.2732 -0.0184 -0.1134 -0.3135 -0.119 ...
##  $ X25.WALKING           : num  0.279 -0.0186 -0.1087 -0.596 -0.1619 ...
##  $ X26.WALKING           : num  0.2793 -0.0154 -0.1089 -0.3402 -0.1364 ...
##  $ X27.WALKING           : num  0.2768 -0.0166 -0.1128 -0.3485 -0.1873 ...
##  $ X28.WALKING           : num  0.2812 -0.0157 -0.1037 -0.293 -0.1179 ...
##  $ X29.WALKING           : num  0.272 -0.0163 -0.1066 -0.1743 -0.0918 ...
##  $ X30.WALKING           : num  0.2764 -0.0176 -0.0986 -0.3464 -0.1736 ...
##  $ X1.WALKING_UPSTAIRS   : num  0.25546 -0.02395 -0.0973 -0.35471 -0.00232 ...
##  $ X3.WALKING_UPSTAIRS   : num  0.2608 -0.0324 -0.1101 -0.3131 0.0116 ...
##  $ X5.WALKING_UPSTAIRS   : num  0.2685 -0.0325 -0.1075 -0.0457 0.185 ...
##  $ X6.WALKING_UPSTAIRS   : num  0.2682 -0.0272 -0.1221 -0.0501 0.1893 ...
##  $ X7.WALKING_UPSTAIRS   : num  0.2487 -0.0276 -0.1438 -0.2949 -0.3262 ...
##  $ X8.WALKING_UPSTAIRS   : num  0.2589 -0.0282 -0.1151 -0.1717 0.3488 ...
##  $ X11.WALKING_UPSTAIRS  : num  0.2638 -0.0303 -0.1068 -0.2388 -0.1032 ...
##  $ X14.WALKING_UPSTAIRS  : num  0.2624 -0.0204 -0.1123 -0.3094 0.3072 ...
##  $ X15.WALKING_UPSTAIRS  : num  0.27019 -0.02875 -0.11695 -0.0261 -0.00407 ...
##  $ X16.WALKING_UPSTAIRS  : num  0.256 -0.0144 -0.1241 -0.3971 -0.1637 ...
##  $ X17.WALKING_UPSTAIRS  : num  0.2526 -0.0229 -0.1213 -0.063 -0.0132 ...
##  $ X19.WALKING_UPSTAIRS  : num  0.2421 -0.0304 -0.1511 -0.1287 0.1763 ...
##  $ X21.WALKING_UPSTAIRS  : num  0.2652 -0.0237 -0.1254 -0.2408 0.1098 ...
##  $ X22.WALKING_UPSTAIRS  : num  0.2484 -0.0269 -0.1176 0.0836 0.2501 ...
##  $ X23.WALKING_UPSTAIRS  : num  0.25 -0.0324 -0.1269 -0.2439 0.0448 ...
##  $ X25.WALKING_UPSTAIRS  : num  0.278 -0.027 -0.126 -0.46 -0.223 ...
##  $ X26.WALKING_UPSTAIRS  : num  0.2727 -0.0282 -0.1219 -0.169 -0.0492 ...
##  $ X27.WALKING_UPSTAIRS  : num  0.2658 -0.0201 -0.1235 -0.2954 -0.095 ...
##  $ X28.WALKING_UPSTAIRS  : num  0.262 -0.0279 -0.1215 -0.2421 -0.1468 ...
##  $ X29.WALKING_UPSTAIRS  : num  0.2654 -0.0299 -0.118 -0.0868 -0.1221 ...
##  $ X30.WALKING_UPSTAIRS  : num  0.2714 -0.0253 -0.1247 -0.3505 -0.1273 ...
##  $ X1.WALKING_DOWNSTAIRS : num  0.28919 -0.00992 -0.10757 0.03004 -0.03194 ...
##  $ X3.WALKING_DOWNSTAIRS : num  0.2924 -0.0194 -0.1161 -0.0574 -0.0332 ...
##  $ X5.WALKING_DOWNSTAIRS : num  0.2935 -0.0085 -0.1003 0.275 0.0908 ...
##  $ X6.WALKING_DOWNSTAIRS : num  0.277 -0.0195 -0.1072 0.3837 0.3602 ...
##  $ X7.WALKING_DOWNSTAIRS : num  0.2803 -0.0166 -0.0969 0.0661 -0.1382 ...
##  $ X8.WALKING_DOWNSTAIRS : num  0.2835 -0.0211 -0.1076 0.0241 0.3438 ...
##  $ X11.WALKING_DOWNSTAIRS: num  0.2916 -0.0178 -0.1111 0.1425 0.0708 ...
##  $ X14.WALKING_DOWNSTAIRS: num  0.2934 -0.02 -0.0928 0.0178 0.3789 ...
##  $ X15.WALKING_DOWNSTAIRS: num  0.2802 -0.00563 -0.11053 0.40647 0.18653 ...
##  $ X16.WALKING_DOWNSTAIRS: num  0.2956 -0.0184 -0.1199 0.2073 -0.147 ...
##  $ X17.WALKING_DOWNSTAIRS: num  0.2939 -0.0167 -0.0892 0.1877 -0.0608 ...
##  $ X19.WALKING_DOWNSTAIRS: num  0.2627 -0.0146 -0.1337 0.6269 0.5148 ...
##  $ X21.WALKING_DOWNSTAIRS: num  0.3015 -0.0173 -0.0982 0.2243 0.3013 ...
##  $ X22.WALKING_DOWNSTAIRS: num  0.2845 -0.0198 -0.1074 0.3486 0.2398 ...
##  $ X23.WALKING_DOWNSTAIRS: num  0.2899 -0.0162 -0.0988 0.0441 0.1083 ...
##  $ X25.WALKING_DOWNSTAIRS: num  0.291 -0.021 -0.107 -0.254 -0.141 ...
##  $ X26.WALKING_DOWNSTAIRS: num  0.2793 -0.0126 -0.1064 0.174 -0.0116 ...
##  $ X27.WALKING_DOWNSTAIRS: num  0.2975 -0.0136 -0.1128 0.1674 -0.098 ...
##  $ X28.WALKING_DOWNSTAIRS: num  0.294 -0.022 -0.109 0.126 0.156 ...
##  $ X29.WALKING_DOWNSTAIRS: num  0.2931 -0.0149 -0.0981 0.1674 -0.1225 ...
##  $ X30.WALKING_DOWNSTAIRS: num  0.2832 -0.0174 -0.1 -0.0578 -0.0273 ...
##  $ X1.SITTING            : num  0.26124 -0.00131 -0.10454 -0.97723 -0.92262 ...
##  $ X3.SITTING            : num  0.2572 -0.0035 -0.0984 -0.971 -0.8566 ...
##  $ X5.SITTING            : num  0.2737 -0.0099 -0.1085 -0.9809 -0.9043 ...
##  $ X6.SITTING            : num  0.2768 -0.0146 -0.1101 -0.9802 -0.9237 ...
##  $ X7.SITTING            : num  0.2847 -0.0146 -0.1225 -0.9727 -0.9095 ...
##  $ X8.SITTING            : num  0.26749 -0.00673 -0.10446 -0.97903 -0.92733 ...
##  $ X11.SITTING           : num  0.2766 -0.0149 -0.1128 -0.9828 -0.9214 ...
##  $ X14.SITTING           : num  0.27999 -0.00871 -0.1004 -0.9763 -0.91494 ...
##  $ X15.SITTING           : num  0.2729 -0.0117 -0.1137 -0.9871 -0.9224 ...
##  $ X16.SITTING           : num  0.2808 -0.0102 -0.0891 -0.9868 -0.9516 ...
##  $ X17.SITTING           : num  0.2774 -0.0142 -0.1136 -0.9944 -0.9619 ...
##  $ X19.SITTING           : num  0.2738 -0.0167 -0.1087 -0.9764 -0.9504 ...
##  $ X21.SITTING           : num  0.2775 -0.0144 -0.1121 -0.9918 -0.9669 ...
##  $ X22.SITTING           : num  0.2736 -0.0123 -0.1058 -0.9785 -0.9281 ...
##  $ X23.SITTING           : num  0.2734 -0.0134 -0.1038 -0.9876 -0.9349 ...
##  $ X25.SITTING           : num  0.2785 -0.0148 -0.1092 -0.9919 -0.9475 ...
##  $ X26.SITTING           : num  0.25824 -0.00713 -0.09744 -0.97977 -0.9408 ...
##  $ X27.SITTING           : num  0.2739 -0.0155 -0.1055 -0.9886 -0.9716 ...
##  $ X28.SITTING           : num  0.277 -0.0185 -0.1115 -0.9833 -0.9399 ...
##  $ X29.SITTING           : num  0.2772 -0.0166 -0.1104 -0.9907 -0.9632 ...
##  $ X30.SITTING           : num  0.26834 -0.00805 -0.09952 -0.98362 -0.93786 ...
##  $ X1.STANDING           : num  0.2789 -0.0161 -0.1106 -0.9958 -0.9732 ...
##  $ X3.STANDING           : num  0.28 -0.0143 -0.1016 -0.9667 -0.8934 ...
##  $ X5.STANDING           : num  0.283 -0.007 -0.102 -0.969 -0.869 ...
##  $ X6.STANDING           : num  0.2803 -0.0181 -0.1122 -0.9818 -0.9215 ...
##  $ X7.STANDING           : num  0.2827 -0.0146 -0.0998 -0.9793 -0.9234 ...
##  $ X8.STANDING           : num  0.2796 -0.0148 -0.1061 -0.9888 -0.9385 ...
##  $ X11.STANDING          : num  0.2777 -0.0172 -0.1087 -0.995 -0.9642 ...
##  $ X14.STANDING          : num  0.2805 -0.0152 -0.1038 -0.9733 -0.9285 ...
##  $ X15.STANDING          : num  0.2789 -0.0184 -0.1059 -0.9889 -0.9319 ...
##  $ X16.STANDING          : num  0.2835 -0.0166 -0.1037 -0.9891 -0.9603 ...
##  $ X17.STANDING          : num  0.2779 -0.0174 -0.1114 -0.9912 -0.9682 ...
##  $ X19.STANDING          : num  0.2782 -0.0154 -0.109 -0.9899 -0.9444 ...
##  $ X21.STANDING          : num  0.277 -0.0167 -0.1104 -0.9814 -0.9436 ...
##  $ X22.STANDING          : num  0.2791 -0.0159 -0.105 -0.985 -0.9227 ...
##   [list output truncated]
##  - attr(*, ".internal.selfref")=<externalptr>
```

```r
head(dt3)
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



Student 4
---------
GitHub repository is https://github.com/glynn/GACD_PeerAssessment.

Read in tidy dataset.


```r
url <- "https://s3.amazonaws.com/coursera-uploads/user-bd605606b2bf83039906529c/972080/asst-3/1cf35bf0ce3b11e3a98a0101a16d1137.txt"
dt4 <- data.table(read.table(url, header = TRUE))
```


Get structure of the dataset.


```r
str(dt4)
```

```
## Classes 'data.table' and 'data.frame':	180 obs. of  81 variables:
##  $ subject.id                     : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ activity                       : Factor w/ 6 levels "LAYING","SITTING",..: 4 4 4 4 4 4 4 4 4 4 ...
##  $ tBodyAcc.mean...X              : num  0.277 0.276 0.276 0.279 0.278 ...
##  $ tBodyAcc.mean...Y              : num  -0.0174 -0.0186 -0.0172 -0.0148 -0.0173 ...
##  $ tBodyAcc.mean...Z              : num  -0.111 -0.106 -0.113 -0.111 -0.108 ...
##  $ tBodyAcc.std...X               : num  -0.284 -0.424 -0.36 -0.441 -0.294 ...
##  $ tBodyAcc.std...Y               : num  0.1145 -0.0781 -0.0699 -0.0788 0.0767 ...
##  $ tBodyAcc.std...Z               : num  -0.26 -0.425 -0.387 -0.586 -0.457 ...
##  $ tGravityAcc.mean...X           : num  0.935 0.913 0.937 0.964 0.973 ...
##  $ tGravityAcc.mean...Y           : num  -0.2822 -0.3466 -0.262 -0.0859 -0.1004 ...
##  $ tGravityAcc.mean...Z           : num  -0.0681 0.08473 -0.13811 0.12776 0.00248 ...
##  $ tGravityAcc.std...X            : num  -0.977 -0.973 -0.978 -0.984 -0.979 ...
##  $ tGravityAcc.std...Y            : num  -0.971 -0.972 -0.962 -0.968 -0.962 ...
##  $ tGravityAcc.std...Z            : num  -0.948 -0.972 -0.952 -0.963 -0.965 ...
##  $ tBodyAccJerk.mean...X          : num  0.074 0.0618 0.0815 0.0784 0.0846 ...
##  $ tBodyAccJerk.mean...Y          : num  0.02827 0.01825 0.01006 0.00296 -0.01632 ...
##  $ tBodyAccJerk.mean...Z          : num  -4.17e-03 7.90e-03 -5.62e-03 -7.68e-04 8.32e-05 ...
##  $ tBodyAccJerk.std...X           : num  -0.114 -0.278 -0.269 -0.297 -0.303 ...
##  $ tBodyAccJerk.std...Y           : num  0.067 -0.0166 -0.045 -0.2212 -0.091 ...
##  $ tBodyAccJerk.std...Z           : num  -0.503 -0.586 -0.529 -0.751 -0.613 ...
##  $ tBodyGyro.mean...X             : num  -0.0418 -0.053 -0.0256 -0.0318 -0.0489 ...
##  $ tBodyGyro.mean...Y             : num  -0.0695 -0.0482 -0.0779 -0.0727 -0.069 ...
##  $ tBodyGyro.mean...Z             : num  0.0849 0.0828 0.0813 0.0806 0.0815 ...
##  $ tBodyGyro.std...X              : num  -0.474 -0.562 -0.572 -0.501 -0.491 ...
##  $ tBodyGyro.std...Y              : num  -0.0546 -0.5385 -0.5638 -0.6654 -0.5046 ...
##  $ tBodyGyro.std...Z              : num  -0.344 -0.481 -0.477 -0.663 -0.319 ...
##  $ tBodyGyroJerk.mean...X         : num  -0.09 -0.0819 -0.0952 -0.1153 -0.0888 ...
##  $ tBodyGyroJerk.mean...Y         : num  -0.0398 -0.0538 -0.0388 -0.0393 -0.045 ...
##  $ tBodyGyroJerk.mean...Z         : num  -0.0461 -0.0515 -0.0504 -0.0551 -0.0483 ...
##  $ tBodyGyroJerk.std...X          : num  -0.207 -0.39 -0.386 -0.492 -0.358 ...
##  $ tBodyGyroJerk.std...Y          : num  -0.304 -0.634 -0.639 -0.807 -0.571 ...
##  $ tBodyGyroJerk.std...Z          : num  -0.404 -0.435 -0.537 -0.64 -0.158 ...
##  $ tBodyAccMag.mean..             : num  -0.137 -0.29 -0.255 -0.312 -0.158 ...
##  $ tBodyAccMag.std..              : num  -0.22 -0.423 -0.328 -0.528 -0.377 ...
##  $ tGravityAccMag.mean..          : num  -0.137 -0.29 -0.255 -0.312 -0.158 ...
##  $ tGravityAccMag.std..           : num  -0.22 -0.423 -0.328 -0.528 -0.377 ...
##  $ tBodyAccJerkMag.mean..         : num  -0.141 -0.281 -0.28 -0.367 -0.288 ...
##  $ tBodyAccJerkMag.std..          : num  -0.0745 -0.1642 -0.1399 -0.3169 -0.2822 ...
##  $ tBodyGyroMag.mean..            : num  -0.161 -0.447 -0.466 -0.498 -0.356 ...
##  $ tBodyGyroMag.std..             : num  -0.187 -0.553 -0.562 -0.553 -0.492 ...
##  $ tBodyGyroJerkMag.mean..        : num  -0.299 -0.548 -0.566 -0.681 -0.445 ...
##  $ tBodyGyroJerkMag.std..         : num  -0.325 -0.558 -0.567 -0.73 -0.489 ...
##  $ fBodyAcc.mean...X              : num  -0.203 -0.346 -0.317 -0.427 -0.288 ...
##  $ fBodyAcc.mean...Y              : num  0.08971 -0.0219 -0.0813 -0.1494 0.00946 ...
##  $ fBodyAcc.mean...Z              : num  -0.332 -0.454 -0.412 -0.631 -0.49 ...
##  $ fBodyAcc.std...X               : num  -0.319 -0.458 -0.379 -0.447 -0.298 ...
##  $ fBodyAcc.std...Y               : num  0.056 -0.1692 -0.124 -0.1018 0.0426 ...
##  $ fBodyAcc.std...Z               : num  -0.28 -0.455 -0.423 -0.594 -0.483 ...
##  $ fBodyAcc.meanFreq...X          : num  -0.208 -0.146 -0.247 -0.139 -0.322 ...
##  $ fBodyAcc.meanFreq...Y          : num  0.11309 0.19859 0.17174 0.01235 -0.00204 ...
##  $ fBodyAcc.meanFreq...Z          : num  0.0497 0.0689 0.0749 -0.0788 0.0247 ...
##  $ fBodyAccJerk.mean...X          : num  -0.171 -0.305 -0.305 -0.359 -0.345 ...
##  $ fBodyAccJerk.mean...Y          : num  -0.0352 -0.0788 -0.1405 -0.2796 -0.1811 ...
##  $ fBodyAccJerk.mean...Z          : num  -0.469 -0.555 -0.514 -0.729 -0.59 ...
##  $ fBodyAccJerk.std...X           : num  -0.134 -0.314 -0.297 -0.297 -0.321 ...
##  $ fBodyAccJerk.std...Y           : num  0.10674 -0.01533 -0.00561 -0.2099 -0.05452 ...
##  $ fBodyAccJerk.std...Z           : num  -0.535 -0.616 -0.544 -0.772 -0.633 ...
##  $ fBodyAccJerk.meanFreq...X      : num  -0.2093 -0.0727 -0.216 -0.1353 -0.3594 ...
##  $ fBodyAccJerk.meanFreq...Y      : num  -0.386 -0.264 -0.259 -0.386 -0.534 ...
##  $ fBodyAccJerk.meanFreq...Z      : num  -0.186 -0.255 -0.347 -0.326 -0.344 ...
##  $ fBodyGyro.mean...X             : num  -0.339 -0.43 -0.438 -0.373 -0.373 ...
##  $ fBodyGyro.mean...Y             : num  -0.103 -0.555 -0.562 -0.688 -0.514 ...
##  $ fBodyGyro.mean...Z             : num  -0.256 -0.397 -0.418 -0.601 -0.213 ...
##  $ fBodyGyro.std...X              : num  -0.517 -0.604 -0.615 -0.543 -0.529 ...
##  $ fBodyGyro.std...Y              : num  -0.0335 -0.533 -0.5689 -0.6547 -0.5027 ...
##  $ fBodyGyro.std...Z              : num  -0.437 -0.56 -0.546 -0.716 -0.42 ...
##  $ fBodyGyro.meanFreq...X         : num  0.01478 0.00728 0.03376 -0.12715 -0.04586 ...
##  $ fBodyGyro.meanFreq...Y         : num  -0.0658 -0.0427 -0.038 -0.2747 -0.0192 ...
##  $ fBodyGyro.meanFreq...Z         : num  0.000773 0.139752 -0.044508 0.149852 0.167458 ...
##  $ fBodyAccMag.mean..             : num  -0.129 -0.324 -0.29 -0.451 -0.305 ...
##  $ fBodyAccMag.std..              : num  -0.398 -0.577 -0.456 -0.651 -0.52 ...
##  $ fBodyAccMag.meanFreq..         : num  0.191 0.393 0.113 0.382 0.15 ...
##  $ fBodyBodyAccJerkMag.mean..     : num  -0.0571 -0.1691 -0.1868 -0.3186 -0.2695 ...
##  $ fBodyBodyAccJerkMag.std..      : num  -0.1035 -0.1641 -0.0899 -0.3205 -0.3057 ...
##  $ fBodyBodyAccJerkMag.meanFreq.. : num  0.09382 0.2075 -0.11716 0.11149 -0.00497 ...
##  $ fBodyBodyGyroMag.mean..        : num  -0.199 -0.531 -0.57 -0.609 -0.484 ...
##  $ fBodyBodyGyroMag.std..         : num  -0.321 -0.652 -0.633 -0.594 -0.59 ...
##  $ fBodyBodyGyroMag.meanFreq..    : num  0.2688 0.3053 0.1809 0.0697 0.2506 ...
##  $ fBodyBodyGyroJerkMag.mean..    : num  -0.319 -0.583 -0.608 -0.724 -0.548 ...
##  $ fBodyBodyGyroJerkMag.std..     : num  -0.382 -0.558 -0.549 -0.758 -0.456 ...
##  $ fBodyBodyGyroJerkMag.meanFreq..: num  0.1907 0.1263 0.0458 0.2654 0.0527 ...
##  - attr(*, ".internal.selfref")=<externalptr>
```

```r
head(dt4)
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

