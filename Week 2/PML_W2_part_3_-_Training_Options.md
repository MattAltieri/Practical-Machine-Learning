# Training Options



## SPAM Example


```r
library(caret)
library(kernlab)
data(spam)
inTrain <- createDataPartition(y=spam$type, p=0.75, list=F)
training <- spam[inTrain,]
testing <- spam[-inTrain,]
modelFit <- train(type ~ ., training, method="glm")
```

---

## Train Options


```r
args(train.default)
```

```
function (x, y, method = "rf", preProcess = NULL, ..., weights = NULL, 
    metric = ifelse(is.factor(y), "Accuracy", "RMSE"), maximize = ifelse(metric == 
        "RMSE", FALSE, TRUE), trControl = trainControl(), tuneGrid = NULL, 
    tuneLength = 3) 
NULL
```

---

## Metric Options

**Continuous outcomes:**

- _RMSE_ -- Root mean squared error
- _RSquared_ -- $R^2$ from regression models

**Categorical outcomes:**

- _Accuracy_ -- Fraction correct
- _Kappa_ -- A measure of [concordance](https://en.wikipedia.org/wiki/Cohen%27s_kappa)

---

## `trainControl`


```r
args(trainControl)
```

```
function (method = "boot", number = ifelse(grepl("cv", method), 
    10, 25), repeats = ifelse(grepl("cv", method), 1, number), 
    p = 0.75, initialWindow = NULL, horizon = 1, fixedWindow = TRUE, 
    verboseIter = FALSE, returnData = TRUE, returnResamp = "final", 
    savePredictions = FALSE, classProbs = FALSE, summaryFunction = defaultSummary, 
    selectionFunction = "best", preProcOptions = list(thresh = 0.95, 
        ICAcomp = 3, k = 5), sampling = NULL, index = NULL, indexOut = NULL, 
    timingSamps = 0, predictionBounds = rep(FALSE, 2), seeds = NA, 
    adaptive = list(min = 5, alpha = 0.05, method = "gls", complete = TRUE), 
    trim = FALSE, allowParallel = TRUE) 
NULL
```

---

## `trainControl` resampling

- _method_
    - _boot_ -- bootstrapping
    - _boot632_ -- bootstrapping with adjustment
    - _cv_ -- cross-validation
    - _repeatedcv_ -- repeated cross-validation
    - _LOOCV_ -- leave-one-out cross-validation
- _number_
    - For boot/cross-validation
    - Number of subsamples to take
- _repeats_
    - Number of times to repeat subsampling
    - If big this can _slow things down_

---

## Setting the Seed

- It is often usedful to set an overall seed
- You can also set a seed for each resample
- Seeding each resample is useful for parallel fits

---

## Seed Example


```r
set.seed(1235)
modelFit2 <- train(type ~ ., training, method="glm")
modelFit2
```

```
Generalized Linear Model 

3451 samples
  57 predictor
   2 classes: 'nonspam', 'spam' 

No pre-processing
Resampling: Bootstrapped (25 reps) 
Summary of sample sizes: 3451, 3451, 3451, 3451, 3451, 3451, ... 
Resampling results

  Accuracy   Kappa      Accuracy SD  Kappa SD  
  0.9213849  0.8349134  0.007067118  0.01525826

 
```

---

## Seed Example


```r
set.seed(1235)
modelFit3 <- train(type ~ ., training, method="glm")
modelFit3
```

```
Generalized Linear Model 

3451 samples
  57 predictor
   2 classes: 'nonspam', 'spam' 

No pre-processing
Resampling: Bootstrapped (25 reps) 
Summary of sample sizes: 3451, 3451, 3451, 3451, 3451, 3451, ... 
Resampling results

  Accuracy   Kappa      Accuracy SD  Kappa SD  
  0.9213849  0.8349134  0.007067118  0.01525826

 
```

---

## Further Resources

- [Caret tutorial](http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf)
- [Model training and tuning](https://topepo.github.io/caret/training.html)
