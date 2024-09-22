# Understanding Modules and Packages in Python: A Quick Guide

When developing in Python, you’ve probably come across terms like *modules* and *packages*. They are essential concepts in Python’s structure, helping to organize and reuse code efficiently. However, distinguishing between these two and understanding their roles can be confusing, especially when executing them using different commands like `python package/module.py` versus `python -m package.module`. In this blog post, we’ll break down what Python modules and packages are and explore the key differences between these execution methods.

---

## What is a Module in Python?

A **module** is simply a Python file (`.py`) that contains Python code (functions, classes, or variables) you can import and use in other Python programs.

### Example:

Let’s create a simple module called `math_operations.py`:

```python
# math_operations.py

def add(a, b):
    return a + b

def subtract(a, b):
    return a - b
```
You can then use this module in other Python scripts by importing it:
``` python
# main.py
import math_operations

result = math_operations.add(5, 3)
print(result)  # Output: 8
```
Modules are a great way to group related functions and classes together in a single file for better code organization and reuse.

----
# What is a Package in Python?
A package is a directory containing multiple Python modules (files), along with a special `__init__.py` file, which tells Python that the directory should be treated as a package.

Packages allow you to structure your project in a more organized way, especially for large-scale applications. They can include sub-packages, meaning packages within packages, making it possible to build complex applications in a modular fashion.

Example:
Let’s say you have a package structure for a library:
```
my_project/
│
├── math_operations/
│   ├── __init__.py  # Makes 'math_operations' a package
│   ├── basic.py     # Module
│   └── advanced.py  # Module
│
└── main.py
```

Now, you can import different modules within this package:

```
# main.py
from math_operations.basic import add
from math_operations.advanced import square

result = add(5, 3)
square_result = square(4)

print(result)          # Output: 8
print(square_result)   # Output: 16
```
----
# Distinguishing Between python package/module.py and python -m package.module
Both these methods involve executing Python code from modules, but there are critical differences in how they work.

```bash
python package/module.py
```
This is a straightforward way of running a Python script. You specify the path to a Python file and execute it. For example:
``` bash
python package/module.py
```

Toy example:
```
my_project/
│
├── aaa/
│   ├── a.py
└── bbb/
│   ├── b.py
```
We have 2 packages `aaa` and `bbb`. In each package contains one module `a.py` and `b.py`.
This is a script in `a.py`
```python
hello = "hello world"
print(hello)
```
`b.py`
```python
from aaa.a import hello

print(hello)
```
In `a.py`, I defined a variable `hello` and in `b.py` I want to import the `hello` variable to `b.py`.
If I run the bash
``` bash
python bbb/b.py
```
I have an error `ModuleNotFoundError: No module named 'aaa'` because the searching path in python only contain the path of `bbb` folder. How can I check it?
It's so easy, you can import `sys` library and print the `sys.path`:

```python
import sys
print(sys.path)
```
First element is the path of `bbb` package, the others is path of built-in and installed packages. You can see that, `aaa` path doesn't lie in the `sys.path`. That is the reason why python cannot find the module you want to import.
Please try the bash
```bash
python -m bbb.b
```
It works. Great. Why? Please check the `sys.path`, it contains the parent path of `bbb` (this is `my_project` path) and `aaa` also lies in this, so Python can find the module `a` in `aaa` package. 

# Homework
- Create a new package `ccc` which lies in `bbb`, after that you define a module named `c.py` and import `hello` variable in `a.py`. What happen? 
- The exercie help you understand the difference between `python package/module.py` and `python -m package.module`
