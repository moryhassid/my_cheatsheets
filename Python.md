- [Python](#python)
  - [Python's Data types:](#pythons-data-types)
  - [Casting](#casting)
    - [Integer to String](#integer-to-string)
    - [String to Integer](#string-to-integer)
  - [Python List](#python-list)
  - [Python Sets](#python-sets)
  - [Slicing](#slicing)
    - [Skipping the n characters](#skipping-the-n-characters)
    - [Removing the last n characters](#removing-the-last-n-characters)
    - [Get the first n characters](#get-the-first-n-characters)
    - [Reverse the string](#reverse-the-string)
  - [Function](#function)

# Python 

## Python's Data types:

* int
* string
* boolean- (True or False)

## Casting

### Integer to String

Example:

```python
num = 156418
my_new_str = str(num)
```

### String to Integer

Example:

```python
my_new_str = '156418'
num = int(my_new_str)
```

## Python List

* A Python list is an ordered and changeable collection of data objects. Unlike an array, which can contain objects of a single type, a list can contain a mixture of objects.

 * A list can contain objects of any data type, including another list. This makes the list data structure one of the most powerful and flexible tools in Pythonic programming.
Defining a list in Python is really quite simple. The name of the list has to be specified, followed by the values it contains:

``` python
# Initiliazing a list with 5 objects
myList1 = [20, 'Edpresso', [1, 2, 3], 3.142, None]
print(myList1)

# Initializing an empty list
myList2 = []
print(myList2)
```

## Python Sets

A set is a collection of unique data. That is, elements of a set cannot be duplicate. For example,
Suppose we want to store information about student IDs. Since student IDs cannot be duplicate, we can use a set.
In Python, we create sets by placing all the elements inside curly braces {}, separated by comma.
A set can have any number of items and they may be of different types (integer, float, tuple, string etc.). But a set cannot have mutable elements like lists, sets or dictionaries as its elements.
Let's see an example,

``` python
# create a set of integer type
student_id = {112, 114, 116, 118, 115}
print('Student ID:', student_id)

# create a set of string type
vowel_letters = {'a', 'e', 'i', 'o', 'u'}
print('Vowel Letters:', vowel_letters)

# create a set of mixed data types
mixed_set = {'Hello', 101, -2, 'Bye'}
print('Set of mixed data types:', mixed_set)
```

## Slicing


### Skipping the n characters

```Python
my_name = 'Mory Hassid'
print(my_name[5:])
```

### Removing the last n characters

Reminder: -1 means the last character

```Python
my_name = 'Mory Hassid'
print(my_name[:-7])
# output would be Mory
```

### Get the first n characters

```Python
my_name = 'Mory Hassid'
print(my_name[:4])
# output would be Mory
```

### Reverse the string

```Python
my_name = 'Mory Hassid'
print(my_name[::-1])
# output would be "dissaH yroM"
```


## Function

An example for function:

```Python
def print_triangles(size):
    for row_idx in range(1, size + 1):
        print(row_idx * 'X')
```


```Python
print_triangles(size = 15)
```