---
layout: post
title:  "Statistics and Probability"
date:   2015-06-23
excerpt: "The science of producing unreliable facts from reliable figures"
tag:
- statistics
- proability
comments: true
---

## Types of data:
+ Numerical
+ Categorical
+ Ordinal

### Numerical data:

Measurable quantitatively
Eg: heights of people, page load times, stock prices, gas in tank, age in years, money spent in store

### Types:
+ Dicrete data(integer based)
+ Continuous data(infinite range of possiblities)

### Categorical data:

Data with no inherent mathematical meaning

Eg: Gender, yes/no questions, Race, states of residence, political party, product category

We often assign numbers to categories in order to represent them more compactly but the numbers have no mathematical meaning.

### Ordinal data:

Sort of the mix of the two above mentioned ones
Eg: Movie ratings on a scale of 1-5(but these values are not just categories, they do have mathematical meaning, ie 5 is better than a 1 meaning 5 for excellent movie and 1 for a worst movie)

---

## Statistics:

### Mean:
+ AKA Average
+ Sum/ No of samples

### Median:
+ Sort the values and take the one at the midpoint

Note: If the no of elements in the data set is even then avg of the midpoint elements is the median

***Median is less susceptible to outliners than the mean***

Mean household income in US is $72,000 while median is only $50,000 as the mean is skewed by a few billionaires.

### Mode:
+ Most common value in the data set(Not relevant to the continous data set)
+ Mode is the data which has the highest frequency

---

## Mean, median and mode using python

### Mean:
+ 27,000 - normal distribution
+ 15,000 - standard deviation
+ 10,000 - no of data point to generate

```python
import numpy as np
incomes = np.random.normal(27000,15000,10000) #creates data
np.mean(incomes)
```

### Plot the histogram:

```python
import matplotlib.pylot as plt
plt.hist(incomes,50) # 50 is the no of buckets for histogram
plt.show()
```

### Median:
`np.median(incomes) #computes the median`

### Mode:

```python
ages = np.random.randint(18,high=90,size=500) 
#generates 50 random integers between 18 and 90
from scipy import stats
stats.mode(ages)
```

---

## Variance: 
What is the spread or shape of the data distribution

Variance(sigma^2) is simply the average of the squared differences from the mean

---

## Standard deviation:
+ Square root of variance
+ Standard deviation is sigma
+ It is a way to identify the outliners
+ mean+/-standard deviation gives us the outliners
+ Data points that lie more than one standard deviation from the mean can be considered unusual

### Nuance:
If we are working with a subset of data(sample) instead of an entire data set(population) then we want to use the sample variance instead of the population variance. In sample variance we divide the squared variances by N-1 and not N

---

## Variance and standard deviation in python:
Standard deviation:
`incomes.std()`

Variance:
`incomes.var()`

---

## Probability density and mass function:
We have already seen normal distribution, that is an example of probability distribution function
 
### Probability density function:
Way to visualize probability of continuous data

With continuous data there are a range of values that can occur and hence the probability of a specific value to occur is infinitesimally small. Hence we need to think about probability of a range of values occuring

### Probability mass function:
Way to visualize probability of discrete data

---

## Common data distributions:

### Uniform distribution:
There is a flat common probability of a value occuring within a range
values.

```python
import numpy as np
import matplotlib.pyplot as plt

values = np.random.uniform(-10.0, 10.0, 100000)
# generates 10000 points of unifrom distribution between -10 and 10
plt.hist(values, 50)
plt.show()
```

### Normal/Guassian distribution:
We have already seen this
Normal and guassian both are same

Lets visualize the probability density function:

```python
from scipy.stats import norm
import matplotlib.pyplot as plt

x = np.arange(-3, 3, 0.001)
plt.plot(x, norm.pdf(x))
```

### Exponential probability distribution function(PDF)Power Law:

```python
from scipy.stats import expon
import matplotlib.pyplot as plt
x = np.arange(0,10,0.001)
# generate data with differences of 0.001 from 0 to 10 
plt.plot(x,expon.pdf(x)) 
# plot a graph with x-axis values as x and y axis values as pdf of x
```

### Binomial probability mass function:

```python
from scipy.stats import binom
import matplotlib.pyplot as plt
n, p = 10, 0.5
x = np.arange(0,10,0.001) #generate data with differences of 0.001 from 0 to 10 
plt.plot(x,binom.pmf(x,n,p)) #plot a graph with x-axis values as x and y axis values as pdf of x
```

### Poison probability mass function:

Ex: My website gets on average 500 visits per day. Whats the odds of getting 550?

```python
from scipy.stats import poisson
import matplotlib.pyplot as plt

mu = 500
x = np.arange(400,600,0.5)
plt.plot(x,poisson.pmf(x,mu))
```

---

## Percentile:
+ In a data set whats the point at which x% of the values are less than that value
+ 50% percentile means median (as half of the data set are making more and half less)

### Percentile using python:

```python
np.percentile(dataset,50) 
# so this gets the 50th percentile value
#note 50th percentile is same as median
```

90th percentile is the point at which 90% of the data is less than the given value

---

## Moments:
+ Way to measure the shape of the data distribution,ie probability distribution function
+ We denote it by u(n)

The ***first moment*** is the mean.

The ***second moment*** is the variance.

The ***third moment*** is called skew. It is basically a measure of how lopsided a distribution is. If their is a longer tail on the left then it is a negative skew and longer tail on the right is a positive skew
 
The ***fourth moment*** is called kurtosis
It is a measure of how thick the tail is and how sharp is the peak. Example: higher peaks have higher kurtosis

### Moments in python:

***First moment:***
`np.mean(dataset)`

***Second moment:***
`np.var(dataset)`

***Third moment:***
```python
import scipy.stats as sp
sp.skew(dataset)
```

***Fourth moment:***
`sp.kurtosis()`

---

## Matplotlib Basics:

### Draw a line graph:

```python
import numpy as np
from scipy.stats import norm
import matplotlib.pyplot as plt
x = np.arange(-3,3,0.001)
plt.plot(x,norm.pdf(x))
plt.show()
```

### Multiple plots on one graph:

All you have to do is call plt.plot() multiples times to draw multiple graphs before using plt.show()

```python
plt.plot(x,norm.pdf(x))
plt.plot(x,norm.pdf(x,1.0,0.5)) #pdf with mean of 1.0 and standard deviation of 0.5
plt.show()
```

### Save it to file: 
So instead of using plt.show() use,

`plt.savefig('complete_path/myplot.png',format='png')`


### Adjust the axis:

```python
axes = plt.axes() #get the axes
axes.set_xlim([-5,5]) #sets the x limit,ie range of x from -5 to 5
axes.set_ylim([0,1.0]) #sets the y limit,ie range of y from -5 to 5
#we can also set the tick marks in y and x axis
axes.set_xticks([-5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5])
axes.set_yticks([0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0])
plt.plot(x, norm.pdf(x))
plt.plot(x, norm.pdf(x, 1.0, 0.5))
plt.show()
```

### Add a grid:
`axex.grid()`

### Change line types and colors:

```python
plt.plot(x,norm.pdf(x),'b-') #b- indicates the style of the line so it means I need a blue solid line(- means solid)
#r: means a red line with little vertical hashes
#Similarly we have r--,r-.
```

### Labelling axes:

```python
plt.xlabel('time')
plt.ylabel('rate')
```

### Adding a legend:

```python
#this is nothing but a key for the graph, ie if you have 2 graphs then you can name the graphs as [graph1name,graph2name]
plt.legend([graph1name,graph2name],loc=4)
#the loc means location of the legend and 4 indicates bottom right
```

### XKCD style(COMIC BOOK LIKE STYLE):

```python
plt.xkcd()
#so now we go into the xkcd mode
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1)
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
plt.xticks([])
plt.yticks([])
ax.set_ylim([-30, 10])

data = np.ones(100)
data[70:] -= np.arange(30)

plt.annotate(
    'THE DAY I REALIZED\nI COULD COOK BACON\nWHENEVER I WANTED',
    xy=(70, 1), arrowprops=dict(arrowstyle='->'), xytext=(15, -10))

plt.plot(data)

plt.xlabel('time')
plt.ylabel('my overall health')
```

### Pie chart:

```python
#this will help us to go out of xkcd mode and enter the default mode
plt.rcdefaults()

values = [12, 55, 4, 32, 14]
colors = ['r', 'g', 'b', 'c', 'm']
explode = [0, 0, 0.2, 0, 0] //explodes the russian segment of the pie by 20%
labels = ['India', 'United States', 'Russia', 'China', 'Europe']
plt.pie(values, colors= colors, labels=labels, explode = explode)
plt.title('Student Locations')
plt.show()
```

### Bar chart:

```python
values = [12, 55, 4, 32, 14]
colors = ['r', 'g', 'b', 'c', 'm'] //c is cyan and m is magenta
plt.bar(range(0,5), values, color= colors) //range if x values is 0 to 5 and y-axis values to plot are given by the values dataset
plt.show()
```

### Scatter plot:

```python
X = randn(500)
Y = randn(500)
plt.scatter(X,Y)
plt.show()
```

### Box and Whisker plot:

The box represents the Inner Quartile Range(IQR) where 50% of the data resides,ie 25% resides on both halves of the box making it 50

We define outliners in box and whisker plt as anything beyond `1.5 x IQR`

```python
uniformSkewed = np.random.rand(100) * 100 - 40 // create uniform data

#now we create some outliners both high and low
high_outliers = np.random.rand(10) * 50 + 100
low_outliers = np.random.rand(10) * -50 - 100

#now we add the outliners to the data set,ie we concatenate
data = np.concatenate((uniformSkewed, high_outliers, low_outliers))

#we plot the graph
plt.boxplot(data)
plt.show()
```

---

## Covariance and correlation:
Say we have 2 attributes and want to know if they are related in some sense or not

### Covariance:
Measures how 2 variables are related to each other, ie how 2 variables vary in tandem from their means

### Measuring covariance mathematically:
1. Think of the data sets for the 2 variables as high dimensional vectors
2. Convert these to vectors of variances from the mean
3. Take the dot product(cosine of the sngle between them) of the 2 vectors
4. Divide by the sample size


### Interpreting covariance is hard:
We know a small covariance is close to 0 means not much correlation between the 2 variables. Large covariances means there is correlation, but how large? We cant say how large is good, so we have the concept of correlation
Thats where correlation comes in.


###Correlation:

Just divide the covariance by the standard deviations of both variables and that normalizes things
+ correlation of 0: means no correlation
+ correlation of 1: means perfect correlation
+ correlation of -1: means perfect inverse correlation(ie if one value increases the other decreases and vice versa)

---

## Covariance and Correlation in python

### Covariance Hard way:
Lets find the covariance ourselves from first principles

```python
import numpy as np
from pylab import *

def de_mean(x):
    xmean = mean(x)
    return [xi - xmean for xi in x]

def covariance(x, y):
    n = len(x)
    return dot(de_mean(x), de_mean(y)) / (n-1)

pageSpeeds = np.random.normal(3.0, 1.0, 1000)
purchaseAmount = np.random.normal(50.0, 10.0, 1000)

scatter(pageSpeeds, purchaseAmount)

covariance (pageSpeeds, purchaseAmount)
```

### Correlation Hard Way:
Similarly lets find the correlation from first priniciples.

```python
def correlation(x, y):
    stddevx = x.std()
    stddevy = y.std()
    return covariance(x,y) / stddevx / stddevy  #we divide by both the std deviations to normalize

correlation(pageSpeeds, purchaseAmount)
```

### Easy way(using numpy):

```python
np.corrcoef(pageSpeeds, purchaseAmount)
""" It returns a matrix of the correlation coefficients between every 
combination of the arrays passed in"""
# similarly we have np.cov func to compute the covariance directly
```

> Note: Correlation doesnt imply causality! If two variables say x and y have perfect correlation(ie 1), then it doesnt mean that x causes y or y causes x

---

## Conditional probability:

1. Probability of something happening given something happened
2. p(B given A) means probability of B happenning given A has occurred
3. p(B given A) = p(A,B)/P(A)
4. p(A,B) is probability of both A and B occurring independent of each other

If probability of E given F,ie P(E given F) is darn close to P(E), then it means E and F are not related to each other,ie independent variables 

---

## Bayes theorem:

Based on conditional probability:
P(a given b) = p(a)*p(b given a)/p(b)

Example: Drug test - 
If p(b given a) is high it doesnt mean p(a given b) is high
