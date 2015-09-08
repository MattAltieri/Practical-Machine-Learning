# Types of Errors



## Basic Terms

In general, **Positive** = identified and **negative** = rejected. Therefore:

- **True positive** - correctly identified
- **False positive** - incorrectly identified
- **True negative** - correctly rejected
- **False negative** - incorrectly rejected

_Medical testing example:_

- **True positive** - Sick people correctly diagnosed as sick
- **False positive** - Healthy people incorrectly identified as sick
- **True negative** - Healthy people correctly identified as healthy
- **False negative** - Sick people incorrectly identified as healthy

[https://en.wikipedia.org/wiki/Sensitivity_and_specificity](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)

---

## Key Quantities

![](keyquant.JPG)

[https://en.wikipedia.org/wiki/Sensitivity_and_specificity](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)

---

## Key Quantities as Fractions

![](keyquant2.JPG)

[https://en.wikipedia.org/wiki/Sensitivity_and_specificity](https://en.wikipedia.org/wiki/Sensitivity_and_specificity)

---

## Screening Tests

![](screening.JPG)

[http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/](http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/)

---

## General Population

![](genpop.JPG)

[http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/](http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/)

---

## General Population as Fractions

![](genpop2.JPG)

[http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/](http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/)

---

## At Risk Population

![](atrisk.JPG)

[http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/](http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/)

---

## At Risk Population as Fractions

![](atrisk2.JPG)

[http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/](http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/)

---

## Key Public Health Issue

![](keyissue.JPG)

[http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/](http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/)

---

## Key Public Health Issue

![](keyissue2.JPG)

---

## For Continuous Data

**Mean Squared Error (MSE):**

$$
\frac{1}{n}\sum_{i=1}^n(Prediction_i - Truth_i)^2
$$

**Root Mean Squared Error (RMSE):**

$$
\sqrt{\frac{1}{n}\sum_{i=1}^n(Prediction_i - Truth_i)^2}
$$

---

## Common Error Measures

1. Mean squared error (or root mean squared error)
    - Continuous data, sensitive to outliers
2. Median absolute deviation
    - Continuous data, often more robust
3. Sensitivity (recall)
    - If you want few missed positives
4. Specificity
    - If you want few negatives called positives
5. Accuracy
    - Weights false positives/negatives equally
6. Concordance
    - One example is [kappa](https://en.wikipedia.org/wiki/Cohen%27s_kappa)
