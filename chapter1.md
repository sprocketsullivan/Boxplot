---
title       : Recap of session
description : A short recap of the session
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

---
## A really bad movie

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
n<-100
set.seed(123)
my.data<-data.frame(volume=c(rnorm(n,50,10),rnorm(n,40,10)),gender=c(rep("male",n),rep("female",n)))
my.data <-
  my.data %>% 
  mutate(weight=volume+25+rnorm(n*2,0,10))

```
Explore a new data set. This data set shows brain?? volumes of 100 male and 100 female participants as well as their body weight.


`@instructions`
We have created the variable my.data in the workspace. Use `summary()` to look at its structure.
Calculate the mean of 

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
aggregate(my.data$volume,list(my.data$gender),sd)```

`@sct`
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_function("str", args = "object",
              not_called_msg = "You didn't call `str()`!",
              incorrect_msg = "You didn't call `str(object = ...)` with the correct argument, `object`.")

test_object("good_movies")

test_function("plot", args = "x")
test_function("plot", args = "y")
test_function("plot", args = "col")

test_error()

success_msg("Good work!")
```


