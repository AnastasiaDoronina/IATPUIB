Основы обработки данных с помощью R
AnastasiaDoronina
2022-10-19
Цель работы
Развить практические навыки использования языка программирования R для обработки данных
Закрепить знания базовых типов данных языка R
Развить пркатические навыки использования функций обработки данных пакета dplyr – функции select(), filter(), mutate(), arrange(), group_by()
Подготовка
library (dplyr)
## 
## Attaching package: 'dplyr'
## The following objects are masked from 'package:stats':
## 
##     filter, lag
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
starwars
## # A tibble: 87 × 14
##    name        height  mass hair_…¹ skin_…² eye_c…³ birth…⁴ sex   gender homew…⁵
##    <chr>        <int> <dbl> <chr>   <chr>   <chr>     <dbl> <chr> <chr>  <chr>  
##  1 Luke Skywa…    172    77 blond   fair    blue       19   male  mascu… Tatooi…
##  2 C-3PO          167    75 <NA>    gold    yellow    112   none  mascu… Tatooi…
##  3 R2-D2           96    32 <NA>    white,… red        33   none  mascu… Naboo  
##  4 Darth Vader    202   136 none    white   yellow     41.9 male  mascu… Tatooi…
##  5 Leia Organa    150    49 brown   light   brown      19   fema… femin… Aldera…
##  6 Owen Lars      178   120 brown,… light   blue       52   male  mascu… Tatooi…
##  7 Beru White…    165    75 brown   light   blue       47   fema… femin… Tatooi…
##  8 R5-D4           97    32 <NA>    white,… red        NA   none  mascu… Tatooi…
##  9 Biggs Dark…    183    84 black   light   brown      24   male  mascu… Tatooi…
## 10 Obi-Wan Ke…    182    77 auburn… fair    blue-g…    57   male  mascu… Stewjon
## # … with 77 more rows, 4 more variables: species <chr>, films <list>,
## #   vehicles <list>, starships <list>, and abbreviated variable names
## #   ¹​hair_color, ²​skin_color, ³​eye_color, ⁴​birth_year, ⁵​homeworld
starwars <- starwars
Задание 1
Сколько строк в датафрейме

starwars %>% nrow()
## [1] 87
Задание 2
Сколько столбцов в датафрейме

starwars %>% ncol()
## [1] 14
Задание 3
Как посмотреть примерный вид датафрейма?

starwars %>% glimpse()
## Rows: 87
## Columns: 14
## $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Or…
## $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 2…
## $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.…
## $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", N…
## $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "…
## $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue",…
## $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, …
## $ sex        <chr> "male", "none", "none", "male", "female", "male", "female",…
## $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "femini…
## $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "T…
## $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Huma…
## $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return…
## $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp…
## $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",…
Задание 4
Сколько уникальных рас персонажей (species) представлено в данных?

length(unique(starwars$species))
## [1] 38
Задание 5
Найти самого высокого персонажа.

max(na.omit(starwars$height))
## [1] 264
starwars %>% filter(height==264)
## # A tibble: 1 × 14
##   name        height  mass hair_c…¹ skin_…² eye_c…³ birth…⁴ sex   gender homew…⁵
##   <chr>        <int> <dbl> <chr>    <chr>   <chr>     <dbl> <chr> <chr>  <chr>  
## 1 Yarael Poof    264    NA none     white   yellow       NA male  mascu… Quermia
## # … with 4 more variables: species <chr>, films <list>, vehicles <list>,
## #   starships <list>, and abbreviated variable names ¹​hair_color, ²​skin_color,
## #   ³​eye_color, ⁴​birth_year, ⁵​homeworld
Задание 6
Найти всех персонажей ниже 170

max(na.omit(starwars$height))
## [1] 264
starwars %>% filter(height<170)
## # A tibble: 23 × 14
##    name        height  mass hair_…¹ skin_…² eye_c…³ birth…⁴ sex   gender homew…⁵
##    <chr>        <int> <dbl> <chr>   <chr>   <chr>     <dbl> <chr> <chr>  <chr>  
##  1 C-3PO          167    75 <NA>    gold    yellow      112 none  mascu… Tatooi…
##  2 R2-D2           96    32 <NA>    white,… red          33 none  mascu… Naboo  
##  3 Leia Organa    150    49 brown   light   brown        19 fema… femin… Aldera…
##  4 Beru White…    165    75 brown   light   blue         47 fema… femin… Tatooi…
##  5 R5-D4           97    32 <NA>    white,… red          NA none  mascu… Tatooi…
##  6 Yoda            66    17 white   green   brown       896 male  mascu… <NA>   
##  7 Mon Mothma     150    NA auburn  fair    blue         48 fema… femin… Chandr…
##  8 Wicket Sys…     88    20 brown   brown   brown         8 male  mascu… Endor  
##  9 Nien Nunb      160    68 none    grey    black        NA male  mascu… Sullust
## 10 Watto          137    NA black   blue, … yellow       NA male  mascu… Toydar…
## # … with 13 more rows, 4 more variables: species <chr>, films <list>,
## #   vehicles <list>, starships <list>, and abbreviated variable names
## #   ¹​hair_color, ²​skin_color, ³​eye_color, ⁴​birth_year, ⁵​homeworld
Задание 7
Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по формуле 𝐼 = 𝑚 ℎ2 , где 𝑚 – масса (weight), а ℎ – рост (height).

starwars %>%
 mutate(imt = mass / ((height*0.01) ^ 2)) %>%
 select(name,imt)
## # A tibble: 87 × 2
##    name                 imt
##    <chr>              <dbl>
##  1 Luke Skywalker      26.0
##  2 C-3PO               26.9
##  3 R2-D2               34.7
##  4 Darth Vader         33.3
##  5 Leia Organa         21.8
##  6 Owen Lars           37.9
##  7 Beru Whitesun lars  27.5
##  8 R5-D4               34.0
##  9 Biggs Darklighter   25.1
## 10 Obi-Wan Kenobi      23.2
## # … with 77 more rows
Задание 8
Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению массы (mass) к росту (height) персонажей.

starwars %>%
  mutate(elongation = mass/(height*0.01)) %>%
  arrange(desc(elongation)) %>%
  head(10) %>%
  select(name,elongation)
## # A tibble: 10 × 2
##    name                  elongation
##    <chr>                      <dbl>
##  1 Jabba Desilijic Tiure      776  
##  2 Grievous                    73.6
##  3 IG-88                       70  
##  4 Owen Lars                   67.4
##  5 Darth Vader                 67.3
##  6 Jek Tono Porkins            61.1
##  7 Bossk                       59.5
##  8 Tarfful                     58.1
##  9 Dexter Jettster             51.5
## 10 Chewbacca                   49.1
Задание 9
Найти средний возраст персонажей каждой расы вселенной Звездных войн.

starwars %>%
  filter(!(birth_year %in% NA)) %>% 
  filter(!(species %in% NA)) %>%
  group_by(species) %>%
  summarise(mean_age = mean(birth_year))
## # A tibble: 15 × 2
##    species        mean_age
##    <chr>             <dbl>
##  1 Cerean             92  
##  2 Droid              53.3
##  3 Ewok                8  
##  4 Gungan             52  
##  5 Human              53.4
##  6 Hutt              600  
##  7 Kel Dor            22  
##  8 Mirialan           49  
##  9 Mon Calamari       41  
## 10 Rodian             44  
## 11 Trandoshan         53  
## 12 Twi'lek            48  
## 13 Wookiee           200  
## 14 Yoda's species    896  
## 15 Zabrak             54
Задание 10
Найти самый распространенный цвет глаз персонажей вселенной Звездных войн.

starwars %>%
  filter(!(eye_color %in% NA)) %>%
  count(eye_color, sort = TRUE) %>%
  head(1)
## # A tibble: 1 × 2
##   eye_color     n
##   <chr>     <int>
## 1 brown        21
Задание 11
Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн.

starwars %>%
  filter(!(species %in% NA)) %>%
  group_by(species) %>%
  summarise(mean_len_name = mean(nchar(name)))
## # A tibble: 37 × 2
##    species   mean_len_name
##    <chr>             <dbl>
##  1 Aleena            13   
##  2 Besalisk          15   
##  3 Cerean            12   
##  4 Chagrian          10   
##  5 Clawdite          10   
##  6 Droid              4.83
##  7 Dug                7   
##  8 Ewok              21   
##  9 Geonosian         17   
## 10 Gungan            11.7 
## # … with 27 more rows
Оценка результата
Вывод
