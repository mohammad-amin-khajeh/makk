+++
title = "pythonic syntactic sugar, no BS"
description = "Some nice, quality of python syntax because why not"
date = "2025-03-22"
[extra]
toc = true
[taxonomies]
tags = ['programming_languages', 'python']
categories = ['tutorial']
+++

Python's syntax is so polarizing.

On one end, C family folks argue that it's alienating and weird.

And on the other end, there are people embracing it wholeheartedly.

I'm here to argue no matter what your stance is, python has a lot of syntactic sugar which makes it really nice and intuitive for dealing with common boilerplate.

## syntactic sugar: definition

> Syntactic sugar refers to pieces of syntax that simplify the code and make it more readable or concise.
>
> — [realpython.com](realpython.com)

## unpacking in a for loop

```python
my_list = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]


# without unpacking
for arr in my_list:
    a = arr[0]
    b = arr[1]
    c = arr[2]
    print(a, b, c)
    # 1 2 3
    # 4 5 6
    # 7 8 9

# with unpacking
for a, b, c in my_list:
    print(a, b, c)
    # 1 2 3
    # 4 5 6
    # 7 8 9
```

Unpacking allows for assignment of each value to a variable upon the for loop initialization.

It can also be used with tuples and dictionary keys and values among other things:

```python
my_dict = {
    1: [1, 2, 3],
    2: [4, 5, 6],
    3: [7, 8, 9],
}

for a, b, c in my_dict.values():
    print(a, b, c)
    # 1 2 3
    # 4 5 6
    # 7 8 9
```

{{admonition(type='note' text='the number of variables must match the number of values.')}}

## list comprehension

Let's accumulate all the odd numbers from 1 to 99 in a list:

```python
odd = []
for i in range(100):
    if i % 2 == 1:
        odd.append(i)
print(odd)
```

Not bad, but certainly we can reduce it further.

First, we can use the truthy and falsey feature of values to determine whether a number is odd or even:

```python
odd = []
for i in range(100):
  # if i % 2 == 1:
    if i % 2:
        odd.append(i)
print(odd)
```

Better, but it's still fundamentally the same code, let's just do it in one line:

```python
odd = [odd for odd in range(100) if odd % 2]
print(odd)
```

Or even better:

```python
odd = [odd for odd in range(1, 100, 2)]
print(odd)
```

Sure, it is language-specific but it is more concise and therefore more readable in my opinion.

{{admonition(type='tip' text='it can even be made shorter! Keep reading or skip to ["merging arrays using the unpacking operator"](#merging-arrays-using-the-unpacking-operator) ')}}

## dictionary comprehension

Before we learn about dictionary comprehension(which is pretty similar to list comprehension), we need to learn about the `zip` function.

### zip

`zip` allows us to loop over multiple iterables at the same time.

Let's see an example:

```python
first_row   = [1, 2, 3]
second_row  = [4, 5, 6]
third_row   = [7, 8, 9]

# without zip
for a, b, c in (first_row, second_row, third_row):
    print(a, b, c)
    # 1, 2, 3
    # 4, 5, 6
    # 7, 8, 9

# with zip
for a, b, c in zip(first_row, second_row, third_row):
    print(a, b, c)
    # 1, 4 ,7
    # 2, 5 ,8
    # 3, 6 ,9
```

Notice how in the zip example we iterated vertically from the top left corner instead of the typical row-wise.

Simply put, `zip` allows us to assign `n` variables to `n` iterables and iterate over them simultaneously.

Now back to dictionary comprehension:

```python

rows = [1, 2, 3], [4, 5, 6], [7, 8, 9]
row_titles = ["first_row", "second_row", "third_row"]

my_dict = {k: v for k, v in zip(row_titles, rows)}
print(my_dict)
# {'first_row': [1, 2, 3], 'second_row': [4, 5, 6], 'third_row': [7, 8, 9]}
```

We're looping over the titles and rows themselves simultaneously using `zip`,
in each iteration we use the current row title as the `key` of our entry and the current row itself as the `value`.

_"what if we have more keys than values?(or vice versa)"_\
I hear you. The zip function stops as one of the iterables reaches the end and no error is thrown(so you need to be careful with how you use it).

## merging arrays using the unpacking operator

Instead of lengthy loops we can merge two arrays into a new array in one line!

``` python
# first array
yellow_fruit = ["banana", "lemon", "sweet corn"]

# second array
red_fruit = ["cherry", "strawberry", "watermelon"]

# deprecated pre python 3.5 fashion
# yellow_and_red_fruit = yellow_fruit + red_fruit

# python 3.5+ syntax with the unpacking operator
yellow_and_red_fruit = [*yellow_fruit, *red_fruit]

print(yellow_and_red_fruit)
# ['banana', 'lemon', 'sweet corn', 'cherry', 'strawberry', 'watermelon']
```

Also, remember the odd numbers array from before? We can further simplify it!

```python
odd = [*range(1,100,2)]
print(odd)
```

### "Extending" an array

Additionally, one can also extend an array. That is, append one array to another array.

```python
# first array
yellow_fruit = ["banana", "lemon", "sweet corn"]

# second array
yellow_fruit_more = ["mango", "pineapple", "yellow apple"]

yellow_fruit.extend(yellow_fruit_more)

print(yellow_fruit)
# ['banana', 'lemon', 'sweet corn', 'mango', 'pineapple', 'yellow apple']
```

## merge dictionaries with the dictionary unpacking operator

Just like we can merge arrays, we can also merge dictionaries.

```python
summer_fruit = {
    "cherry": "red",
    "watermelon": "red",
    "blueberries": "blue",
}

winter_fruit = {
    "Pomegranate": "red",
    "orange": "orange",
    "kiwi": "green",
}

fruits = {**summer_fruit, **winter_fruit}
print(fruits) # {'cherry': 'red', 'watermelon': 'red', 'blueberries': 'blue', 'Pomegranate': 'red', 'orange': 'orange', 'kiwi': 'green'}
```

## summary

Hopefully you can see now why python is such a nice language for coding interviews, scripts, etc.

If I missed anything or there's something cool you that you feel belongs here make sure to dm me on my telegram or github(or Email me, whatever suits you:) )
