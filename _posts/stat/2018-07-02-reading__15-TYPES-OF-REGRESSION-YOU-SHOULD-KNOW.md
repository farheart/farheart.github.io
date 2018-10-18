---
layout: post
title: Reading Notes - 15 TYPES OF REGRESSION YOU SHOULD KNOW
category: Stat
tags: Stat
keywords: Regression
published: true
---

# Reading Notes : 15 TYPES OF REGRESSION YOU SHOULD KNOW

> Ref: https://www.listendata.com/2018/03/regression-analysis.html

## Terminologies

### Mutlticollinearity
> Many types of regression techniques assumes multicollinearity should not be present in the dataset. It is because it causes problems in ranking variables based on its importance. Or it makes job difficult in selecting the most important independent variable (factor).

### Heteroscedasticity
> When dependent variable's variability is not equal across values of an independent variable, it is called heteroscedasticity

## Types of Regression

### Linear Regression

#### Assumptions of linear regression
- There must be a linear relation between independent and dependent variables
- There should not be any outliers present
- No heteroscedasticity
- Sample observations should be independent
- Error terms should be normally distributed with mean 0 and constant variance
- Absence of **multicollinearity** and **auto-correlation**

### Polynomial Regression

**Question: Why do we not care about the multicollinearity?**

### Logistic Regression

#### Why not use linear regression here?

- The homoscedasticity assumption is violated
- Errors are not normally distributed
- *y* follows binomial distribution and hence is not normal

#### Odds Ratio
exponential of coefficients == odds ratio for ith explanatory variable

#### Logistic Regression in R
- fit logistic regression with glm() function and we set family = "binomial"

```r
model <- glm(Lung.Cancer..Y.~Smoking..X.,data = data, family = "binomial")
```

### Quantile Regression
(TBD)