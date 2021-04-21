---
title: Additional Exercises
teaching: 1
exercises: 0
questions:
- "Practice your python."
objectives:
- "Work on some python exercises to continue building your skills."
keypoints:
- "Practice makes Python."
---


# Python Practice

The key to building your skills as a programmer is _practice_. Try giving yourself challenges to use the Python tools we have learned in class. Here are some exercises for you to work through to continue working on becoming a Pythonista. 

## Python Datatypes

1. Create a list in python `x = list(range(1,20))`. This will make a list of numbers from 1 to 20. Now try the following with this list:
    1. Create a new list containing every 2nd element starting with element `x[1]`.
    2. Print the last 8 values in the list to screen. 
    3. Write a `for` loop to print each value, one at a time. 
    4. Write a function that converts the entire list to a single string.

2. Create 3 dictionaries:
```
d1 = {'a': 10, 'b': 20, 'c': 30}
d1 = {'d': 40, 'e': 50}
d1 = {'f': 60, 'g': 70, 'h': 80}
```
    1. Concatenate the dictionaries into one single dictionary called `my_dictionary`
    2. Check to see if `my_dictionary` has the key `i`. 
    3. Change the value associated with key `d` to `40.4`

## Math

1. Use the Numpy package to draw 100 random variables from a normal distribution with mean = 5 and standard deviation = 1. Assign these to a list called `norm_rvs`.
2. Use Numpy to compute the sum of the list `x` crated in the very first exercise. 
3. Write a function that takes the list `norm_rvs` and returns a list where each element is the `log(norm_rvs[i])`.
4. Use the seaborn package to view histograms of `norm_rvs` and the log-transformed values. 

## Pandas

1. Load the CSV file from our previous exercises: `surveys_df = pd.read_csv("surveys.csv")`
    1. Create a new dataframe variable called `surveys1997` containing only the observations from the year 1997.
    2. Use a `for` loop to iterate over `surveys1997` and print the plot number to screen.


<br>


