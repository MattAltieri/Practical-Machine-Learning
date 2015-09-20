# Predicting with Trees



## Key Ideas

- Iteratively split variables into groups
- Evaluate "homogeneity" within each group
- Split again if necessary

**Pros:**

- Easy to interpret
- Better performance in nonlinear settings

**Cons:**

- Without pruning/cross-validation can lead to overfitting
- Harder to estimate uncertainty
- Results may be variable depending on the exact values of the variables being used

---

## Example Tree

![](extree.JPG)

[http://graphics8.nytimes.com/images/2008/04/16/us/0416-nat-subOBAMA.jpg](http://graphics8.nytimes.com/images/2008/04/16/us/0416-nat-subOBAMA.jpg)

---

## Basic Algorithm

1. Start with all variables in one group
2. Find the variable/split that best separates the outcomes
3. Divide the data into two groups ("leaves") on that split ("node")
4. Within each split, find the best variable/split that separates the outcomes
5. Continue until the groups are too small or sufficiently "pure"

---

## Measures of Impurity

$$
\hat p_{mk} = \frac{1}{N_m} \sum_{x_i\ in\ Leaf\ m} 1(y_i = k)
$$

**Misclassification Error:**

$$
1 - \hat p_{mk(m)};k(m) = most;common;k
$$

- 0 = perfect purity
- 0.5 = no purity

**Gini Index:**

$$
\sum_{k \ne k'} \hat p_{mk} \times \hat p_{mk'} = \sum_{k=1}^K \hat p_{mk}(1 - \hat p_{mk}) = 1 - \sum_{k=1}^K p_{mk}^2
$$

- 0 = perfect purity
- 0.5 = no purity

[http://en.wikipedia.org/wiki/Decision_tree_learning](http://en.wikipedia.org/wiki/Decision_tree_learning)

---

## Measures of Impurity

**Deviation/information gain:**

$$
-\sum_{k=1}^K \hat p_{mk}log_2\hat p_{mk}
$$

- 0 = perfect purity
- 1 = no purity

[http://en.wikipedia.org/wiki/Decision_tree_learning](http://en.wikipedia.org/wiki/Decision_tree_learning)

## Measures of Impurity

<div class="rimage center"><img src="fig/leftplot-1.png" title="" alt="" class="plot" /></div>

- **Misclassification:** $1/16 = 0.06$
- **Gini:** $1 - [(1/16)^2 + (15/16)^2] = 0.12$
- **Information:** $-[1/16 \times \log_2(1/16) + 15/16 \times \log_2(15/16)] = 0.34$

<div class="rimage center"><img src="fig/rightplot-1.png" title="" alt="" class="plot" /></div>

- **Misclassification:** $8/16 = 0.5$
- **Gini:** $1 - [(8/16)^2 + (8/16)^2] = 0.5$
- **Information:** $-[8/16 \times \log_2(8/16) + 8/16 \times \log_2(8/16)] = 1$

---

## Example: Iris Data


```r
data(iris)
library(ggplot2)
library(caret)
names(iris)
```

```
[1] "Sepal.Length" "Sepal.Width"  "Petal.Length" "Petal.Width"  "Species"     
```

```r
table(iris$Species)
```

```

    setosa versicolor  virginica 
        50         50         50 
```

---

## Create training and test sets


```r
in.t <- createDataPartition(y=iris$Species, p=0.7, list=F)
iris.train <- iris[in.t,]
iris.test <- iris[-in.t,]
dim(iris.train)
```

```
[1] 105   5
```

```r
dim(iris.test)
```

```
[1] 45  5
```

---

## Iris Pedal Widths / Sepal Width


```r
qplot(Petal.Width, Sepal.Width, color=Species, data=iris.train)
```

<div class="rimage center"><img src="fig/unnamed-chunk-3-1.png" title="" alt="" class="plot" /></div>

---

## Iris Petal Widths / Sepal Width


```r
fit <- train(Species ~ ., method="rpart", data=iris.train)
print(fit$finalModel)
```

```
n= 105 

node), split, n, loss, yval, (yprob)
      * denotes terminal node

1) root 105 70 setosa (0.33333333 0.33333333 0.33333333)  
  2) Petal.Length< 2.5 35  0 setosa (1.00000000 0.00000000 0.00000000) *
  3) Petal.Length>=2.5 70 35 versicolor (0.00000000 0.50000000 0.50000000)  
    6) Petal.Width< 1.75 36  2 versicolor (0.00000000 0.94444444 0.05555556) *
    7) Petal.Width>=1.75 34  1 virginica (0.00000000 0.02941176 0.97058824) *
```

---

## Plot Tree


```r
plot(fit$finalModel, uniform=T, main="Classification Tree")
text(fit$finalModel, use.n=T, all=T, cex=.8)
```

<div class="rimage center"><img src="fig/unnamed-chunk-5-1.png" title="" alt="" class="plot" /></div>

---

## Prettier Plots


```r
library(rattle)
fancyRpartPlot(fit$finalModel)
```

<div class="rimage center"><img src="fig/unnamed-chunk-6-1.png" title="" alt="" class="plot" /></div>

---

## Predicting New Values


```r
predict(fit, newdata=iris.test)
```

```
 [1] setosa     setosa     setosa     setosa     setosa     setosa     setosa     setosa    
 [9] setosa     setosa     setosa     setosa     setosa     setosa     setosa     versicolor
[17] versicolor versicolor versicolor versicolor versicolor versicolor versicolor versicolor
[25] versicolor versicolor versicolor versicolor versicolor versicolor virginica  versicolor
[33] virginica  virginica  virginica  virginica  virginica  versicolor virginica  virginica 
[41] virginica  versicolor virginica  virginica  virginica 
Levels: setosa versicolor virginica
```

---

## Notes and Further Resources

- Classification trees are nonlinear models
    - They use interactions between variables
    - Data transformations may be less important (monotone transformation)
    - Trees can also be used for regression problems (continuous outcome)
- note that there are multiple tree building options in R both in the `caret` package -- [party](https://cran.r-project.org/web/packages/party/index.html), [rpart](https://cran.r-project.org/web/packages/rpart/index.html), and out of the `caret` package -- [tree](https://cran.r-project.org/web/packages/tree/index.html)
- [Introduction to Statistical Learning](http://www-bcf.usc.edu/~gareth/ISL/)
- [Elements of Statistical Learning](http://statweb.stanford.edu/~tibs/ElemStatLearn/)
- [Classification and Regression Trees](http://www.amazon.com/Classification-Regression-Trees-Leo-Breiman/dp/0412048418)
