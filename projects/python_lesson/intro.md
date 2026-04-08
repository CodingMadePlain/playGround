# Python Variables

**TL;DR:** Variables store data so you can use it later. You create a variable by giving it a name and assigning a value with the `=` sign.

---

## What Is a Variable?

A variable is a label that points to a piece of data stored in your computer's memory. Think of it as a named container. You put something in, give it a name, and retrieve it whenever you need it.

```python
message = "Hello, World!"
print(message)
```

**Output:**
```
Hello, World!
```

The `=` sign is called the **assignment operator**. It takes the value on the right and stores it in the name on the left.

### Assignment 1

Create a variable called `greeting` that stores the text `"Welcome to Python"`. Print it to the screen.

---

## Naming Rules

Python has strict rules about what you can name a variable.

- A variable name must start with a letter or an underscore.
- It can contain letters, numbers, and underscores.
- It cannot contain spaces.
- You cannot use Python **keywords** (reserved words such as `if`, `for`, `class`, and `return`).
- Python is **case sensitive**, so `Name`, `name`, and `NAME` are three different variables.

```python
# Valid names
user_name = "Alex"
age2 = 30
_score = 95

# Invalid names
# 2nd_place = "Silver"   → starts with a number
# my name = "Alex"       → contains a space
# class = "Beginner"     → uses a reserved keyword
```

```python
name = "Alex"
Name = "Jordan"

print(name)
print(Name)
```

**Output:**
```
Alex
Jordan
```

### Naming Conventions

The Python community follows a style guide called **PEP 8**. For variables, the convention is to use **snake_case**, which means all lowercase letters with words separated by underscores.

```python
# Recommended (snake_case)
first_name = "Alex"
total_score = 100

# Not recommended
firstName = "Alex"    # This style is called camelCase
TotalScore = 100
```

### Assignment 2

Create three variables using proper snake_case naming. Store your first name, your age, and your favourite colour. Print all three.

---

## Data Types

Every value in Python has a **data type**. A data type tells Python what kind of data a variable holds. You do not need to declare the type yourself. Python figures it out automatically. This is called **dynamic typing**.

The four types you will use most often are listed below.

```python
name = "Alex"         # str (string) → text
age = 30              # int (integer) → whole number
height = 1.75         # float → decimal number
is_student = True     # bool (boolean) → True or False
```

You can check the type of any variable using the built in `type()` function.

```python
print(type(name))
print(type(age))
print(type(height))
print(type(is_student))
```

**Output:**
```
<class 'str'>
<class 'int'>
<class 'float'>
<class 'bool'>
```

### Assignment 3

Create one variable of each type (string, integer, float, boolean). Use `type()` to print the data type of each one.

---

## Reassigning Variables

You can change the value of a variable at any time by assigning a new value to it. The old value is replaced.

```python
score = 10
print(score)

score = 25
print(score)
```

**Output:**
```
10
25
```

Because Python uses dynamic typing, you can even change the data type of a variable. This is valid Python, but it can make your code harder to read, so use it with caution.

```python
value = 100
print(type(value))

value = "one hundred"
print(type(value))
```

**Output:**
```
<class 'int'>
<class 'str'>
```

### Assignment 4

Create a variable called `status` and set it to `"loading"`. Print it. Then reassign it to `"complete"` and print it again.

---

## Multiple Assignment

Python lets you assign values to several variables in a single line. This keeps your code short and readable.

```python
x, y, z = 1, 2, 3
print(x)
print(y)
print(z)
```

**Output:**
```
1
2
3
```

You can also assign the same value to multiple variables at once.

```python
a = b = c = 0
print(a, b, c)
```

**Output:**
```
0 0 0
```

### Assignment 5

Using a single line of code, create three variables called `city`, `country`, and `population`. Assign them the values `"London"`, `"UK"`, and `9000000`. Print all three.

---

## Constants

A **constant** is a variable whose value should never change after it is set. Python does not have a built in way to enforce this, so the community uses a naming convention instead. Write the name in all uppercase letters with underscores separating words.

```python
MAX_ATTEMPTS = 5
PI = 3.14159
BASE_URL = "https://api.example.com"
```

This convention signals to other developers (and to your future self) that these values are not meant to be changed.

### Assignment 6

Create two constants. One should store the speed of light (299792458) and the other should store your application name. Print both.

---

## Recap

- **Variables** store data using the assignment operator (`=`).
- **Naming rules** require names to start with a letter or underscore, avoid spaces, and avoid reserved keywords.
- **snake_case** is the standard naming convention from PEP 8.
- **Data types** (string, integer, float, boolean) are assigned automatically through dynamic typing.
- **Reassignment** lets you change a variable's value at any time.
- **Multiple assignment** allows you to set several variables in one line.
- **Constants** use ALL_CAPS naming to signal values that should not change.
- **Functions used in this lesson:** `print()` for displaying output, `type()` for checking a variable's data type.