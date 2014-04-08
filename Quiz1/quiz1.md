Week 1 Quiz
===========

The due date for this quiz is Sun 13 Apr 2014 4:30 PM PDT.


Fix URL reading for knitr. See [Stackoverflow](http://stackoverflow.com/a/20003380).


```r
setInternet2(TRUE)
```



Question 1
----------
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here: 

[https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv](https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv)

and load the data into R. The code book, describing the variable names is here: 

[https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf](https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf)

How many housing units in this survey were worth more than $1,000,000?


```r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf"
f <- file.path(getwd(), "PUMSDataDict06.pdf")
download.file(url, f)
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
f <- file.path(getwd(), "ss06pid.csv")
download.file(url, f)
require(data.table)
```

```
## Loading required package: data.table
```

```r
dt <- data.table(read.csv(f))
setkey(dt, VAL)
dt[, .N, key(dt)]
```

```
##     VAL    N
##  1:  NA 2076
##  2:   1   75
##  3:   2   42
##  4:   3   33
##  5:   4   30
##  6:   5   26
##  7:   6   29
##  8:   7   23
##  9:   8   70
## 10:   9   99
## 11:  10  119
## 12:  11  152
## 13:  12  199
## 14:  13  233
## 15:  14  495
## 16:  15  483
## 17:  16  486
## 18:  17  357
## 19:  18  502
## 20:  19  232
## 21:  20  312
## 22:  21  164
## 23:  22  159
## 24:  23   47
## 25:  24   53
##     VAL    N
```



Question 2
----------
Use the data you loaded from Question 1. Consider the variable FES in the code book. Which of the "tidy data" principles does this variable violate?


```r
setkey(dt, FES)
dt[, .N, key(dt)]
```

```
##    FES    N
## 1:  NA 2445
## 2:   1 1730
## 3:   2  826
## 4:   3  236
## 5:   4  638
## 6:   5  151
## 7:   6   40
## 8:   7  305
## 9:   8  125
```



Question 3
----------
Download the Excel spreadsheet on Natural Gas Aquisition Program here: 

[https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx](https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx)

Read rows 18-23 and columns 7-15 into R and assign the result to a variable called:

`dat`

What is the value of:

`sum(dat$Zip*dat$Ext,na.rm=T)`

(original data source: [http://catalog.data.gov/dataset/natural-gas-acquisition-program](http://catalog.data.gov/dataset/natural-gas-acquisition-program))


```r
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx"
f <- file.path(getwd(), "DATA.gov_NGAP.xlsx")
download.file(url, f)
require(xlsx)
```

```
## Loading required package: xlsx
## Loading required package: rJava
```

```r
rows <- 18:23
cols <- 7:15
dat <- read.xlsx(f, colIndex = cols, rowIndex = rows)
```

```
## Error: could not find function "read.xlsx"
```

```r
sum(dat$Zip * dat$Ext, na.rm = T)
```

```
## Error: object 'dat' not found
```

