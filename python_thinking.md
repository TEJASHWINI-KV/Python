# Python Best Practices & Pythonic Code - Complete Guide

**Author:** Beginner's Learning Path  
**Last Updated:** February 19, 2026  
**Purpose:** Quick reference guide for writing Pythonic, clean, and maintainable Python code

---

# TABLE OF CONTENTS
1. [Writing Pythonic Code (PEP8)](#1-writing-pythonic-code-pep8)
2. [Naming Conventions](#2-naming-conventions)
3. [Using Docstrings](#3-using-docstrings)
4. [Pythonic Control Structures](#4-pythonic-control-structures)
5. [Raising Exceptions](#5-raising-exceptions)

---

# 1. WRITING PYTHONIC CODE (PEP8)

## What is PEP8?
- Python's official style guide for writing code
- Makes code readable and consistent
- Everyone on a team writes similarly

## Key Principles

### Readability Over Cleverness
- Write code that anyone can understand
- Don't write one-liners just to impress people
- Complex logic = Use loops (not one-liners)

### String Concatenation

**❌ BAD (memory inefficient):**
```python
name = "John"
last = "Doe"
full = name + " " + last
```

**✅ GOOD (fast, efficient):**
```python
name = "John"
last = "Doe"
full = " ".join([name, last])
```

**Why?** Python strings are immutable. `join()` allocates memory ONCE. `+` allocates new memory for EACH concatenation.

---

# 2. NAMING CONVENTIONS

## Variables and Functions
- Use **lowercase** with **underscores** separating words
- Be SPECIFIC, not vague

```python
# ❌ WRONG (too vague)
def get_user_info(id):
    return user

# ✅ RIGHT (specific and clear)
def get_user_by(user_id):
    return user
```

## Classes
- Use **CamelCase** (first letter capital)

```python
class UserProfile:
    pass

class StudentInformation:
    pass
```

## Constants
- Use **ALL CAPS** with underscores

```python
MAX_STUDENTS = 30
TIMEOUT = 60
DATABASE_URL = "localhost:5432"
```

## Private Variables/Methods
- Use **single underscore** `_variable` for internal use
- Use **double underscore** `__variable` to prevent name mangling

```python
class Student:
    _student_id = 123  # Internal, not meant to be accessed
    __secret = "hidden"  # Prevent name mangling
```

## Memory Trick: NAMING RULE
```
Variables & Functions → lowercase_with_underscore
Classes → CamelCase
Constants → ALL_CAPS_WITH_UNDERSCORE
Private → _single_or__double_underscore
```

---

# 3. USING DOCSTRINGS

## What Are Docstrings?
- Documentation written INSIDE your code
- Triple quotes: `"""`
- Become the `__doc__` attribute

## One-Line Docstring
```python
def add(a, b):
    """Add two numbers and return the result."""
    return a + b
```

**Rules:**
- Use triple quotes (even for one line)
- End with a period
- Be brief

## Multi-Line Docstring
```python
def calculate_grade(score):
    """Calculate student grade based on score.
    
    Takes a test score and returns the letter grade.
    Scores 90+ = A, 80+ = B, 70+ = C, below 70 = F.
    
    :param score: The student's test score.
    :type score: int
    :return: The letter grade as a string.
    :rtype: str
    """
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    # ... more code
```

## Three Places to Write Docstrings

### 1. Module Level (Top of file)
```python
"""
This module handles all database operations.
Provides functions to create, read, update, delete records.

Exceptions:
    DatabaseError: When database connection fails
    RecordNotFound: When requested record doesn't exist
"""

import sqlite3
```

### 2. Class Docstring
```python
class Student:
    """Represents a student and stores their information.
    
    This class manages student data including name, age, and grades.
    
    Usage:
    >>> student = Student("Alice", 15)
    >>> student.get_name()
    'Alice'
    """
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
```

### 3. Function Docstring
```python
def is_prime_number(number):
    """Check if a number is prime.
    
    A prime number is only divisible by 1 and itself.
    This function checks all numbers up to the square root.
    
    :param number: The number to check.
    :type number: int
    :return: True if prime, False if not.
    :rtype: boolean
    """
    # code here
```

## Docstring Styles (Pick ONE for your project)

### Google Style
```python
def get_weather(city):
    """Get weather information for a city.
    
    Parameters:
        city (str): Name of the city.
    
    Returns:
        dict: Weather data for the city.
    """
```

### Restructured Text (Official)
```python
def get_weather(city):
    """Get weather information for a city.
    
    :param city: Name of the city.
    :type city: str
    :return: Weather data for the city.
    :rtype: dict
    """
```

### NumPy Style
```python
def get_weather(city):
    """Get weather information for a city.
    
    Parameters
    ----------
    city : str
        Name of the city.
    
    Returns
    -------
    dict
        Weather data for the city.
    """
```

## Modern Python with Type Hints
```python
def get_weather(city: str) -> dict:
    """Get weather information for a city."""
    # Types are clear from function definition!
```

## Memory Trick: DOCSTRING CHECKLIST
```
✓ Start with triple quotes """
✓ First line = brief description
✓ Blank line before details
✓ End with period
✓ Include :param, :return (if not using type hints)
✓ Use same style throughout project
```

---

# 4. PYTHONIC CONTROL STRUCTURES

## 4.1 LIST COMPREHENSIONS

### What It Is
- Quick way to build lists using a single line
- Much faster than `append()` in loops

### Pattern
```
[WHAT_TO_DO for ITEM in LIST if CONDITION]
```

### Examples

**Example 1: Square numbers**
```python
numbers = [1, 2, 3, 4, 5]
squares = [n * n for n in numbers]
# Result: [1, 4, 9, 16, 25]
```

**Example 2: Filter even numbers**
```python
numbers = [1, 2, 3, 4, 5, 6]
evens = [n for n in numbers if n % 2 == 0]
# Result: [2, 4, 6]
```

**Example 3: Uppercase strings**
```python
words = ["hello", "world"]
upper = [word.upper() for word in words]
# Result: ['HELLO', 'WORLD']
```

### When NOT to Use List Comprehension

**❌ Too complex (use loop instead):**
```python
ages = [1, 34, 5, 7, 3, 57, 356]
old = [age for age in ages if age > 10 and age < 100 and age is not None]
```

**✅ Better (clear loop):**
```python
ages = [1, 34, 5, 7, 3, 57, 356]
old = []
for age in ages:
    if age > 10 and age < 100:
        old.append(age)
```

## Memory Trick: LIST COMPREHENSION RULE
```
Simple operation? → List Comprehension
Multiple conditions? → Use a loop
More than 2 loops? → Use a loop
```

---

## 4.2 LAMBDA FUNCTIONS

### What It Is
- Quick, one-line anonymous function
- Used ONLY inside other functions
- Pattern: `lambda x: what_to_do`

### Good Use: Inside Other Functions

```python
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x * x, numbers))
# Result: [1, 4, 9, 16, 25]
```

**With sorted():**
```python
words = ["python", "java", "c"]
sorted_by_length = sorted(words, key=lambda word: len(word))
# Result: ['c', 'java', 'python']
```

**With min/max:**
```python
lists = [[5, 10], [1, 2], [8, 3]]
smallest = min(lists, key=lambda x: x[1])
# Result: [1, 2] (because 2 is smallest in second position)
```

### Bad Use: DON'T Assign to Variable

❌ **WRONG:**
```python
square = lambda x: x * x
```

✅ **RIGHT:**
```python
def square(x):
    return x * x
```

**Why?** If you name it, use `def`. Lambda is for temporary use only.

## Understanding Index in Lambda

```python
lists = [[5, 10], [1, 2], [8, 3]]
smallest = min(lists, key=lambda x: x[0])
# x[0] = first element: 5, 1, 8
# smallest = [1, 2]

smallest = min(lists, key=lambda x: x[1])
# x[1] = second element: 10, 2, 3
# smallest = [1, 2]
```

## Memory Trick: LAMBDA RULE
```
Lambda in expression? → Good
Lambda assigned to variable? → Bad (use def instead)
Temporary, one-time use? → Good
Reusable function? → Use def
```

---

## 4.3 GENERATORS vs LIST COMPREHENSION

### List Comprehension
- Stores **ALL items in memory** at once
- Like printing all 1000 pages at once

```python
with open("huge_file.txt") as f:
    lines = [line for line in f]  # All lines in memory!
```

### Generator
- Creates items **one at a time**
- Like reading one page at a time

```python
def read_file(file_name):
    with open(file_name) as f:
        for line in f:
            yield line  # One line at a time

for line in read_file("huge_file.txt"):
    print(line)
```

### When to Use Each

**Use List Comprehension:**
- Small data sets
- Need to access multiple times
- Need list methods (`.sort()`, `.reverse()`)

**Use Generator:**
- HUGE data (files, databases)
- One-pass reading only
- Memory matters

## Example: Create Range Efficiently

```python
# List (stores all 1 million)
numbers = [x for x in range(1000000)]

# Generator (creates as needed)
numbers = (x for x in range(1000000))

for num in numbers:
    print(num)  # Only one in memory at a time
```

## Memory Trick: GENERATOR vs LIST
```
Small data, multiple uses? → List Comprehension
Huge data, one pass? → Generator
Memory critical? → Generator
```

---

## 4.4 USE range() INSTEAD OF LISTS

### Why range() Is Better
- Doesn't store numbers in memory
- Only stores start, stop, step
- Much faster

### Examples

```python
# Count 0 to 9
range(10)  # 0, 1, 2, 3, 4, 5, 6, 7, 8, 9

# Count 2 to 8
range(2, 8)  # 2, 3, 4, 5, 6, 7

# Count by 2s
range(0, 10, 2)  # 0, 2, 4, 6, 8

# Count backwards
range(10, 0, -1)  # 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
```

### Comparison

❌ **BAD (slow):**
```python
for i in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    print(i)
```

✅ **GOOD (fast):**
```python
for i in range(10):
    print(i)
```

---

## 4.5 AVOID else WITH LOOPS

### The Problem
```python
for item in [1, 2, 3]:
    print("In loop")
else:
    print("Else clause")  # Runs when loop FINISHES normally

# Output:
# In loop
# In loop
# In loop
# Else clause  ← Confusing! Looks wrong!
```

The `else` runs when loop finishes normally (no `break`). **This confuses everyone.**

### Better: Use a Flag

```python
found = False
for item in [1, 2, 3, 4, 5]:
    if item == 3:
        found = True
        print("Found 3!")
        break

if found:
    print("Successfully found it!")
else:
    print("Not found!")
```

**Much clearer!**

## Memory Trick: AVOID LOOP else
```
Loop with else clause? → AVOID (use flag instead)
Want to run code after loop? → Use flag or separate if
```

---

## 4.6 COMPARISON TABLE

| Feature | Use When | Example |
|---------|----------|---------|
| List Comprehension | Simple list creation | `[x*2 for x in nums]` |
| Lambda | Quick function in expression | `sorted(list, key=lambda x: x[1])` |
| Generator | Huge data, memory matters | `yield item` |
| range() | Counting numbers | `range(10)` |
| Loop | Complex logic | Multiple if/elif |

---

# 5. RAISING EXCEPTIONS

## What Is an Exception?
- A message that tells the user "something went wrong"
- Python stops execution and shows error message
- Better than silently breaking

```python
10 / 0
# ZeroDivisionError: division by zero
```

## The Try-Except Pattern

```python
try:
    # Code that might fail
except SpecificError:
    # Handle the error
finally:
    # Clean up (optional)
```

---

## 5.1 RAISE AN EXCEPTION WHEN SOMETHING FAILS

### Example 1: Validation
```python
def divide(a, b):
    """Divide two numbers."""
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

divide(10, 0)
# ValueError: Cannot divide by zero
```

### Example 2: Database Lookup
```python
def get_user(user_id):
    """Get user from database."""
    user = db.find_user(user_id)
    if user is None:
        raise ValueError(f"User {user_id} not found")
    return user
```

### Key Rule: DON'T Return None

❌ **WRONG (hides error):**
```python
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return None  # Later, you wonder why it's None
```

✅ **RIGHT (tells the truth):**
```python
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        raise ValueError("Cannot divide by zero")
```

---

## 5.2 CATCH SPECIFIC EXCEPTIONS ONLY

### Bad: Bare except (Hides Bugs)

```python
❌ WRONG:
try:
    result = int("abc")
except:  # Catches EVERYTHING
    print("Error")
# You don't know what failed!
```

### Good: Specific Exceptions

```python
✅ RIGHT:
try:
    result = int("abc")
except ValueError:
    print("Cannot convert to integer")
except TypeError:
    print("Wrong type")
# Now you know EXACTLY what failed
```

### Multiple Exception Types

```python
try:
    data = process_data()
except (TypeError, ValueError) as e:
    print(f"Error: {e}")
except IndexError:
    print("Index out of range")
```

---

## 5.3 USE finally TO CLEAN UP RESOURCES

`finally` always runs, no matter what.

### Example 1: Close File

```python
def read_file(filename):
    file = open(filename)
    try:
        content = file.read()
        return content
    finally:
        file.close()  # Runs EVEN if error occurs
```

### Example 2: Close Database Connection

```python
def send_email(host, port, user, password, message):
    server = smtplib.SMTP(host=host, port=port)
    try:
        server.login(user, password)
        server.send_email(message)
    finally:
        server.quit()  # Always closes, even if login fails
```

### Flow Diagram

```
try:
    Something goes wrong
    ↓
except:
    Error caught here
    ↓
finally:
    Runs no matter what
    ↓
Code continues
```

---

## 5.4 CREATE CUSTOM EXCEPTION CLASSES

When you need a **specific error name** for your application.

### Pattern

```python
class YourErrorName(Exception):
    """Error description."""
    def __init__(self, message=None, errors=None):
        super().__init__(message)
        self.errors = errors
```

### Example 1: User Not Found

```python
class UserNotFoundError(Exception):
    """Raised when user doesn't exist."""
    def __init__(self, message=None, errors=None):
        super().__init__(message)
        self.errors = errors

def get_user(user_id):
    user = database.find(user_id)
    if not user:
        raise UserNotFoundError(f"User ID {user_id} does not exist")
    return user

get_user(999)
# UserNotFoundError: User ID 999 does not exist
```

### Example 2: Validation Error

```python
class ValidationError(Exception):
    """Raised when validation fails."""
    def __init__(self, message=None, errors=None):
        super().__init__(message)
        self.errors = errors

def validate_email(email):
    if "@" not in email:
        raise ValidationError(f"Invalid email: {email}")
    return True
```

**Why custom exceptions?**
- Clear error name tells what failed
- Easy to catch in caller code
- Better for debugging

---

## 5.5 KEEP try BLOCK MINIMAL

Only wrap code that can actually fail.

### Bad (Too Much Code)

```python
❌ WRONG:
def write_file(filename, data):
    try:
        file = open(filename, "w")
        file.write(data)
        file.close()
        print("Success")  # Can't fail
        return True  # Can't fail
    except FileNotFoundError:
        print("Error")
```

### Good (Only Risky Code)

```python
✅ RIGHT:
def write_file(filename, data):
    try:
        file = open(filename, "w")
        file.write(data)
    except FileNotFoundError:
        print("Error")
    finally:
        file.close()
    
    print("Success")  # Outside try
    return True  # Outside try
```

**Why?** Makes it clear which lines can fail.

---

## 5.6 EXCEPTION HANDLING FLOW EXAMPLE

```python
try:
    data = get_data_from_db()
    print(data)  # Line A
except ConnectionError:
    print("Database error")  # Line B
except KeyError:
    print("Key not found")  # Line C
finally:
    close_connection()  # Line D - ALWAYS runs

print("Done")  # Line E
```

**Scenarios:**

1. **Success:** A → D → E
2. **ConnectionError:** B → D → E
3. **KeyError:** C → D → E

---

## 5.7 HANDLE THIRD-PARTY LIBRARY EXCEPTIONS

Learn what exceptions a library throws.

### Example: AWS boto3

```python
from botocore.exceptions import ClientError

try:
    ec2.describe_instances(InstanceIds=['i-badid'])
except ClientError as e:
    error_code = e.response['Error']['Code']
    if error_code == 'InvalidInstanceID.NotFound':
        raise CustomError("Instance not found")
```

---

## Memory Trick: EXCEPTION RULE

```
Function can fail? → Raise specific exception
Catch specific errors? → Not bare except
Need cleanup? → Use finally
Want custom error? → Create exception class
Many lines in try? → Keep it minimal
```

---

# MASTER CHEAT SHEET

## Naming
```
variables_and_functions → lowercase_with_underscore
Classes → CamelCase
CONSTANTS → ALL_CAPS_WITH_UNDERSCORE
```

## Docstrings
```
def func():
    """Brief description.
    
    Detailed explanation.
    
    :param name: Description
    :return: What it returns
    """
```

## List Comprehension
```
[WHAT for ITEM in LIST if CONDITION]
```

## Lambda
```
sorted(items, key=lambda x: x[value])
```

## Generator
```
def generator():
    yield item
```

## Range
```
range(10) → 0 to 9
range(2, 8) → 2 to 7
range(0, 10, 2) → 0, 2, 4, 6, 8
```

## Exceptions
```
try:
    code_that_might_fail()
except SpecificError:
    handle_error()
finally:
    cleanup()
```

---

# QUICK DECISION TREES

## Should I Use List Comprehension?
```
Simple operation? → YES
Multiple conditions? → NO (use loop)
More than 2 loops? → NO (use loop)
Readable in one line? → YES
```

## Should I Use Lambda?
```
Inside sorted/min/max? → YES
Assigning to variable? → NO (use def)
One-time use? → YES
Reusable function? → NO (use def)
```

## Should I Use Generator?
```
Small data? → NO (use list)
Huge data? → YES
One-pass reading? → YES
Multiple accesses? → NO (use list)
Memory critical? → YES
```

## Should I Raise Exception?
```
Something can fail? → YES
Function has preconditions? → YES
Caller needs to know error? → YES
Can handle silently? → Maybe return None
```

---

# KEY TAKEAWAYS

1. **Readability > Cleverness** → Write code others understand
2. **Be Specific** → Specific names, specific exceptions
3. **Raise Exceptions** → Tell the caller when something fails
4. **Catch Specific Errors** → Don't use bare `except:`
5. **Clean Up Resources** → Use `finally` for cleanup
6. **Document Code** → Write docstrings
7. **Follow PEP8** → Consistent style
8. **Use Right Tool** → List comprehension, lambda, generator for their purposes

---

# PRACTICE TEMPLATE

Copy this template and practice:

```python
"""Module docstring here."""

class MyClass:
    """Class docstring here."""
    
    MAX_SIZE = 100  # Constant
    
    def my_method(self, param1, param2):
        """Method docstring with details.
        
        :param param1: Description
        :param param2: Description
        :return: What it returns
        """
        try:
            # Code that might fail
            if param1 is None:
                raise ValueError("param1 cannot be None")
            
            # Process
            result = [x*2 for x in param1 if x > 0]
            
            return result
            
        except ValueError as e:
            print(f"Validation error: {e}")
            raise
        finally:
            # Cleanup if needed
            pass
```

---

**Remember:** Master these concepts and you'll write professional, maintainable Python code! 🚀

