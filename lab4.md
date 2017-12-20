1.	Завантажте файл з даними за посиланням https://dcc.ligo.org/public/0146/P1700337/001/H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5 
```r
> download.file(url="https://dcc.ligo.org/public/0146/P1700337/001/H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5", destfile = "data.hdf5", mode="wb")
trying URL 'https://dcc.ligo.org/public/0146/P1700337/001/H-H1_LOSC_C00_4_V1-1187006834-4096.hdf5'
Content type 'text/plain; charset=UTF-8' length 125217658 bytes (119.4 MB)
downloaded 119.4 MB
```
2.	Встановить в R пакет для роботи з HDF5 файлами.
```r
> source("http://bioconductor.org/biocLite.R")
Installing package into ‘C:/Users/Andriy/Documents/R/win-library/3.4’
(as ‘lib’ is unspecified)
trying URL 'https://bioconductor.org/packages/3.6/bioc/bin/windows/contrib/3.4/BiocInstaller_1.28.0.zip'
Content type 'application/zip' length 130329 bytes (127 KB)
downloaded 127 KB

package ‘BiocInstaller’ successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\Andriy\AppData\Local\Temp\Rtmp8iXzWT\downloaded_packages
Bioconductor version 3.6 (BiocInstaller
  1.28.0), ?biocLite for help
> biocLite("rhdf5")
BioC_mirror: https://bioconductor.org
Using Bioconductor 3.6 (BiocInstaller 1.28.0),
  R 3.4.3 (2017-11-30).
Installing package(s) ‘rhdf5’
also installing the dependency ‘zlibbioc’

trying URL 'https://bioconductor.org/packages/3.6/bioc/bin/windows/contrib/3.4/zlibbioc_1.24.0.zip'
Content type 'application/zip' length 751310 bytes (733 KB)
downloaded 733 KB

trying URL 'https://bioconductor.org/packages/3.6/bioc/bin/windows/contrib/3.4/rhdf5_2.22.0.zip'
Content type 'application/zip' length 5819220 bytes (5.5 MB)
downloaded 5.5 MB

package ‘zlibbioc’ successfully unpacked and MD5 sums checked
package ‘rhdf5’ successfully unpacked and MD5 sums checked

The downloaded binary packages are in
	C:\Users\Andriy\AppData\Local\Temp\Rtmp8iXzWT\downloaded_packages
> library(rhdf5)
```
3.	Виведіть зміст файлу командою h5ls().
```r
> h5ls("data.hdf5")
                 group            name
0                    /            meta
1                /meta     Description
2                /meta  DescriptionURL
3                /meta        Detector
4                /meta        Duration
5                /meta        GPSstart
6                /meta     Observatory
7                /meta            Type
8                /meta        UTCstart
9                    /         quality
10            /quality          detail
11            /quality      injections
12 /quality/injections InjDescriptions
13 /quality/injections   InjShortnames
14 /quality/injections         Injmask
15            /quality          simple
16     /quality/simple  DQDescriptions
17     /quality/simple    DQShortnames
18     /quality/simple          DQmask
19                   /          strain
20             /strain          Strain
         otype  dclass      dim
0    H5I_GROUP                 
1  H5I_DATASET  STRING    ( 0 )
2  H5I_DATASET  STRING    ( 0 )
3  H5I_DATASET  STRING    ( 0 )
4  H5I_DATASET INTEGER    ( 0 )
5  H5I_DATASET INTEGER    ( 0 )
6  H5I_DATASET  STRING    ( 0 )
7  H5I_DATASET  STRING    ( 0 )
8  H5I_DATASET  STRING    ( 0 )
9    H5I_GROUP                 
10   H5I_GROUP                 
11   H5I_GROUP                 
12 H5I_DATASET  STRING        5
13 H5I_DATASET  STRING        5
14 H5I_DATASET INTEGER     4096
15   H5I_GROUP                 
16 H5I_DATASET  STRING        7
17 H5I_DATASET  STRING        7
18 H5I_DATASET INTEGER     4096
19   H5I_GROUP                 
20 H5I_DATASET   FLOAT 16777216
```

4.	Зчитайте результати вимірів. Для цього зчитайте name Strain з групи strain в змінну strain. Після зчитування не забувайте закривати файл командою H5Close().
```r
> strain <- h5read("data.hdf5", "strain/Strain")
> H5close()
```

5.	Також з «strain/Strain» зчитайте атрибут (функція h5readAttributes) Xspacing в змінну st та виведіть її. Це інтервал часу між вимірами.
```r
> st <- h5readAttributes("data.hdf5", "/strain/Strain")$Xspacing
> st
[1] 0.0002441406
```

6.	Знайдіть час початку події та її тривалість. Для цього з групи meta зчитайте в змінну gpsStart  name GPSstart та в змінну duration name Duration.
```r
> gpsStart <- h5read("data.hdf5", "meta/GPSstart")
> duration <- h5read("data.hdf5", "meta/Duration")
```

7.	Знайдіть час закінчення події та збережіть його в змінну gpsEnd.
```r
> gpsEnd <- gpsStart + duration
```
8.	Створіть вектор з часу вимірів і збережіть у змінну myTime. Початок послідовності – gpsStart, кінець – gpsEnd, крок – st.
```r
> myTime <- seq(gpsStart, gpsEnd, st)
```

9.	Побудуємо графік тільки для першого мільйону вимірів. Для цього створіть змінну numSamples, яка дорівнює 1000000.
```r
> numSamples <- 1000000
```

10.	Побудуйте графік за допомогою функції plot(myTime[0:numSamples], strain[0:numSamples], type = "l", xlab = "GPS Time (s)", ylab = "H1 Strain")
```r
> plot(myTime[0:numSamples], strain[0:numSamples], type = "l", xlab = "GPS Time (s)", ylab = "H1 Strain")
```
![alt text](https://github.com/savandriy/mpoi/raw/master/Rplot.png)
