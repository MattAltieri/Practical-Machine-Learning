# Caret Package



## The Caret R Package

![](caret.JPG)

[http://caret.r-forge.r-project.org/](http://caret.r-forge.r-project.org/)

---

## Caret Functionality

- Some preprocessing (cleaning)
    - `preProcess`
- Data splitting
    - `createDataPartition`
    - `createResample`
    - `createTimeSeries`
- Training/testing functions
    - `train`
    - `predict`
- Model comparison
    - `confusionMatrix`

---

## Machine Learning Algorithms in R

- Linear discriminant analysis
- Regression
- Naive Bayes
- Support vector machines
- Classification and regression trees
- Random forests
- Boosting
- etc.

---

## Why Caret?

![](whycaret.JPG)

[http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf](http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf)

---

## SPAM Example: Data splitting


```r
library(caret)
library(kernlab)
data(spam)
inTrain <- createDataPartition(y=spam$type, p=0.75, list=F)
training <- spam[inTrain,]
testing <- spam[-inTrain,]
dim(training)
```

```
[1] 3451   58
```

---

## SPAM Example: Fit a Model


```r
set.seed(32343)
modelFit <- train(type ~ ., data=training, method="glm")
modelFit
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
  0.9203165  0.8328361  0.01451473   0.02825155

 
```

---

## SPAM Example: Final Model


```r
modelFit$finalModel
```

```

Call:  NULL

Coefficients:
      (Intercept)               make            address                all              num3d  
       -1.424e+00         -3.742e-01         -1.729e-01          9.880e-02          2.496e+00  
              our               over             remove           internet              order  
        6.498e-01          1.144e+00          2.056e+00          6.012e-01          6.009e-01  
             mail            receive               will             people             report  
        1.452e-01          5.334e-03         -2.370e-01         -3.211e-01          3.277e-01  
        addresses               free           business              email                you  
        2.105e+00          9.435e-01          8.965e-01          5.269e-02          9.927e-02  
           credit               your               font             num000              money  
        7.683e-01          1.984e-01          2.529e-01          2.423e+00          5.323e-01  
               hp                hpl             george             num650                lab  
       -2.285e+00         -1.133e+00         -1.045e+01          8.690e-01         -3.575e+00  
             labs             telnet             num857               data             num415  
       -4.258e-01         -1.707e-01          3.072e+00         -6.635e-01         -3.177e+01  
            num85         technology            num1999              parts                 pm  
       -2.345e+00          7.214e-01         -1.539e-01         -6.142e-01         -1.028e+00  
           direct                 cs            meeting           original            project  
       -4.121e-01         -5.704e+02         -2.769e+00         -2.175e+00         -1.658e+00  
               re                edu              table         conference      charSemicolon  
       -8.577e-01         -1.398e+00         -1.839e+00         -3.571e+00         -1.423e+00  
 charRoundbracket  charSquarebracket    charExclamation         charDollar           charHash  
       -2.020e-01         -7.587e-01          2.512e-01          6.137e+00          2.270e+00  
       capitalAve        capitalLong       capitalTotal  
       -1.100e-02          1.140e-02          8.770e-04  

Degrees of Freedom: 3450 Total (i.e. Null);  3393 Residual
Null Deviance:	    4628 
Residual Deviance: 1337 	AIC: 1453
```

---

## SPAM Example: Prediction


```r
predictions <- predict(modelFit, newdata=testing)
predictions
```

```
   [1] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
  [12] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    spam   
  [23] spam    spam    nonspam nonspam nonspam spam    nonspam nonspam spam    spam    spam   
  [34] spam    spam    spam    spam    spam    nonspam spam    spam    spam    spam    spam   
  [45] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
  [56] spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam    spam   
  [67] spam    nonspam spam    spam    spam    spam    spam    spam    spam    spam    spam   
  [78] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    nonspam
  [89] spam    spam    spam    spam    nonspam nonspam spam    spam    spam    spam    spam   
 [100] spam    nonspam spam    spam    nonspam spam    spam    nonspam spam    spam    spam   
 [111] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [122] spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam    spam   
 [133] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [144] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [155] spam    nonspam spam    spam    spam    spam    spam    spam    spam    nonspam spam   
 [166] spam    spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam   
 [177] spam    nonspam spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [188] spam    spam    spam    spam    nonspam spam    spam    spam    spam    spam    spam   
 [199] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    spam   
 [210] spam    spam    spam    spam    spam    spam    nonspam spam    spam    spam    spam   
 [221] spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam    spam   
 [232] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [243] spam    nonspam spam    spam    spam    spam    spam    nonspam spam    spam    spam   
 [254] spam    spam    spam    spam    spam    spam    spam    spam    spam    nonspam nonspam
 [265] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [276] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    nonspam
 [287] spam    spam    spam    spam    spam    spam    spam    spam    spam    nonspam spam   
 [298] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [309] spam    spam    spam    spam    spam    nonspam spam    spam    spam    spam    spam   
 [320] nonspam spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [331] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [342] spam    spam    spam    spam    nonspam spam    spam    spam    spam    spam    spam   
 [353] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    nonspam
 [364] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [375] spam    spam    spam    spam    nonspam nonspam spam    spam    nonspam spam    spam   
 [386] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [397] spam    spam    nonspam spam    spam    spam    spam    spam    nonspam spam    spam   
 [408] spam    spam    spam    spam    nonspam spam    spam    nonspam spam    nonspam nonspam
 [419] spam    spam    spam    nonspam spam    spam    spam    spam    spam    nonspam spam   
 [430] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [441] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    nonspam
 [452] spam    spam    spam    nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam
 [463] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [474] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [485] nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [496] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [507] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [518] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [529] nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam
 [540] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [551] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [562] nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam spam    spam    nonspam
 [573] nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [584] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [595] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [606] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [617] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [628] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [639] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [650] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [661] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [672] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [683] nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [694] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [705] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [716] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [727] nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [738] nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam
 [749] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [760] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [771] nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam
 [782] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [793] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [804] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam   
 [815] nonspam nonspam nonspam spam    spam    spam    nonspam nonspam nonspam nonspam nonspam
 [826] nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [837] nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [848] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam
 [859] nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [870] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [881] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [892] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [903] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [914] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [925] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam   
 [936] nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam
 [947] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam   
 [958] nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam
 [969] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [980] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [991] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1002] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1013] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1024] spam    nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam
[1035] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1046] nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam
[1057] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1068] nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam
[1079] spam    nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam
[1090] nonspam nonspam nonspam nonspam spam    nonspam spam    nonspam spam    spam    nonspam
[1101] nonspam spam    nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam
[1112] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1123] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1134] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1145] nonspam nonspam nonspam nonspam nonspam nonspam
Levels: nonspam spam
```

---

## SPAM Example: Confusion Matrix


```r
confusionMatrix(predictions, testing$type)
```

```
Confusion Matrix and Statistics

          Reference
Prediction nonspam spam
   nonspam     655   48
   spam         42  405
                                          
               Accuracy : 0.9217          
                 95% CI : (0.9047, 0.9366)
    No Information Rate : 0.6061          
    P-Value [Acc > NIR] : <2e-16          
                                          
                  Kappa : 0.8357          
 Mcnemar's Test P-Value : 0.5982          
                                          
            Sensitivity : 0.9397          
            Specificity : 0.8940          
         Pos Pred Value : 0.9317          
         Neg Pred Value : 0.9060          
             Prevalence : 0.6061          
         Detection Rate : 0.5696          
   Detection Prevalence : 0.6113          
      Balanced Accuracy : 0.9169          
                                          
       'Positive' Class : nonspam         
                                          
```

---

## Further Information

- Caret tutorials
    - [http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf](http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf)
    - [http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf](http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf)
- A paper introducting the caret package
    - [http://www.jstatsoft.org/v28/i05/paper](http://www.jstatsoft.org/v28/i05/paper)
