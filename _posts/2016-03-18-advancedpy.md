---
layout: post
title:  "Power of python"
date:   2016-03-18
excerpt: "A bird's eye view of some advanced concepts in Python"
tag:
- python 
- advanced
comments: true
---

## Namespaces

There are totally 3 namespaces in python:
1. Built-in namespace:
This is where all the built-in functions like len(),etc are defined
2. Global namespace
3. Local namespace(variable declared locally inside a function are not accessible outside)

To use a global variable inside a local namespace we need to use the 'global' keyword follwed by the var name to access it locally.

---

## Modules 

Modules allow us to group behaviour into modules. Module in python is essentially a file, thats it. Lets say we have a file named my_module.py(a simple python file) and it contains a variable name my_var and a function named my_func. 
Lets say we are now in a file name sample.py. We can import the module in the file in the following ways:

### Method 1:

```python
import my_module
# to access the function or var we need to use module_name.attrib
# we can create another var named my_var in this namespace
print(my_module.my_var)
my_module.my_func()
```
### Alternate method:

```python
from my_module import my_var, my_func
#when using from we need not use module_name.atrib wecan directly access
#so we cannot create a var with the same name my_var in this namespace
print(my_var)
my_func()
```
This means we are not importing everything in the file, we are importing only variables and functions that are needed/used. This is good wrt resourse management.

### Last method:
 
We can use `from my_module import *`, which means we are essentially importing everything from the file.
It loads the local namespace of the application we are importing into and hence generally not a good practice. 

When importing a module, pyhton intially checks if the module is in the same dir as that of the file. If it is then it imports it. If not then it checks the path set by the PYTHONPATH env variable, we can change/edit it. It is by default set to /usr/local/bin/python3.4, ie this is the directory it checks for if the module is not found in the same directory as that of the application file.

### Built-in and External Modules:

The python standard library has a no of [in-built modules](https://docs.python.org/3/library/index.html) which we can use for our own purposes.

`import module_name`

[External modules](https://pypi.python.org/pypi) are stored in this repository. Its called the PyPI index. Allows us to leverage other people's work and not re-invent the wheel.

Steps to include an external module:
1. Select python3 packages from the left side of the web page
2. Select the required package
3. Download the .tar.gz file
4. Unzip the package and locate the setup.py file 
5. Move to that directory
6. Run python3 setup.py install
7. Now import module_name in code and start using
8. To check if it has installed properly, open the REPL and import module_name shouldnt give us any error

---

## Unit testing

Python standard library includes a unit test module. It is very powerful and flexible to use.

```python
import saytime #module to test
import unittest #import this module for unit-testing 

class TestSaytime(unittest.TestCase): #this is how we define test suite
    #the class name can be anything but should inherit from TestCase

    #the setup method sets up the variables needed for the test case
    def setUp(self):
        self.nums = list(range(11))

    def test_numbers(self): #this method is a test case
        # make sure the numbers translate correctly
        words = (
            'oh', 'one', 'two', 'three', 'four', 'five',
            'six', 'seven', 'eight', 'nine', 'ten'
        )
        for i, n in enumerate(self.nums):
            self.assertEqual(saytime.numwords(n).numwords(), words[i])
            #assertEqual compares the passed 2 values and throws error if not equal

    def test_time(self): #this is another test case
        time_tuples = (
            (0, 0), (0, 1), (11, 0), (12, 0), (13, 0), (12, 29), (12, 30),
            (12, 31), (12, 15), (12, 30), (12, 45), (11, 59), (23, 15), 
            (23, 59), (12, 59), (13, 59), (1, 60), (24, 0)
        )
        time_words = (
            "midnight",
            "one past midnight",
            "eleven o'clock",
            "noon",
            "one o'clock",
            "twenty-nine past noon",
            "half past noon",
            "twenty-nine til one",
            "quarter past noon",
            "half past noon",
            "quarter til one",
            "one til noon",
            "quarter past eleven",
            "one til midnight",
            "one til one",
            "one til two",
            "OOR",
            "OOR"
        )
        for i, t in enumerate(time_tuples):
            self.assertEqual(saytime.saytime(*t).words(), time_words[i])

if __name__ == "__main__": unittest.main() 
#we call the main func from the unittest module
```
---

## Comprehensions

This concept allows us to build sequences in an easy and elegant way.

### List comprehensions:

```python
list = []
for i in range(10):
	j = i ** 2 # raised to power 2
	list1.append(j) 

#same code can be achieved in a single line using comprehension
list2 = [x ** 2  for x in range(10)]

#lets enter a conditional statement in this looping construct

#lets say we need only the numbers greater than 5 be raised to power 2
list3 = [x ** 2 for x in range(10) if x>5]
```

### Set comprehensions:

set1= {x ** 2 for x in range(10)}

### Dictionary comprehensions:

dict1 = {x:x+1 for x in range(10)}

dic2 = {k:v**2 for k,v in zip(['a','b','c'],range(3))}
#{'a':0,'b':1,'c':2}

---

## Lambdas(Anonymous functions)

Python allows us to create anonymous functions(no name). We can store these functions in variables. 

`lambda arg1,arg2...argN: expression`

```python
a = lambda x , y: x * y
#type(a) will be <type 'function'>
#a(2,10) will return 20

#lets combine lambda funct and comprehensions where we use nested loops

def myfunc(list):
	prod_list =[]
	for x in range(10):
		for y in range(5):
			product = x*y
			prod_list.append(product)
	return prod_list + list #concatenates the list passed with prod_list

#the same code can be achieved in a single line
b = lambda list: [x*y for x in range(10) for y in range(5)] + list
```

Lambdas are frequently used in conjuction with 3 other python functions:
1. Map
2. Filter 
3. Reduce

### Map:
Takes a function and sequence as an arg and applies the function to every element in the sequence and returns a map object which is iterable

`map((lambda a: a*10), list)`

Map function can also take muliple sequences as arguments. 

```python
a = [1,2,3]
b = [4,5,6]
c = [7,8,9]

map(lambda x,y:x+y,a,b) #takes 2 sequences as parameters
"""so this will return a map obj where the result is addition of individual indexes of a and b ie [5,7,9]"""

map(lambda x,y,z:x+y+z,a,b,c) #takes 3 sequences as parameters
"""so this will return a map obj where the result is addition of individual indexes of a,b and c ie [12,15,18]"""
```

### Filter:
The filter function also takes a function and the sequence as argument. It extracts all the elements in the sequence for which the function returns true. It returns back a filter object which is iterable.

`filter(lambda a:a>5,list)`

### Reduce:
Takes a function and a sequence as an argument. It takes the first 2 elements of the sequence and feeds it to the function and returns back the result. This result and the third arg is fed as arguments to the func which returns a result back. This keeps happening until the last element of the sequence is exhausted. So the final result of the filter func will be a single value.

```python 
#finding max element using reduce
max_find = lambda a,b: a if (a>b) else b
lst = [1,2,3,4,5,6,7,8]
max_elem = reduce(max_find, lst)
# returns 8(max element in the list)
```

```python
import functools
functools.reduce(lambda a,b:a+b,list) # returns the sum of the list`
```
Use functools.reduce() if you really need it; however, 99 percent of the time an explicit for loop is more readable.

---

## Other built-in functions

### Zip:

zip() makes an iterator that aggregates elements from each of the iterables.
Returns an iterator of tuples, where the i-th tuple contains the i-th element from each of the argument sequences or iterables. The iterator stops when the shortest input iterable is exhausted. With a single iterable argument, it returns an iterator of 1-tuples. With no arguments, it returns an empty iterator.

Infact we can define zip function ourselves:

```python
def zip(*iterables):
    # zip('ABCD', 'xy') --> Ax By
    sentinel = object()
    iterators = [iter(it) for it in iterables]
    while iterators:
        result = []
        for it in iterators:
            elem = next(it, sentinel)
            if elem is sentinel:
                return
            result.append(elem)
        yield tuple(result)
```

```python
#Zip the lists together
x = [1,2,3]
y = [4,5,6]
zip(x,y)
#[(1, 4), (2, 5), (3, 6)]

# Note how tuples are returned if one iterabel is longer than the other
x = [1,2,3]
y = [4,5,6,7,8]
zip(x,y)
#[(1, 4), (2, 5), (3, 6)]

# Zipping dicts together(just the keys are zipped)
d1 = {'a':1,'b':2}
d2 = {'c':4,'d':5}
zip(d1,d2)
# [('a', 'c'), ('b', 'd')]

# Keys and values of dict as tuples
zip(d1,d1.values())
# [('a',1),('b',2)]

#lets use zip a to switch the keys and values of the two dictionaries
def switch(d1,d2):
    dout = {}
    
    for d1key,d2val in zip(d1,d2.values()):
        dout[d1key] = d2val
    
    return dout

#lets use zip to return the max of corres elements in 2 lists
a=[1,2,3,4]
b=[21,22,23,5]
for pair in zip(a,b):
	print max(pair)

#it can also be achieved using map
map(lambda pair:max(pair),zip(a,b))
```

### Enumerate:

Enumerate allows us to keep a count as we iterate through an object. It does this by returning a tuple in the form (count,element). 

We can write this function ourselves:

```python
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
```

```python
lst = ['a','b','c']
for count,item in enumerate(lst):
    if count >= 2:
        break
    else:
        print item
#outputs a and b
```

### All and Any:

all() and any() are built-in functions in Python that allow us to convienently check for boolean matching in an iterable. all() will return True if all elements in an iterable are True. It is the same as this function code:

```python
def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
```
any() will return True if any of the elements in the iterable are True. It is equivalent to the following funciton code:

``` python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```

### Complex:

complex() returns a complex number with the value real + imag*1j or converts a string or number to a complex number.
If the first parameter is a string, it will be interpreted as a complex number and the function must be called without a second parameter. The second parameter can never be a string. Each argument may be any numeric type (including complex). If imag is omitted, it defaults to zero and the constructor serves as a numeric conversion like int and float. If both arguments are omitted, returns 0j.
If we are dealing with math or engineering that requires complex numbers (such as dynamics,control systems, or impedence of a circuit) this is a useful tool to have in Python.

```python
# Create 2+3j
complex(2,3)
# (2+3j)
#We can also pass strings:
complex('12+2j')
# (12+2j)
```

---

## Stacks

Lets implement stacks using Lists in python. Stack works on LIFO(last in first out) principle. 

```python
list = [1,2,3]
#lets push elements onto the top of the stack
list.append(4)
list.append(5)
#now lets pop elements out of the stack
val = list.pop()
# val stores the top element popped out
```
So append() func does the push operation and pop() function does the pop operation, ie deletes the top element of the stack and returns the value back.

---

## Queues

Queues works on FIFO(first in first out) principle. 

Lets implement Queues using Lists:

```python
list = [1,2,3]
#lets push elements in the queue
list.append(4)
list.append(5)
#now lets pop elements out of the queue
val = list.pop(0)
# val stores the element popped out in FIFO style
```

Lets implement queues using the collection module:

```python
from collections import deque
queue = deque([1,2,3])
#lets push elements in the queue
queue.append(4)
queue.append(5)
#now lets pop elements out of the queue
val = list.popleft()
# val stores the element popped out in FIFO style

```

---

## Package

Package in python is a collection of modules.

Steps for creating package:
1. Create a folder.(say a dir name calculator)
2. Go inside calculator/
3. Create a new file named "\_\_init__.py". Python is going to use this file to create a package for this dir
4. Create 2 sub dirs named complex and simple. We are essentially creating/breaking the namespace to hold different functionality.
5. Copy the "\_\_init__.py" file in both the sub dirs.
6. Now lets say we have 2 files name simple_calc.py in simple/ dir and complex_calc.py in complex/ dir.
7. simple_calc.py contains:

```python
def compute():
	print("This is simple")
```

8. complex_calc.py contains:

```python
def compute():
	print("This is simple")
```

Now lets use this:

```python
import calculator.simple.simple_calc
#just import the module with the full extension
calculator.simple.simple_calc.compute()

from calculator import simple
simple.simple_calc.compute()

from calculator.simple.simple_calc import compute
compute()
```

Note the folder structure we create is the name of the package we are creating just as in Java. Also the "\_\_init__.py" file is an empty python file. This file just lets know python this this directory is a package. So while creating packages/namespaces make sure you include this file in the sub-directories also.

---

## Randomize

```python
import random

rnd_no = random.randint(0,100) #generates a random no between 0-100
#randint is a method of random module

list = ['yellow','white','blue','green','red','black']

rand_color = random.choice(list) #gets a rand color from the list
#choice is a method of random module
```

---

## Date and time

Python has the datetime module to help deal with timestamps in your code. Time values are represented with the time class. Times have attributes for hour, minute, second, and microsecond. They can also include time zone information. The arguments to initialize a time instance are optional, but the default of 0 is unlikely to be what you want.

### Time:

Lets take a look at how we can extract time information from the datetime module. We can create a timestamp by specifying datetime.time(hour,minute,second,microsecond)

```python
import datetime

t = datetime.time(4, 20, 1)
# Lets show the different compoenets

print t
print 'hour  :', t.hour
print 'minute:', t.minute
print 'second:', t.second
print 'microsecond:', t.microsecond
print 'tzinfo:', t.tzinfo

"""prints the following:
04:20:01
hour  : 4
minute: 20
second: 1
microsecond: 0
tzinfo: None
"""
```

Note: A time instance only holds values of time, and not a date associated with the time.
We can also check the min and max values a time of day can have in the module:

```python
print 'Earliest  :', datetime.time.min
print 'Latest    :', datetime.time.max
print 'Resolution:', datetime.time.resolution

"""prints the following:
Earliest  : 00:00:00
Latest    : 23:59:59.999999
Resolution: 0:00:00.000001
"""
```

The min and max class attributes reflect the valid range of times in a single day.

### Date:

datetime (as you might suspect) also allows us to work with date timestamps. Calendar date values are represented with the date class. Instances have attributes for year, month, and day. It is easy to create a date representing today’s date using the today() class method.
Lets see some examples:

```python
today = datetime.date.today()
print today
print 'ctime:', today.ctime()
print 'tuple:', today.timetuple()
print 'ordinal:', today.toordinal()
print 'Year:', today.year
print 'Mon :', today.month
print 'Day :', today.day

"""prints the following:
2015-09-18
ctime: Fri Sep 18 00:00:00 2015
tuple: time.struct_time(tm_year=2016, tm_mon=6, tm_mday=5, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=4, tm_yday=261, tm_isdst=-1)
ordinal: 735859
Year: 2015
Mon : 9
Day : 18
"""
```

As with time, the range of date values supported can be determined using the min and max attributes.

```python
print 'Earliest  :', datetime.date.min
print 'Latest    :', datetime.date.max
print 'Resolution:', datetime.date.resolution

"""prints the following:
Earliest  : 0001-01-01
Latest    : 9999-12-31
Resolution: 1 day, 0:00:00
"""
```
Another way to create new date instances uses the replace() method of an existing date. For example, you can change the year, leaving the day and month alone.

```python
d1 = datetime.date(2015, 3, 11)
print 'd1:', d1

d2 = d1.replace(year=1990)
print 'd2:', d2
```

---

## Animations

We are going to use the tkinter module for the GUI. Lets walk through the window creation process.

```python
from tkinter import *

class Window(Frame): #we are importing from Frame(a class in tkinter)
	def __init__(self, master=None):
		Frame.__init__(self, master)
		self.master = master

root = Tk()
app = Window(root)
root.mainloop()
```

---

## Decorators

Decorators can be thought of as functions which modify the functionality of another function. They help to make your code shorter and more "Pythonic". We often come across this in python web frameworks like Django and Flask.

Lets create a simple function:

```python
def func():
	return 1

s = "This is a global variable" #global var
```

In python, functions create a new scope. We can use the globals() which return a dict back with the global var names as keys and values stored as values of the dict.

`print(globals().keys())` - Will return a list of keys,ie global vars

`print(globals()['s'])` - This will print 'this is a global variable' as globals() will return a dict and we are accessing the value of the key 's'

Everything in python is an object. Even functions are objects and can be binded to variables. 

`hello = func` - Here we are not calling func we are binding to to a var named hello

`hello()` - Will return 1 

```python
del func
hello() #will return 1, hello is not attached to func
```

### Functions within functions:

```python
def hello(name='Sailesh'):
    print 'The hello() function has been executed'
    
    def greet():
        return '\t This is inside the greet() function'
    
    def welcome():
        return "\t This is inside the welcome() function"
    
    print greet()
    print welcome()
    print "Now we are back inside the hello() function"

hello() #prints the following string below
    """
    The hello() function has been executed
		This is inside the greet() function
	 	This is inside the welcome() function
     Now we are back inside the hello() function
    """
"""if we call welcome(), it will complain saying not defined. Because welcome func it defined inside the hello scope and is not avail outside
"""

def hello(name='Sailesh'):
   
    def greet():
        return '\t This is inside the greet() function'
    
    def welcome():
        return "\t This is inside the welcome() function"
    
    if name == "Sailesh"
    return greet
    else 
    return welcome

    """we are returing the func greet and not value returned by greet func, in that case it would be return greet()"""

x = hello()
# x is now the greet func
```

Now lets look at the concept of decorator functions in python.

### Creating a decorator:

```python
#this is the decorator function which adds new functionality
def new_decorator(func):

    def wrap_func():
        print "Code would be here, before executing the func"

        func()

        print "Code here will execute after the func()"

    return wrap_func

def func_needs_decorator():
    print "This function is in need of a Decorator"

func_needs_decorator()
#will print 'This function is in need of a Decorator'

# Reassign func_needs_decorator
func_needs_decorator = new_decorator(func_needs_decorator)

func_needs_decorator()
"""will print:
Code would be here, before executing the func
This function is in need of a Decorator
Code here will execute after the func()
"""
```

A decorator simply wrapps the function and modifies its behaviour. Now lets understand how we can rewrite this code using the @ symbol, which is what Python uses for Decorators.

```python
@new_decorator 
def func_needs_decorator():
    print "This function is in need of a Decorator"
#python uses @ to understand that new_decorator is the decorator func

func_needs_decorator()
"""will print:
Code would be here, before executing the func
This function is in need of a Decorator
Code here will execute after the func()
"""
```

The code above is the same as `func_needs_decorator = new_decorator(func_needs_decorator)`. It is essentially the same thing. We are reassigning the definition of the function using the decorator function.We have now built a Decorator manually and used the @ symbol in Python to automate this.

---

## Iterators and Generators

Generators allow us to generate as we go along, instead of holding everything in memory.
range() function in Python 2 and the similar xrange(), with the difference being the xrange() is a generator.

Lets explore a little deep. Generator functions allow us to write a function that can send back a value and then later resume to pick up where it left off. This type of function is a generator in Python, allowing us to generate a sequence of values over time. The main difference in syntax will be the use of a yield statement.

In most aspects, a generator function will appear very similar to a normal function. The main difference is when a generator function is compiled they become an object that support an iteration protocol. 

That means when they are called in your code the don't actually return a value and then exit, the generator functions will automatically suspend and resume their execution and state around the last point of value generation. 

The main advantage here is that instead of having to compute an entire series of values upfront and the generator functions can be suspended, this feature is known as ***state suspension***.

Lets go ahead and see how we can create some.

```python
# Generator function for the cube of numbers (power of 3)
def gencubes(n):
    for num in range(n):
        yield num**3

for x in gencubes(10):
    print x
```

Now since we have a generator function we don't have to keep track of every single cube we created. Generators are best for calculating large sets of results (particularly in calculations that involve loops themselves) in cases where we don’t want to allocate the memory for all of the results at the same time.
Many Standard Library functions that return lists in Python 2 have been modified to return generators in Python 3.

Lets create another example generator which calculates fibonacci numbers:

```python
def genfibon(n):
    '''
    Generate a fibonnaci sequence up to n
    '''
    a = 1
    b = 1
    for i in range(n):
        yield a
        a,b = b,a+b #tuple unpacking happens

for num in genfibon(10):
    print num
```

What is this was a normal function, what would it look like?

```python
def fibon(n):
    a = 1
    b = 1
    output = []
    
    for i in range(n):
        output.append(a)
        a,b = b,a+b
        
    return output

fibon(10) #returns a list of first 10 fibonnaci nos
```
Notice that if we call some huge value of n (like 100000) the second function will have to keep track of every single result, when in our case we actually only care about the previous result to generate the next one!

### next() and iter() built-in functions:

A key to fully understanding generators is the next function() and the iter() function.
The next function allows us to access the next element in a sequence. Lets check it out:

```python
def simple_gen():
    for x in range(3):
        yield x

# Assign simple_gen 
g = simple_gen()

print next(g)
# returns 0

print next(g)
# returns 1

print next(g)
# returns 2

print next(g)
# StopIteration Error
```

After yielding all the values next() caused a StopIteration error. What this error informs us of is that all the values have been yielded.
You might be wondering that why don’t we get this error while using a for loop? The for loop automatically catches this error and stops calling next.

Lets go ahead and check out how to use iter(). You remember that strings are iterables:

```python
s = 'hello'
#Iterate over string
for let in s:
    print let
```
But that doesn't mean the string itself is an iterator! We can check this with the next() function:

`next(s)` #gives 'TypeError: str object is not an iterator'

Interesting, this means that a string object supports iteration, but is not a iterator. We can not directly iterate over it as we could with a generator function. The iter() function allows us to do just that!

```python
s_iter = iter(s)
next(s_iter)
# returns 'h'
next(s_iter)
#returns 'e'
```
Great! Now we know how to convert objects that are iterable into iterators themselves!

---

## Collections

The collections module is a built-in module that implements specialized container datatypes providing alternatives to Python’s general purpose built-in containers.

### Counter:

Counter is a dict subclass which helps count hashable objects. Inside of it elements are stored as dictionary keys and the counts of the objects are stored as the value.


```python
from collections import Counter

l = [1,1,1,1,1,2,2,2,2,3,3,3,4,4,5]
s = "aabbccddd"

c = c.counter(l)
dict(c)
#returns a dict back {1:5,2:4,3:3,4:2,5:1}

words = "lets count the no of words in this string lets string count"
wordarr = words.split()
c = Counter(wordarr) 

c.most_common(2)
#show the 2 most common words
#will return a list if tuple pairs [('lets',2),('string',2)]
```

### Common patterns with Counter() object

```python
sum(c.values())                 # total of all counts
c.clear()                       # reset all counts
list(c)                         # list unique elements
set(c)                          # convert to a set
dict(c)                         # convert to a regular dictionary
c.items()                       # convert to a list of (elem, cnt) pairs
Counter(dict(list_of_pairs))    # convert from a list of (elem, cnt) pairs
c.most_common()[:-n-1:-1]       # n least common elements
c += Counter()                  # remove zero and negative counts
```

### defaultdict

defaultdict is a dictionary like object which provides all methods provided by dictionary but takes first argument (default_factory) as default data type for the dictionary. Using defaultdict is faster than doing the same using dict.set_default method.

A defaultdict will never raise a KeyError. Any key that does not exist gets the value returned by the default factory.

```python
from collections import defaultdict

#normal dictionary
d = {}

d['one'] 
#gives a key error back as the key doesnt exist

d  = defaultdict(object)
#this is how we create a defaultdict

d['one'] 
"""wont raise an error and will return back object as that is what we passed as an arg while creating the dictionary"""

""" So this helps us in returing a defalt value back if the key doesnt exist"""

d = defaultdict(lambda: 0)
# we can use lambda functions to create default dict
# this means that return 0 back if the key doesnt exist
```

### OrderedDict

An OrderedDict is a dictionary subclass that remembers the order in which its contents are added.

A normal dict is just an un-ordered collection, ie just a mapping and doesnt record the order in which they are stored.

```python
print 'Normal dictionary:'

d = {}

d['a'] = 'A'
d['b'] = 'B'
d['c'] = 'C'
d['d'] = 'D'
d['e'] = 'E'

for k, v in d.items():
    print k, v

"""will print:
Normal dictionary:
a A
c C
b B
e E
d D
"""
```

An Ordered Dictionary:

```python
from collections import OrderedDict

print 'OrderedDict:'

d = OrderedDict()

d['a'] = 'A'
d['b'] = 'B'
d['c'] = 'C'
d['d'] = 'D'
d['e'] = 'E'

for k, v in d.items():
    print k, v

"""will print:
OrderedDict:
a A
b B
c C
d D
e E
"""
```

### Equality with an Ordered Dictionary:

A regular dict looks at its contents when testing for equality. An OrderedDict also considers the order the items were added.

A normal Dictionary:

```python
d1 = {}
d1['a'] = 'A'
d1['b'] = 'B'

d2 = {}
d2['b'] = 'B'
d2['a'] = 'A'

print d1 == d2
# will print True
```

An Ordered Dictionary:

```python
d1 = collections.OrderedDict()
d1['a'] = 'A'
d1['b'] = 'B'

d2 = collections.OrderedDict()

d2['b'] = 'B'
d2['a'] = 'A'

print d1 == d2
#will print False
```

## namedtuple

The standard tuple uses numerical indexes to access its members, for example:

```python
t = (12,13,14)
t[0]
#returns the first element
```

For simple use cases, this is usually enough. 
On the other hand, remembering which index should be used for each value can lead to errors, especially if the tuple has a lot of fields and is constructed far from where it is used. 

A namedtuple assigns names, as well as the numerical index, to each member.

namedtuple can basically be seen as a very quick way of creating a new object/class type with some attribute fields. They are created by using the ***namedtuple()*** factory function. 

The arguments are the name of the new class and a string containing the names of the elements.

```python
from collections import namedtuple

Dog = namedtuple('Dog','age breed name')
# first arg is a strign which is the name of the class we are creating
"""the second arg is again a string which is a list of attributes seperated by space"""

# so we have created a dog which can have a age, breed and a name

#now lets create a dog,ie an instance
sam = Dog(age=2,breed='Lab',name='Sammy')

#another instance
frank = Dog(age=2,breed='Shepard',name="Frankie")

frank.age
#returns 2

frank[0]
#we can also call the atrribute by the index
#will return 2(age)
```

---

## Python debugger

We have probably used a variety of print statements to try to find errors in your code. A better way of doing this is by using Python's built-in debugger module (pdb). 

The pdb module implements an ***interactive*** debugging environment for Python programs. It includes features to let us pause our program, look at the values of variables, and watch program execution step-by-step.

Here we will create an error on purpose, trying to add a list to an integer:

```python
x = [1,3,4]
y = 2
z = 3

result = y + z
print result
result2 = y+x
print result2
# will raise an error as we are tring to adda list to an int
```

Lets implement a set_trace() using the pdb module. This will allow us to basically pause the code at the point of the trace and check if anything is wrong.

```python
import pdb

x = [1,3,4]
y = 2
z = 3

result = y + z
print result

# Set a trace using Python Debugger
"""so this basically stops the execution of the program here and opens up an interactive debugger. We can use the debugger to look at the values of variables"""
pdb.set_trace()

result2 = y+x
print result2
```
We could check what the various variables were and check for errors. We can use 'q' to quit the debugger.

---

## StringIO module

The StringIO module implements an in-memory ***file like object***. This object can then be used as input or output to most functions that would expect a standard file object.

The best way to show this is by example:

```python
import StringIO

# Arbitrary String
message = 'This is just a normal string.'

# Use StringIO method to set as file object
f = StringIO.StringIO(message)

"""Now we have an object f that we will be able to treat just like a file. For example:"""

f.write(' Second line written to file like object')

# Reset cursor just like you would a file
f.seek(0)

# Read again
f.read()
"""returns
'This is just a normal string. Second line written to file like object'
"""
```

We can use StringIO to turn normal strings into in-memory file objects in our code. This kind of action has various use cases, especially in web scraping cases where we want to read some string we scraped as a file.

---

## Numpy

```python 
import numpy as np

array_l = np.array([[0,1],[10,11],[21,21]],float) #typecasting
""" Even though we pass the elements in the list as ints, the float, ie second parameter ensures that all the elements are saved as floats, typecasting is handled """

""" The above code passes a list of list so essentially a 2d matrix is creating with 3 rows"""

a_list = [0,1,10,11,20,21]
vector = np.array([[0,1],[10,11],[21,21]],float) #typecasting is handled
""" This is just a 1d array, ie a vector"""

vector[0] # we can use the normal indexing

vector[::-1] # we can use the standard slicing techniques

array_2 = np.array([[0,1],[10,11],[21,21]],float).reshape((3,2))
""" We reshaping we can change the dimentionality, array_2 is same as array_l, ie it is a 2d array with 3 rows"""

array_2 * 4 #returns a new matrix with each element multiplied by 4

array_2 * array_2 
""" This is not array multiplication, this is just multiplying element by element in both the arrays"""

array_2.transpose() #gives the transpose
```

## Databases

There are many choices of dbs for python. 

### SQLite3

Lets make use of SQLite3(as it comes built-in with python):

Benefits of SQLite3:
1. Its reliable and simple
2. Doesnt require seperate db engine
3. Its self-contained
4. Server-less and zero configuration
5. Fully transactional

```python
import sqlite3

class database:
    def __init__(self, **kwargs):
        self.filename = kwargs.get('filename')
        self.table = kwargs.get('table', 'test')
    
    def sql_do(self, sql, *params):
        self._db.execute(sql, params)
        self._db.commit()
    
    def insert(self, row):
        self._db.execute('insert into {} (t1, i1) values (?, ?)'.format(self._table), (row['t1'], row['i1']))
        self._db.commit()
    
    def retrieve(self, key):
        cursor = self._db.execute('select * from {} where t1 = ?'.format(self._table), (key,))
        return dict(cursor.fetchone())
    
    def update(self, row):
        self._db.execute(
            'update {} set i1 = ? where t1 = ?'.format(self._table),
            (row['i1'], row['t1']))
        self._db.commit()
    
    def delete(self, key):
        self._db.execute('delete from {} where t1 = ?'.format(self._table), (key,))
        self._db.commit()

    def disp_rows(self):
        cursor = self._db.execute('select * from {} order by t1'.format(self._table))
        for row in cursor:
            print('  {}: {}'.format(row['t1'], row['i1']))

    def __iter__(self):
        cursor = self._db.execute('select * from {} order by t1'.format(self._table))
        for row in cursor:
            yield dict(row)

    @property
    def filename(self): return self._filename

    @filename.setter
    def filename(self, fn):
        self._filename = fn
        self._db = sqlite3.connect(fn)
        self._db.row_factory = sqlite3.Row

    @filename.deleter
    def filename(self): self.close()

    @property
    def table(self): return self._table
    @table.setter
    def table(self, t): self._table = t
    @table.deleter
    def table(self): self._table = 'test'

    def close(self):
            self._db.close()
            del self._filename

def main():
    db = database(filename = 'test.db', table = 'test')

    print('Create table test')
    db.sql_do('drop table if exists test')
    db.sql_do('create table test ( t1 text, i1 int )')

    print('Create rows')
    db.insert(dict(t1 = 'one', i1 = 1))
    db.insert(dict(t1 = 'two', i1 = 2))
    db.insert(dict(t1 = 'three', i1 = 3))
    db.insert(dict(t1 = 'four', i1 = 4))
    for row in db: print(row)

    print('Retrieve rows')
    print(db.retrieve('one'), db.retrieve('two'))

    print('Update rows')
    db.update(dict(t1 = 'one', i1 = 101))
    db.update(dict(t1 = 'three', i1 = 103))
    for row in db: print(row)

    print('Delete rows')
    db.delete('one')
    db.delete('three')
    for row in db: print(row)

if __name__ == "__main__": main()

"""The row factory allows us to specify how the row are returned from the cursor. 
It we use sqlite3. Row then the cursor obj contains sqlite3.Row objects instead of tuples. 
The row objects can be looked as dictionaries."""
``` 

### MySQL

Lets now deal with MySQL. Install the MySQLdb package.

***Do the following steps:***

1. Install mysql

2. Login into mysql, ie mysql -u root -p

3. Create a db, ie create database database_name

4. Create a user and grant all permissions on the database

```
create user 'username'@'localhost' identified by 'password' 
use database_name;
grant all on database_name.* to 'username'@'localhost';
```

5. Create a table 

6. Fill in the table with data(use insert query)


***Python code to handle db operations:***

```python
import MySQLdb as mdb
# The above line imports the module and creats an alias 

sql_host = "localhost"
sql_username="user_name"
sql_password="python"
sql_database = "database_name"

sql_connection = mdb.connect(sql_host, sql_username,sql_password,sql_database)

#print(sql_connection)

cursor = sql_connection.cursor()

cursor.execute("use database_name")

cursor.execute("can pass in literally any SQL statement here")

# cursor.execute("select * from table_name")
# output = cursor.fetchone() #fetches row by row and returns back tuple

sql_connection.commit() #commits the transaction

sql_connection.close() 
```











































































 



























	


 
































