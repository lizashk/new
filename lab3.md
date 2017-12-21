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
> head(data)
  X1 Інформація Про.надання.списків.для.нагородження. X42739 X01.12.1 надання..список..нагородження
1  2 Інформація                Про проблемні питання   42739  01-12/2             проблема, питання
2  3 Інформація                Про звернення громадян  42739  01-12/3          звернення, громадяни
3  4 Інформація                Про звернення громадян  42739  01-12/4          звернення, громадяни
4  5 Інформація                 Про тимчасовий розпис  42740  01-12/5             тимчасово, розпис
5  6 Інформація                    Список працівників  42740  01-11/6             список, працівник
6  7 Інформація                  Про роботу з кадрами  42740  01-12/7                 робота, кадри
  електронний Управління.капітального.будівництва.Сумської.ОДА
1 електронний Управління капітального будівництва Сумської ОДА
2    паперова Управління капітального будівництва Сумської ОДА
3    паперова Управління капітального будівництва Сумської ОДА
4 електронний Управління капітального будівництва Сумської ОДА
5    паперова Управління капітального будівництва Сумської ОДА
6 електронний Управління капітального будівництва Сумської ОДА
  Управління.капітального.будівництва.Сумської.ОДА.1 X42740 основна NA. NA..1 NA..2
1   Управління капітального будівництва Сумської ОДА  42740 основна  NA    NA    NA
2   Управління капітального будівництва Сумської ОДА  42740 основна  NA    NA    NA
3   Управління капітального будівництва Сумської ОДА  42740 основна  NA    NA    NA
4   Управління капітального будівництва Сумської ОДА  42741 фінанси  NA    NA    NA
5   Управління капітального будівництва Сумської ОДА  42741 основна  NA    NA    NA
6   Управління капітального будівництва Сумської ОДА  42741 основна  NA    NA    NA
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
