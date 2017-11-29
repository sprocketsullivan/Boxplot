---
title       : Beyond the barplot
description : How to include distributional information in Figures.
attachments :
slides_link : 
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
## Barplot

```yaml
type: NormalExercise
lang: r
xp: 50
skills: 1
key: 470e26deb0
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

`@instructions`

We created a new dataframe (`p.bar.data`) that contains the mean and standard error of the mean ($\frac{SD}{\sqrt(N)}$) for the volume for each gender.

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


---
## Boxplot

```yaml
type: NormalExercise
key: 2a7427429d
lang: r
xp: 100
skills: 1
```


`@instructions`

Here you will find an explanation of central tendency like mean and median 
and the second and third moment of a distribution.
[insert plot here, link to material, see here for explanation]
To get a good feeling for your data it is often beneficial to plot it 
in several ways that also include second and third moments
one example is the so called boxplot. 
Execute the code on the right to view a boxplot.

Compared to a barplot, the boxplot contains more information.
The line in the middle of the box represents the median. If this line is shifted upwards or downwards
of the middle, the data is skewed (not symetrically distributed around the mean).

`@hint`

`@pre_exercise_code`
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(volume=c(rnorm(n,50,10),rnorm(n,40,10)),gender=c(rep("male",n),rep("female",n)))
my.data <-
  my.data %>% 
  mutate(weight=volume+25+rnorm(n*2,0,10))
```

`@sample_code`
```{r}
p.box<-ggplot(aes(y=volume,x=gender),data=my.data)+
    geom_boxplot()
plot(p.box)
```

`@solution`
```{r}
p.box<-ggplot(aes(y=volume,x=gender),data=my.data)+
    geom_boxplot()
plot(p.box)
```

`@sct`
```{r}
success_msg("Well done!")
```
---
## Boxplot and individual data

```yaml
type: NormalExercise
key: 294e800201
lang: r
xp: 100
skills: 1
```


`@instructions`
To understand the rest of the boxplot it is convenient to overlay the 
boxplot with the individual data via adding a jitter geom.

Run the code on the right.

50% of the data lie within the box (interquartile range `IQR`). 
A large box thus signifies data that has a larger spread or variance.
The whiskers (lines above and below box) are `1.5*IQR`. 
The data points outside the whiskers are outliers. 


`@hint`

`@pre_exercise_code`
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(volume=c(rnorm(n,50,10),rnorm(n,40,10)),gender=c(rep("male",n),rep("female",n)))
my.data <-
  my.data %>% 
  mutate(weight=volume+25+rnorm(n*2,0,10))
```

`@sample_code`
```{r}
p.box<-ggplot(aes(y=volume,x=gender),data=my.data)+
    geom_boxplot()+
    geom_jitter(width=.1,col="red")
    
plot(p.box)

```

`@solution`
```{r}
p.box<-ggplot(aes(y=volume,x=gender),data=my.data)+
    geom_boxplot()+
    geom_jitter(width=.1,col="red")
    
plot(p.box)
```

`@sct`
```{r}
success_msg("HoHo!")
```

---
## Beautiful Plots

```yaml
type: NormalExercise
key: 901e2614c4
lang: r
xp: 100
skills: 1
```

`@instructions`
ggplot has excellent plotting capabilities that allow for a wide range of customisation.
Here we first add some colour to the plot.


`@hint`

`@pre_exercise_code`
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(volume=c(rnorm(n,50,10),rnorm(n,40,10)),gender=c(rep("male",n),rep("female",n)))
my.data <-
  my.data %>% 
  mutate(weight=volume+25+rnorm(n*2,0,10))
```

`@sample_code`
```{r}

#first create boxplot with fill colours
#defined by gender
#leave next two lines untouched
p.box<-
  ggplot(aes(y=volume,x=gender,fill=gender),data=my.data) +
  geom_boxplot()
#change the two colours look out for obvious stereotypes and do not use them!
# Why not use "pink" and "light blue"
p.box<- p.box+
  scale_fill_manual(values=c("_____","_____"))
#add a theme to remove a lot of clutter
#one such theme is theme_classic()
p.box<- p.box+  
  theme_c_____()
#remove the legend
#by setting the legend.position to "none"
p.box<- p.box+  
  theme(legend._____ = '_____')
```

`@solution`
```{r}
#first create boxplot with fill colours
#defined by gender
#leave next two lines untouched
p.box<-
  ggplot(aes(y=volume,x=gender,fill=gender),data=my.data) +
  geom_boxplot()
#change the two colours look out for obvious stereotypes and do not use them!
# Why not use "pink" and "light blue"
p.box<- p.box+
  scale_fill_manual(values=c("pink","light blue"))
#add a theme to remove a lot of clutter
#one such theme is theme_classic()
p.box<- p.box+  
  theme_classic()
#remove the legend
#by setting the legend.position to "none"
p.box<- p.box+  
  theme(legend.position = 'none')
```

`@sct`
```{r}
success_msg("Well done!")
```
