---
layout: post
title:  "Predictive Analytics"
date:   2016-03-18
excerpt: "A brief introduction to predictive models using python"
tag:
- mathematical models 
- predictive models
- regression
comments: true
---

The goal of developing a predictive model is to develop a model that is accurate on unseen data. This can be achieved using statistical techniques where the training dataset is carefully used to estimate the performance of the model on new and unseen data.

## Regression analysis:
Try to fit a function to a set given values and predict values we haven seen yet

## Linear regression:
* Fitting a straight line to a set of observations
* And use this line to predict unobserved values

This is the same as maximizing the likelihood of the observed data in terms of probabilities. Sometimes called "maximum likelihood estimation"

### How does it work?

We have 2 methods:

### Ordinary least squares(OLS):
Minimizes the squared error between each point and the line(least squares)

### Gradient Descent:
Gradient descent is an alternative method(can make sense when dealing with 3D data)

### How good is my regression? 

How good is my fitting line?
It is determined by R-squared(also called coefficient of determination). It is the fraction of the total variantion in Y,
that is captured by the model.

R-squared = (1.0 - Sum of squared errors)/ Sum of squared variance from mean

It ranges from 0 to 1:
* 0 is bad(none of the variances is captured)
* 1 is good(all the variances on both sides of the fitting line is captured)

We have many models like linear regression. R-squared is a quantitative measure of the suitability of the model. Depending on its value we can choose the model that suits the problem best.

---

## Linear regression with python:

So lets create 2 variables which have a linear relationship and lets work on those datasets.

```python
import numpy as np
from pylab import *

pageSpeeds = np.random.normal(3.0, 1.0, 1000)
purchaseAmount = 100 - (pageSpeeds + np.random.normal(0, 0.1, 1000)) * 3
#so we are creatng 2 vars which linearly depend on each other
scatter(pageSpeeds, purchaseAmount)
```

Now lets use the scipy package to find the linear line and r-squared value of the linear regression model.

```python
from scipy import stats
slope, intercept, r_value, p_value, std_err = stats.linregress(pageSpeeds, purchaseAmount)
# so essentially tuple unpacking happens here
# slope and intercept are the parameters of the line that fits the model
# r_value**2 will talk about the quality of this linear model we are trying to fit
```

Now lets use the slope and intercept to plot the linear model along side the original data

```python
import matplotlib.pyplot as plt

def predict(x):
    return slope * x + intercept

    fitLine = predict(pageSpeeds)

    plt.scatter(pageSpeeds, purchaseAmount)
    plt.plot(pageSpeeds, fitLine, c='r')
    plt.show()
```

So if the r-squared value is close to 1, then it means that the model is suitable and hence we can use the line equation to
predict new values. Here in this case r value will be close to 1(as we created the data sets to be linearly dependent). Hence,
we can use the above predict() function to predict new values as this model is a good fit.

---

## Polynomial regression:

* Why limit to straight lines, may be we cant capture our data in a straight line and there is a curve to it
* Not all relationships are linear
* If we do polynomial regression for degree 2 we get a,b,c values of ax^2+bx+c
* Similarly for polynomial regression for degree 3 we get a,b,c,d and so on

## Beware overfitting:
1. Dont use more degrees than you need
2. Vizualize your fit
3. A higher r-squared simply means your curve fits your training data well, but it may not be a good predictor

---

## Polynomial regression using python:

What if your data doesnt look linear at all? Lets look at some more realistic-looking page speed / purchase data:

```python
from pylab import *

np.random.seed(2) #allows us to generate the same random values over and over again
pageSpeeds = np.random.normal(3.0, 1.0, 1000)
purchaseAmount = np.random.normal(50.0, 10.0, 1000) / pageSpeeds

scatter(pageSpeeds, purchaseAmount)
```
    
numpy has a handy polyfit function we can use, to let us construct an nth-degree polynomial model of our data that minimizes squared error. Lets try it with a 4th degree polynomial:

```python
x = np.array(pageSpeeds)
y = np.array(purchaseAmount)

p4 = np.poly1d(np.polyfit(x, y, 4)) #so np.polyfit does the job and 4 means we need python to perform 4th degree polynomial regression
# np.polyfit returns the coefficients as np.ndarray 
# np.ploy1d constructs the polynomial with those coeffiecients, so now p4(2) gives the value of the poly at 4
```

We will visualize our original scatter plot, together with a plot of our predicted values using the polynomial for page speed times ranging from 0-7 seconds:

```python
import matplotlib.pyplot as plt

xp = np.linspace(0, 7, 100)
# means start is 0 and end is 7 and we need 100 equi-spaced points
# in np.arange we specify start,stop and step(not no of points) unlike np.linspace
plt.scatter(x, y)
plt.plot(xp, p4(xp), c='r')
plt.show()
```
Lets measure the r-squared error:

```python
from sklearn.metrics import r2_score

r2 = r2_score(y, p4(x)) //r2_score is a func in sklearn.metrics
print r2
```

---

## Multivariate Regression

What happens if we are predecting a value and it is based on more than one attribute?
Ex: height depends not only on weight but also on genetics

Thats where Multivariate Regression analysis comes into place

Multivariate Regression(More than one factor influences):
Ex: Predicting the price for a car depends on body style, mileage, brand,etc and should take into account all the factors

### How it works?
+ Still uses the least squares 
+ We end up with coeffients for each factor
+ These coefficients imply how important each factor is(if the data is all normalized).
These coefficients inply how important each factor is.
+ Need to assume the different factors are not themselves dependent on each other.
+ Get rid of ones that dont matter!
+ Can still measure fit with r-squared

## Multivariate regression using python:

Lets find the effect of mileage, model and no of doors on a car price.

We can use pandas to split up this matrix into the feature vectors we are interested in, and the value we are trying to predict.
Note how we use pandas.Categorical to convert textual category data (model name) into an ordinal number that we can work with.

```python
# lets us deal with tabular data easier
import pandas as pd

df = pd.read_excel('path_can_be_even_from_web')
# df is a dataFrame obj
df.head()

import statsmodels.api as sm

df['Model_ord'] = pd.Categorical(df.Model).codes # we are converting the model column to number so that we can work with it
# df.Model is same as df['Model'] which is a Series obj
# pd.Categorical(df.Model).codes returns an np.ndarray back
X = df[['Mileage', 'Model_ord', 'Doors']]
y = df[['Price']]
# X, y are dataFrame objs

X1 = sm.add_constant(X) 
#the model needs an extra column for intercept, so we are craeting a new column named constant and filling all values with 1
est = sm.OLS(y, X1).fit()

est.summary()
#this actually returns a Summary obj
```
We get a table back which summarizes the fitting.
The table of coefficients gives us the values to plug into an equation of form:
`B0 + B1 * Mileage + B2 * model_ord + B3 * doors`
        
But in this example, its pretty clear that mileage is more important than anything based on the std errs in the table.

---

## Multi-level models:

The concept is that some effects happens at various levels. Multi-level models attempts to model and account for these interdepencies.

Ex: health,wealth
Identify the factors that affect the outcome we are trying to predict at each level
































