---
title       : Recap of session
description : A short recap of the session
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

---
## Recap of last session

```yaml
type: NormalExercise
lang: r
xp: 50
skills: 1
key: 304c453ac8
```


`@pre_exercise_code`
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer
library(tidyr)
library(dplyr)
n<-100
set.seed(123)
my.data<-data.frame(volume=c(rnorm(n,50,10),rnorm(n,40,10)),gender=c(rep("male",n),rep("female",n)))
my.data <-
  my.data %>% 
  mutate(weight=volume+25+rnorm(n*2,0,10))

```
Explore a new data set. This data set shows brain?? volumes of 100 male and 100 female participants as well as their body weight.


`@instructions`
We have created the variable my.data in the workspace. 
1. Use `summary()` to look at its structure.
2. Calculate the mean of volume in the my.data dataframe. 
3. Use `aggregate()` to calculate the mean for each give gender
4. Do the same for the standard deviation

`@sample_code`
```{r}
# summary(my.data)


#average volume


#aggregate over gender and calculate the volume


#calculate the standard deviation
```

`@solution`
```{r}
# summary(my.data)
summary(my.data)
#average volume
mean(my.data$volume)
#aggregate over gender and calculate the volume
aggregate(my.data$volume,list(my.data$gender),mean)
#calculate also the standard deviation
aggregate(my.data$volume,list(my.data$gender),sd)
```

`@sct`
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

success_msg("Good work!")
```


---
## Plotting

```yaml
type: NormalExercise
lang: r
xp: 50
skills: 1
key: '80360380e6'
```
`@pre_exercise_code`
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(volume=c(rnorm(n,50,10),rnorm(n,40,10)),gender=c(rep("male",n),rep("female",n)))
my.data <-
  my.data %>% 
  mutate(weight=volume+25+rnorm(n*2,0,10))
p.bar.data<-
  my.data %>% 
  group_by(gender) %>% 
  summarise(mean_vol=mean(volume),
         N=n(),
         sem_vol=sd(volume)/sqrt(N)) %>% 
  mutate(ymin=mean_vol-sem_vol,ymax=mean_vol+sem_vol)
```
We created a new dataframe that contains the mean and standard error of the mean (`SD/sqrt(N)`) for the volume for each gender.

`@instructions`

1. Make a simple barplot and assign it to the variable p.bar
2. Add error bars to the bar plot.

`@sample_code`
```{r}
# simple barplot assign this to variable p.bar
p.bar <- ggplot(aes(y=______,x=_____),data=p.bar.data)+
  geom_bar(stat="______")

#add error bars to p.bar
p.bar <- ggplot(aes(y=_____,x=_____),data=p.bar.data)+
  geom_bar(stat="_____")+
  geom_errorbar(aes(ymin=______,ymax=_____),width=0.3)

```

`@solution`
```{r}
# simple barplot assign this to variable p.bar
p.bar <- ggplot(aes(y=mean_vol,x=gender),data=p.bar.data)+
  geom_bar(stat="identity")

#add error bars to p.bar
p.bar <- ggplot(aes(y=mean_vol,x=gender),data=p.bar.data)+
  geom_bar(stat="identity")+
  geom_errorbar(aes(ymin=ymin,ymax=ymax),width=0.3)

```

`@sct`
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

success_msg("Good work!")
```




