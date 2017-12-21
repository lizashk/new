1.	За допомогою download.file() завантажте любий excel файл з порталу http://data.gov.ua та зчитайте його (xls, xlsx – бінарні формати, тому встановить mode = “wb”. Виведіть перші 6 строк отриманого фрейму даних.
```r
# change locale to ukrainian
> fileURL <- "http://data.gov.ua/file/153481/download?token=kJ17wUAF"
> Sys.setlocale(locale = "ukrainian")
[1] ""
Warning message:
In Sys.setlocale(locale = "ukrainian") :
  OS reports request to set locale to "ukrainian" cannot be honored
> Sys.setlocale(locale = "uk_UA")
[1] "uk_UA/uk_UA/uk_UA/C/uk_UA/en_US.UTF-8"
> fileURL <- "http://data.gov.ua/file/153481/download?token=kJ17wUAF"
> download.file(fileURL, destfile = "data.xls", mode = "wb")
trying URL 'http://data.gov.ua/file/153481/download?token=kJ17wUAF'
Content type 'application/vnd.ms-excel' length 31744 bytes (31 KB)
==================================================
downloaded 31 KB
> data <- read.xlsx("data.xls", sheetIndex = 1, header = TRUE)
> head(data, 20)
# A tibble: 20 x 7
                                                                                    Інформація
                                                                                         <chr>
 1                                                про надходження доходів до обласного бюджету
 2                                                                         Рівненської області
 3                                                                                        <NA>
 4                                                                        станом на 15.12.2017
 5                                                                                        <NA>
 6                                                                                        <NA>
 7                                                                                        <NA>
 8                                                                                        <NA>
 9                                                                                        <NA>
10                                                                                        <NA>
11                                                                                        <NA>
12                                                                 Власні та закріплені доходи
13                                                                              Базова дотація
14                       "Додаткова дотація на утримання закладів освіти та охорони здоров\"я"
15                                                                      Cтабілізаційна дотація
16                                                     Субвенції з державного бюджету - всього
17                                                                              в тому числі :
18 - на виплату допомоги сім'ям з дітьми, малозабезпеченим сім'ям, інвалідам з дитинства, дітя
19                                                                         - освітня субвенція
20                                                                         - медична субвенція
# ... with 6 more variables: X__1 <chr>, X__2 <chr>, X__3 <chr>, X__4 <chr>, X__5 <chr>,
#   X__6 <chr>
```
2.	За допомогою download.file() завантажте файл getdata_data_ss06hid.csv за посиланням https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv та завантажте дані в R. Code book, що пояснює значення змінних знаходиться за посиланням https://www.dropbox.com/s/dijv0rlwo4mryv5/PUMSDataDict06.pdf?dl=0  Необхідно знайти, скільки property мають value $1000000+
```r
> download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv", destfile = "data.csv")
trying URL 'https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv'
Content type 'text/csv' length 4246554 bytes (4.0 MB)
downloaded 4.0 MB
> data <- read.csv("data.csv")
# Get only the VAL column
> values <- data$VAL
# Create a list, where for values that are non-NA and equal 24 (in the Code book it is said 24 stands for $1000000+) say TRUE, else - NA
> a <- lapply(values, function(x) if (!is.na(x) && x==24) TRUE else NA)
# Filter out the NA values and get the count of all TRUE values 
> length(a[!is.na(a)])
[1] 53
```
3.	Зчитайте xml файл за посиланням http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml Скільки ресторанів мають zipcode 21231?
```r
> url <- "http://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml"
> doc <- xmlTreeParse(url,useInternal=TRUE)
> rootNode <- xmlRoot(doc)
> length(xpathApply(rootNode, '//row/row/zipcode[text()="21231"]'))
[1] 127
```
