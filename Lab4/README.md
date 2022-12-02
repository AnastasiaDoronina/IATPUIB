### Основы обработки данных с помощью R

### Цель работы
1. Зекрепить практические навыки использования языка программирования R для обработки данных
2. Закрепить знания основных функций обработки данных экосистемы tidyverse языка R
3. Развить пркатические навыки использования функций обработки данных пакета dplyr – функции
select(), filter(), mutate(), arrange(), group_by()

### Подготовка
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
install.packages('dplyr')
install.packages('nycflights13')
library(dplyr)
library(nycflights13)
```

### Задание1 Сколько встроенных в пакет nycflights13 датафреймов?
```{r}
df_package<-ls(package:nycflights13)
df_package %>%
  length
```
### Задание2 Сколько строк в каждом датафрейме?
```{r}
airlines <- nycflights13::airlines
airports<- nycflights13::airports
flights<- nycflights13::flights
planes<- nycflights13::planes
weather<- nycflights13::weather
airlines %>% nrow()
airports %>% nrow()
flights %>% nrow()  
planes %>% nrow()
weather %>% nrow()
```
### Задание3 Сколько столбцов в каждом датафрейме?
```{r}
airlines %>% ncol()
airports %>% ncol()
flights %>% ncol()
planes %>% ncol()
weather %>% ncol()
```
### Задание4 Как просмотреть примерный вид датафрейма?
```{r}
airlines %>% glimpse()
airports %>% glimpse()
flights %>% glimpse ()
planes %>% glimpse()
weather %>% glimpse()
```
### Задание5 Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?
```{r}
airlines%>%
  select(carrier)%>%
  distinct() %>%
  count()
```
### Задание6 Сколько рейсов принял аэропорт John F Kennedy Intl в мае?
```{r}
a<- airports %>%
  filter(name =='John F Kennedy Intl') %>%
  select(faa)
flights%>%
  filter(!is.na(time_hour))%>%
  filter(dest==a,time_hour >= as.POSIXct("2013-05-01 00:00:00", tz="UTC"), time_hour<= as.POSIXct("2013-05-31 24:59:59", tz="UTC"))%>%
  count()
```
### Задание7 Какой самый северный аэропорт?
```{r}
airports %>%
  filter(lat==max(lat))
```
### Задание8 Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?
```{r}
airports %>%
  filter(lon==max(lon))
```
### Задание9 Какие бортовые номера у самых старых самолетов?
```{r}
planes %>%
  filter(!is.na(year))%>%
  filter(year==min(year)) %>%
  select(tailnum)
```
### Задание10 Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия).
```{r}
a <- airports %>%
  filter(name =='John F Kennedy Intl') %>%
  select(faa)
b <- weather%>%
  filter(origin==a$faa, month==9)%>%
  select(temp)
5/9*(mean(b$temp)-32)
```
### Задание11 Самолеты какой авиакомпании совершили больше всего вылетов в июне?
```{r}
flights %>%
  filter(month==6)%>%
  group_by(carrier)%>%
  summarise(count_flights=length(flight), .groups = "drop")%>%
  arrange(desc(count_flights)) %>%
  slice(1:1)
```
### Задание12 Самолеты какой авиакомпании задерживались чаще других в 2013 году?
```{r}

flights%>%
  filter(year==2013)%>%
  filter(dep_delay>0)%>%
  group_by(carrier)%>%
  summarise(delay_times=length(flight), .groups="drop")%>%
  arrange(desc(delay_times))%>%
  slice(1:1)
```
