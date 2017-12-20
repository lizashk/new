Код:
```r
# Download the data
> a <- read_html("http://www.imdb.com/search/title?count=100&release_date=2017,2017&title_type=feature")

# Parse rank position
> rank_data_html <- html_nodes(a,'.text-primary')
> rank_data <- html_text(rank_data_html)
# Convert ranks to numbers
> rank_data<-as.numeric(rank_data)

# Parse the movie titles
> title_data_html <- html_nodes(a,'.lister-item-header a')
> title_data <- html_text(title_data_html)

# Parse the Duration of movies
> runtime_data_html <- html_nodes(a,'.text-muted .runtime')
> runtime_data <- html_text(runtime_data_html)
# Delete the trailing " min" in durations
> runtime_data<-gsub(" min","",runtime_data)
# Convert durations to numbers
> runtime_data<-as.numeric(runtime_data)

# Create the data frame
> movies <- data.frame(Rank = rank_data, Title = title_data, Runtime = runtime_data, stringsAsFactors = FALSE )
```

1.	Виведіть перші 6 назв фільмів дата фрейму.
```r
> head(movies$Title, 6)
[1] "Зорянi вiйни: Останнi Джедаi"  
[2] "The Disaster Artist"           
[3] "The Shape of Water"            
[4] "Лiга справедливостi"           
[5] "Jumanji: Welcome to the Jungle"
[6] "Тор: Рагнарок"      
```

2.	Виведіть всі назви фільмів с тривалістю більше 120 хв.
```r
> movies[movies$Runtime > 120, ]$Title
 [1] "Зорянi вiйни: Останнi Джедаi"            
 [2] "The Shape of Water"                      
 [3] "Тор: Рагнарок"                           
 [4] "Mother!"                                 
 [5] "Kingsman: Золоте кiльце"                 
 [6] "Вартовi Галактики 2"                     
 [7] "Воно"                                    
 [8] "The Killing of a Sacred Deer"            
 [9] "Darkest Hour"                            
[10] "Call Me by Your Name"                    
[11] "Той, хто бiжить по лезу 2049"            
[12] "Валерiан i мiсто тисячi планет"          
[13] "Логан: Росомаха"                         
[14] "Phantom Thread"                          
[15] "Downsizing"                              
[16] "Диво-Жiнка"                              
[17] "Людина-павук: Повернення додому"         
[18] "Detroit"                                 
[19] "All the Money in the World"              
[20] "Molly's Game"                            
[21] "Сiм сестер"                              
[22] "Красуня i Чудовисько"                    
[23] "Вiйна за планету мавп"                   
[24] "Mudbound"                                
[25] "The Square"                              
[26] "Roman J. Israel, Esq."                   
[27] "Пiрати Карибського моря: Помста Салазара"
[28] "Трансформери: Останнiй лицар"            
[29] "Пострiл в безодню"                       
[30] "Power Rangers"                           
[31] "Hostiles"                                
[32] "Girls Trip"                              
[33] "Джон Уiк 2"   
```

3.	Скільки фільмів мають тривалість менше 100 хв.
```r
> length(movies[movies$Runtime < 100, ]$Title)
[1] 20
```
