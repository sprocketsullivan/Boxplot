---
title       : Boxplot
description : 
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
100 men and 100 women agreed to have their brain volume as well as their body weight measured. We put the resulting data into variable `my.data` in your R workspace. `my.data` is of type `data frame` (see Chapter 5 of course "Introduction to R")

`@instructions`
- Use [`summary()`](https://www.rdocumentation.org/packages/base/versions/3.4.3/topics/summary) on `my.data` to have a look at its structure.
- Calculate the mean ($\mu$) of brain volume in the my.data dataframe.
- Use [`aggregate()`](https://www.rdocumentation.org/packages/stats/versions/3.4.3/topics/aggregate) to calculate the mean for each gender.
- Do the same for the standard deviation ($\sigma$).

`@hint`
Have a look at the plot. Which color does the point with the lowest rating have?

`@pre_exercise_code`
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer
library(tidyr)
library(dplyr)
n<-100
set.seed(123)
my.data<-data.frame(gender=c(rep("male",n),rep("female",n)), brain=c(rnorm(n,1273,100),rnorm(n,1131,100)))
my.data <-
  my.data %>% 
  mutate(body=brain+25+rnorm(n*2,0,100))
```
`@sample_code`
```{r}
# summary(my.data)


#average brain


#aggregate over gender and calculate the brain volume


#aggregate over gender and calculate the standard deviation

```

`@solution`
```{r}
# summary(my.data)
summary(my.data)

#average brain
mean(my.data$brain)

#aggregate over gender and calculate the brain volume
aggregate(my.data$brain,list(my.data$gender),mean)

#aggregate over gender and calculate the standard deviation
aggregate(my.data$brain,list(my.data$gender),sd)
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
xp: 100
skills: 1
key: 470e26deb0
```

We created a new dataframe (`p.bar.data`) that contains the mean and its standard error ($\frac{\sigma}{\sqrt{N}}$) for the brain volume of each gender.

`@instructions`
- Make a simple bar plot and assign it to variable `p.bar`
- Add error bars to the bar plot.

`@hint`
Have a look at the plot. Which color does the point with the lowest rating have?

`@pre_exercise_code`
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(gender=c(rep("male",n),rep("female",n)), brain=c(rnorm(n,1273,100),rnorm(n,1131,100)))
my.data <-
  my.data %>% 
  mutate(body=brain+25+rnorm(n*2,0,100))
p.bar.data<-
  my.data %>% 
  group_by(gender) %>% 
  summarise(N=n(),
  		 mean_brain=mean(brain),         
         sem_brain=sd(brain)/sqrt(N)) %>% 
  mutate(ymin=mean_brain-sem_brain,ymax=mean_brain+sem_brain)
```

`@sample_code`
```{r}
# simple barplot assign this to variable p.bar
p.bar <- ggplot(aes(y=______,x=_____),data=p.bar.data)+
  geom_bar(stat="______")

# add error bars to p.bar
p.bar <- ggplot(aes(y=_____,x=_____),data=p.bar.data)+
  geom_bar(stat="_____")+
  geom_errorbar(aes(ymin=______,ymax=_____),width=0.3)

# plot p.bar
plot(p.bar)
```

`@solution`
```{r}
# simple barplot assign this to variable p.bar
p.bar <- ggplot(aes(y=mean_brain,x=gender),data=p.bar.data)+
  geom_bar(stat="identity")

#add error bars to p.bar
p.bar <- ggplot(aes(y=mean_brain,x=gender),data=p.bar.data)+
  geom_bar(stat="identity")+
  geom_errorbar(aes(ymin=ymin,ymax=ymax),width=0.3)

# plot p.bar
plot(p.bar)

```

`@sct`
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

success_msg("Good work!")
```

---
## Central tendency, dispersion, and skew

```yaml
type: NormalExercise
key: 2190a55148
lang: r
xp: 100
skills: 1
```

We already introduced you to central tendency measures of a distribution (mean ($\mu$) and median) and to the spread (dispersion) of a distribution (variance ($\sigma^2$) and standard deviation ($\sigma$)).

A normal distribution is symmetrical, i.e. $Mean = Median$. Whenever a distribution is not symmetrical: $Mean\neq Median$.

You can find additional, more in depth material about mean, variance, and skew of a distribution at
[Moments of Distributions](https://www.youtube.com/watch?v=fv5QB3eK7jA) and 
[Normal Distributions, Standard Deviations, Modality, Skewness and Kurtosis: Understanding concepts](https://www.youtube.com/watch?v=HnMGKsupF8Q). 

`@instructions`
The two commands provided in your workspace will each plot a probability density function (PDF). The red line represents $\mu$, the blue dashed line represents the median, repectively.

- `p.norm` plots a standard normal distribution ($\mu=0$). The dark gray area represents $\mu\pm1\sigma$ and covers ~67% of the area under the curve, the light gray area represents $\mu\pm2\sigma$ and covers ~95% of the area under the curve.

- `p.skew` plots a non-symmetrical distribution.

`@hint`
Have a look at the plot. Which color does the point with the lowest rating have?

`@pre_exercise_code`
```{r}
library(ggplot2)
library(sn)
x <- seq(-4, 4,by=0.01 )
x2<-seq(-1, 1,by=0.01 )
x3<-seq(-2, 2,by=0.01 )
x4 <- seq(-2, 6,by=0.01 )
n.dist<-data.frame(x=x,y=dnorm(x=x, mean=0, sd=1))
n.dist.sd<-data.frame(x=x2,y=dnorm(x=x2, mean=0, sd=1))
n.dist.sd2<-data.frame(x=x3,y=dnorm(x=x3, mean=0, sd=1))
p.norm<-ggplot(aes(y=y,x=x),data=n.dist)+geom_line()+geom_area(aes(x=x,y=y),data=n.dist.sd2,alpha=0.3)+geom_area(aes(x=x,y=y),data=n.dist.sd,alpha=0.5)+theme_classic()+geom_vline(xintercept=0,col="red")+geom_vline(xintercept=0,col="blue",linetype=2)
n.dist.skew<-data.frame(x=x4,y=dsn(x=x4,alpha=5,omega=2))
mean.skew<-mean(rsn(10000,alpha=5,omega=2))
median.skew<- median(rsn(10000,alpha=5,omega=2))
p.skew<-ggplot(aes(y=y,x=x4),data=n.dist.skew)+geom_line()+theme_classic()+geom_vline(aes(xintercept=mean.skew),colour="red")+geom_vline(aes(xintercept=median.skew),colour="blue",linetype=2)


```

`@sample_code`
```{r}
#plot density normal distribution
p.norm
#plot skewed distribution
p.skew
```

`@solution`
```{r}
#plot density normal distribution
p.norm
#plot skewed distribution
p.skew

```

`@sct`
```{r}
success_msg("Hope that is clear now!")
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

As seen in the previous exercise, some distributions may be skewed. This may also be true for data you collected. Therefore, it is a good idea to visualize your data by using plots that include spread (dispersion) and skew.
One example in R is the function [`boxplot()`](https://www.rdocumentation.org/packages/graphics/versions/3.4.3/topics/boxplot). 

A boxplot displays more information than a barplot: 
- A box describes the inter quartile range (`IQR`) of a distribution. 50% of the data points lie within the box. The bottom of the box lies at 25% (first quartile), the top at 75% (third quartile) of the data.

- The line in the center of the box represents the median, i.e. 50% of the data points lie above this value and 50% lie below this value. If this line is shifted off center, the data is skewed, i.e $Mean\neq Median$.

- The whiskers (small lines protruding from the box) have an intervall of `1.5*IQR` on each side of the box.

- Data points outside the box are called outliers.

See [Box Plot: Display of Distribution](http://www.physics.csbsju.edu/stats/box2.html) for further details on the layout of boxplots.

`@instructions`
Execute the code on the right to view a boxplot. Outliers are depicted by dots.


`@hint`
Have a look at the plot. Which color does the point with the lowest rating have?

`@pre_exercise_code`
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(gender=c(rep("male",n),rep("female",n)), brain=c(rnorm(n,1273,100),rnorm(n,1131,100)))
my.data <-
  my.data %>% 
  mutate(body=brain+25+rnorm(n*2,0,100))
```

`@sample_code`
```{r}
p.box<-ggplot(aes(y=brain,x=gender),data=my.data)+
    geom_boxplot()
plot(p.box)
```

`@solution`
```{r}
p.box<-ggplot(aes(y=brain,x=gender),data=my.data)+
    geom_boxplot()
plot(p.box)
```

`@sct`
```{r}
success_msg("Well done!")
```
---
## Boxplot and individual data points

```yaml
type: NormalExercise
key: ce9859ec20
lang: r
xp: 100
skills: 1
```
Overlaying a boxplot with the individual data points leads to a better understanding of the boxplot.

Here, we will add a [`geom_jitter()`](https://www.rdocumentation.org/packages/ggplot2/versions/2.2.1/topics/geom_jitter), which creates a scatterplot of your data.

`@instructions`
Execute the code on the right to view a boxplot. Outliers are depicted by dots.


`@hint`
Have a look at the plot. Which color does the point with the lowest rating have?

`@pre_exercise_code`
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(gender=c(rep("male",n),rep("female",n)), brain=c(rnorm(n,1273,100),rnorm(n,1131,100)))
my.data <-
  my.data %>% 
  mutate(body=brain+25+rnorm(n*2,0,100))
```

`@sample_code`
```{r}
p.box<-ggplot(aes(y=brain,x=gender),data=my.data)+
    geom_boxplot()+
    geom_jitter(width=.1,col="red")
    
plot(p.box)
```

`@solution`
```{r}
p.box<-ggplot(aes(y=brain,x=gender),data=my.data)+
    geom_boxplot()+
    geom_jitter(width=.1,col="red")
    
plot(p.box)
```

`@sct`
```{r}
success_msg("HoHo!")
```
---
## Plot aesthetics

```yaml
type: NormalExercise
key: '4331132383'
lang: r
xp: 100
skills: 1
```
library `ggplot2` offers excellent plotting capabilities that allow for a wide range of customisation. Function [`aes()`](https://www.rdocumentation.org/packages/ggplot2/versions/2.2.1/topics/aes), e.g, describes how  data are mapped to visual properties (aesthetics) of geometric objects (geoms).

Let's add some colour to the plot. For convenience, the plot commands can be assigned to a variable. We will use `p.box`. Overlays can be generated by adding several plot commands using `+`. To actually see the plot use  [`plot()`](https://www.rdocumentation.org/packages/graphics/versions/3.4.3/topics/plot) or just type in the name or the variable an execute.

`@instructions`
- The first line of code will create a  boxplot with fill colours defined by gender
- Change the two colours look out for obvious stereotypes and do not use them! Why not use "pink" and "light blue"?
- Add a theme to remove a lot of clutter. One theme provided by R is `theme_classic()`. See [`ggtheme`] (https://www.rdocumentation.org/packages/ggplot2/versions/2.2.1/topics/ggtheme) for more information.
- Remove the legend by setting the legend.position to "none"

`@hint`
Have a look at the plot. Which color does the point with the lowest rating have?

`@pre_exercise_code`
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)
n<-100
set.seed(123)
my.data<-data.frame(gender=c(rep("male",n),rep("female",n)), brain=c(rnorm(n,1273,100),rnorm(n,1131,100)))
my.data <-
  my.data %>% 
  mutate(body=brain+25+rnorm(n*2,0,100))
```

`@sample_code`
```{r}
#boxplot
p.box<-
  ggplot(aes(y=brain,x=gender,fill=gender),data=my.data) +
  geom_boxplot()
plot(p.box)
# change the two colours 
p.box<- p.box+
  scale_fill_manual(values=c("_____","_____"))
plot(p.box)
# add a theme 
p.box<- p.box+  
  theme_c_____()
plot(p.box)
# remove the legend
p.box<- p.box+  
  theme(legend._____ = '_____')
plot(p.box)
```

`@solution`
```{r}
#boxplot
p.box<-
  ggplot(aes(y=brain,x=gender,fill=gender),data=my.data) +
  geom_boxplot()
plot(p.box)
# change the two colours 
p.box<- p.box+
  scale_fill_manual(values=c("pink","light blue"))
plot(p.box)
# add a theme 
p.box<- p.box+  
  theme_classic()
plot(p.box)
# remove the legend
p.box<- p.box+  
  theme(legend.position = 'none')
plot(p.box)
```

`@sct`
```{r}
success_msg("Well done!")
```
---
## Relationship between variables

```yaml
type: NormalExercise
key: f621387b92
lang: r
xp: 100
skills: 1
```
Exercise Text

`@instructions`
Exercise Instructions 

`@hint`
Exercise Hint

`@pre_exercise_code`
```{r}
```

`@sample_code`
```{r}
```

`@solution`
```{r}
```

`@sct`
```{r}

```