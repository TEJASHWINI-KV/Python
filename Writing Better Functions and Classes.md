# Python Module Notes (Functions + Classes)

> Brief, practical notes for future reference. Includes memory tricks, analogies, and runnable examples.

---

## 1) Functions (Best Practices)

### What it is
Guidelines to write clean, safe, and maintainable Python functions.

### Why it matters
Prevents hidden bugs, improves readability, saves memory, and makes code easier to test.

### Key rules (brief)
- **Small functions:** one main responsibility.
- **Use generators:** handle large data efficiently.
- **Raise exceptions:** don’t return `None` for errors.
- **Keyword args:** improve readability for many parameters.
- **Don’t return `None` explicitly** when a real value is expected.
- **Be defensive:** logging + tests.
- **Avoid overusing lambdas** unless trivial and clear.

### Memory tricks
- **One task per function**
- **Big data → `yield`**
- **Bad input → raise**
- **Many args → keyword**
- **Empty result → empty list, not `None`**

### Analogies (short)
- Small functions are like “single‑purpose tools.”
- Generators are like “tap water” (flowing, not stored in a tank).

### Common mistakes
- Mixing multiple tasks in one function.
- Returning `None` instead of raising errors.
- Loading entire huge files into lists.
- Using complex lambdas that hide logic.

### Runnable examples
**Example 1: Generator for large files**
```python
import re

def read_lines(path):
    with open(path, "r") as f:
        for line in f:
            yield line

def get_unique_emails(path):
    emails = set()
    for line in read_lines(path):
        for email in re.findall(r"[\w\.-]+@[\w\.-]+", line):
            emails.add(email)
    return emails
```

**Example 2: Raise exceptions**
```python
def read_lines_for_python(file_name, file_type):
    if file_type not in ("txt", "html"):
        raise ValueError("Not correct file format")
    if not file_name:
        raise FileNotFoundError("File not found")

    with open(file_name, "r") as f:
        for line in f:
            if "python" in line:
                return "Found Python"
    return "Not Found"
```

**Example 3: Keyword arguments**
```python
def spam_emails(from_addr, to_addr, subject, size, sender_name, receiver_name):
    return f"{sender_name} -> {receiver_name}: {subject} ({size})"

print(spam_emails(
    from_addr="ab_from@gmail.com",
    to_addr="nb_to@yahoo.com",
    subject="Is email spam",
    size=10000,
    sender_name="ab",
    receiver_name="nb"
))
```

---

## 2) Classes (Best Practices)

### What it is
Guidelines for structuring classes and using OOP features correctly (SRP, properties, static/class methods, abstract classes).

### Why it matters
Prevents bloated classes, improves reuse, and reduces bugs in large systems.

### Key rules (brief)
- **SRP:** one class = one responsibility.
- **Structure order:** class vars → `__init__` → special methods → class methods → static methods → properties → instance methods → private methods.
- **`@property`:** use for getters/setters and validation.
- **`@staticmethod`:** utility logic related to class but no state.
- **`@classmethod`:** alternative constructors/factories.
- **Use `abc`** for proper abstract classes.
- **Prefer public attributes** unless name‑mangling is needed.

### Memory tricks
- **SRP = one class, one job**
- **Property = getter/setter**
- **Static = no state**
- **Classmethod = alternate constructor**
- **ABC = enforce required methods**

### Analogies (short)
- SRP classes are like “single‑purpose containers.”
- `@classmethod` is like “alternate entry door.”

### Common mistakes
- Huge classes with unrelated responsibilities.
- `@property` that **modifies** state instead of returning.
- Using static methods that really need `self` or `cls`.
- Using `NotImplementedError` instead of `abc`.

### Runnable examples
**Example 1: Class structure + SRP**
```python
class UserInfo:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def display(self):
        return f"{self.name} <{self.email}>"
```

**Example 2: Proper `@property` getter/setter**
```python
class Temperature:
    def __init__(self, celsius=0):
        self.celsius = celsius

    @property
    def fahrenheit(self):
        return (self.celsius * 1.8) + 32

    @fahrenheit.setter
    def fahrenheit(self, temp_f):
        if not isinstance(temp_f, (int, float)):
            raise ValueError("Must be number")
        self.celsius = (temp_f - 32) / 1.8

# demo
x = Temperature(0)
print(x.fahrenheit)
x.fahrenheit = 212
print(x.celsius)
```

**Example 3: `@staticmethod` utility**
```python
class BookPriceCalculator:
    PER_PAGE_PRICE = 8

    def __init__(self, pages):
        self.pages = pages

    @property
    def standard_price(self):
        return self.pages * self.PER_PAGE_PRICE

    @staticmethod
    def price_to_book_ratio(market_price, book_value):
        return market_price / book_value

# demo
b = BookPriceCalculator(100)
print(b.standard_price)
print(BookPriceCalculator.price_to_book_ratio(120, 80))
```

**Example 4: `@classmethod` factory**
```python
class User:
    def __init__(self, first, last):
        self.first = first
        self.last = last

    @classmethod
    def from_string(cls, name):
        first, last = name.split()
        return cls(first, last)

u = User.from_string("Larry Page")
print(u.first, u.last)
```

**Example 5: Proper abstract class**
```python
from abc import ABC, abstractmethod

class Fruit(ABC):
    @abstractmethod
    def taste(self):
        pass

    @abstractmethod
    def originated(self):
        pass

class Apple(Fruit):
    def taste(self):
        return "sweet"

    def originated(self):
        return "Central Asia"

# demo
# Fruit()  # would raise TypeError
print(Apple().taste())
```

---

## Quick Checklist (Functions + Classes)
- One responsibility per function/class
- Use `yield` for large data
- Raise exceptions on bad input
- Prefer keyword args for clarity
- Use `@property` for computed/validated access
- Use `@staticmethod` for class‑related utilities
- Use `@classmethod` for alternate constructors
- Use `abc` to enforce required methods

---

## Interview Quick‑fire (Sample Questions)
1. Why prefer generators over lists for large data?
2. When should you raise an exception instead of returning `None`?
3. What is SRP and how does it apply to classes?
4. Difference between `@staticmethod` and `@classmethod`?
5. When is `@property` appropriate?
6. How does `abc` prevent incomplete classes?
