# Combining Predictors



## Key Ideas

- You can combine classifiers by averaging/voting
- Combining classifiers improves accuracy
- Combining classifiers reduces interpretability
- Boosting, bagging, and random forests are variants on this theme

---

## Netflix Prize

BellKor = Combination of 107 predictors

![](netflix1.JPG)

[http://www.netflixprize.com//leaderboard](http://www.netflixprize.com//leaderboard)

---

## Heritage Health Prize - Progress Prize 1

![](progress1.JPG)

[Market Makers](https://kaggle2.blob.core.windows.net/wiki-files/327/e4cd1d25-eca9-49ca-9593-b254a773fe03/Market%20Makers%20-%20Milestone%201%20Description%20V2%201.pdf)

![](progress2.JPG)

[Mestrom](https://kaggle2.blob.core.windows.net/wiki-files/327/09ccf652-8c1c-4a3d-b979-ce2369c985e4/Willem%20Mestrom%20-%20Milestone%201%20Description%20V2%202.pdf)

---

## Basic Intuition -- Majority Vote

Suppose we have 5 completely independent classifiers

If accuracy is 70% for each:

- $10 \times (0.7)^3(0.3)^2 + 5 \times (0.7)^4(0.3)^2 + (0.7)^5$
- 83.7% majority vote accuracy

With 101 independent classifiers

- 99.9% majority vote accuracy

---

## Approaches for Combining Classifiers

1. Bagging, boosting, random forests
    - Usually combine similar classifiers
2. Combining different classifiers
    - Model stacking
    - Model ensembling

--

## Example with `Wage` data

**Create training, test, and validation sets**


```r
library(ISLR)
data(Wage)
library(ggplot2)
library(caret)
Wage <- subset(Wage, select=-c(logwage))

# Create a building dataset and validation set
inBuild <- createDataPartition(y=Wage$wage,
                               p=0.7, list=F)
validation <- Wage[-inBuild,]
buildData <- Wage[inBuild,]

inTrain <- createDataPartition(y=buildData$wage,
                               p=0.7, list=F)
training <- buildData[inTrain,]
testing <- buildData[-inTrain,]
```

---

## `Wage` datasets

**Create training, test, and validation sets**


```r
dim(training)
```

```
[1] 1474   11
```

```r
dim(testing)
```

```
[1] 628  11
```

```r
dim(validation)
```

```
[1] 898  11
```

---

## Build Two Different Models


```r
mod1 <- train(wage ~ ., training, method="glm")
mod2 <- train(wage ~ ., training, method="rf",
              trControl=trainControl(method="cv"), number=3)
```

---

## Predict on the Testing Set


```r
pred1 <- predict(mod1, testing)
pred2 <- predict(mod2, testing)
qplot(pred1, pred2, color=wage, data=testing)
```

<div class="rimage center"><img src="fig/predict1-1.png" title="" alt="" class="plot" /></div>

---

## Fit a Model that Combines Predictors


```r
predDF <- data.frame(pred1, pred2, wage=testing$wage)
combModFit <- train(wage ~ ., predDF, method="gam")
combPred <- predict(combModFit, predDF)
```

---

## Testing Errors


```r
sqrt(sum((pred1 - testing$wage)^2))
```

```
[1] 1143.419
```

```r
sqrt(sum((pred2 - testing$wage)^2))
```

```
[1] 1173.376
```

```r
sqrt(sum((combPred - testing$wage)^2))
```

```
[1] 991.8219
```

---

## Predict on Validation Dataset


```r
pred1V <- predict(mod1, validation)
pred2V <- predict(mod2, validation)
predVDF <- data.frame(pred1=pred1V, pred2=pred2V)
combPredV <- predict(combModFit, predVDF)
```

---

## Evaluate on Validation


```r
sqrt(sum((pred1V - validation$wage)^2))
```

```
[1] 1553.432
```

```r
sqrt(sum((pred2V - validation$wage)^2))
```

```
[1] 1573.247
```

```r
sqrt(sum((combPredV - validation$wage)^2))
```

```
[1] 1315.753
```

---

## Notes and Further Resources

- Even simple blending can be useful
- Typical model for binary/multiclass data
    - Build an odd number of models
    - Predict with each model
    - Predict the class by majority vote
- This can get dramatically more complicated
    - Simple blending in `caret`: [caretEnsemble](https://github.com/zachmayer/caretEnsemble) (use at your own risk!)
    - Wikipedia [ensemble learning](https://en.wikipedia.org/wiki/Ensemble_learning)
    
---

## Recall - Scalability Matters

![](netflix2.JPG)

[http://www.techdirt.com/blog/innovation/articles/20120409/03412518422/](http://www.techdirt.com/blog/innovation/articles/20120409/03412518422/)

[http://techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html](http://techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html)
