# Regularized Regression



## Basic Idea

1. Fit a regression model
2. Penalize (or shrink) large coefficients

**Pros:**

- Can help with the bias/variance tradeoff
- Can help with model selection

**Cons:**

- May be computationally demanding on large datasets
- Does not perform as well as random forests and boosting

---

## A Motivating Example

$$Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \epsilon$$

where $X_1$ and $X_2$ are nearly perfectly correlated (co-linear). You can approximate this model by:

$$Y = \beta_0 + (\beta_1 + \beta_2) X_1 + \epsilon$$

The result is:

- You will get a good estimate of $Y$
- The estimate (of Y) will be biased
- We may reduce variance in the estimate

---

## Prostate Cancer


```r
library(ElemStatLearn)
data(prostate)
str(prostate)
```

```
'data.frame':	97 obs. of  10 variables:
 $ lcavol : num  -0.58 -0.994 -0.511 -1.204 0.751 ...
 $ lweight: num  2.77 3.32 2.69 3.28 3.43 ...
 $ age    : int  50 58 74 58 62 50 64 58 47 63 ...
 $ lbph   : num  -1.39 -1.39 -1.39 -1.39 -1.39 ...
 $ svi    : int  0 0 0 0 0 0 0 0 0 0 ...
 $ lcp    : num  -1.39 -1.39 -1.39 -1.39 -1.39 ...
 $ gleason: int  6 6 7 6 6 6 6 6 6 6 ...
 $ pgg45  : int  0 0 20 0 0 0 0 0 0 0 ...
 $ lpsa   : num  -0.431 -0.163 -0.163 -0.163 0.372 ...
 $ train  : logi  TRUE TRUE TRUE TRUE TRUE TRUE ...
```

---

## Subset Selection

![](prostate1.JPG)

[Code here](http://www.cbcb.umd.edu/~hcorrada/PracticalML/src/selection.R)

---

## Most Common Pattern

![](modelerror.JPG)

[http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/](http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/)

---

## Model Selection Approach: Split Samples

- No method better when data/computation time permits
- Approach:
    1. Divide data into training/test/validation
    2. Treat validation as test data, train all competing models on the train data and pick the best one on validation
    3. To appropriately assess performance on new data apply to test set
    4. You may re-split and reperform steps 1-3
- Two common problems:
    - Limited data
    - Computational complexity

[http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/](http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/)

[http://www.cbcb.umd.edu/~hcorrada/PracticalML/](http://www.cbcb.umd.edu/~hcorrada/PracticalML/)

---

## Decomposing Expected Prediction Error

Assume $Y_i = f(X_i) + \epsilon_i$

$EPE(\lambda) = E\left[{Y - \hat f_\lambda (X)}^2\right]$

Suppose $\hat f_\lambda$ is the estimate from the training data and look at a new data point $X = x*$

$$
E\left[{Y - \hat f_\lambda (x*)}^2\right] = \sigma^2 + {E[\hat f_\lambda (x*)] - f(x*)}^2 + var[\hat f_\lambda(x_0)] \\
= \mbox{Irreducible error + Bias}^2 \ \mbox{+ Variance}
$$

[http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/](http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/)

[http://www.cbcb.umd.edu/~hcorrada/PracticalML/](http://www.cbcb.umd.edu/~hcorrada/PracticalML/)

---

## Another Issue for High-Dimensional Data


```r
small = prostate[1:5,]
lm(lpsa ~ ., data=small)
```

```

Call:
lm(formula = lpsa ~ ., data = small)

Coefficients:
(Intercept)       lcavol      lweight          age         lbph          svi          lcp  
    9.60615      0.13901     -0.79142      0.09516           NA           NA           NA  
    gleason        pgg45    trainTRUE  
   -2.08710           NA           NA  
```

[http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/](http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/)

[http://www.cbcb.umd.edu/~hcorrada/PracticalML/](http://www.cbcb.umd.edu/~hcorrada/PracticalML/)

---

## Hard Thresholding

- Model $Y = f(X) + \epsilon$
- Set $\hat f_\lambda(x) = x'\beta$
- Constrain only $\lambda$ coeffficients to be non-zero
- Selection problem is after chosing $\lambda$ figure out which $p - \lambda$ coefficients to make non-zero

[http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/](http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/)

[http://www.cbcb.umd.edu/~hcorrada/PracticalML/](http://www.cbcb.umd.edu/~hcorrada/PracticalML/)

---

## Regularization for Regression

If the $\beta_j$'s are unconstrained:

- They can explode
- And hence are susceptible to very high variance

To control variance, we might regularize/shrink the coefficients

$$PSRR(\beta) = \sum_{j=1}^n(Y_j - \sum_{i=1}^m\beta_{1i}X_{ij})^2 + P(\lambda;\beta)$$

where _PRSS_ is a penalized form of the sum of squares. Things that are commonly looked for

- Penalty reduces complexity
- Penalty reduces variance
- Penalty respects structure of the problem

---
