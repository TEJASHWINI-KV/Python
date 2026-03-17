# Python Lists & Tuples — Course Notes

---

## 1. Course Overview

**Topics Covered:**
- Python `list` — creation, indexing, mutation, slicing
- Strings as lists of characters
- Python `tuple` — creation, immutability, slicing
- The `zip` function
- Built-in functions: `len`, `max`, `min`, `sum`, `sorted`, `all`, `any`

**Prerequisites:** Jupyter or interactive Python environment, basic variables and built-in functions.

---

## 2. Python Lists

### What is a List?
- An **ordered, mutable collection** of elements enclosed in `[]`
- Elements can be of **any data type** (mixed types allowed)
- Preserves insertion order
- **Allows duplicate values**
- Index starts at **0**

```python
empty_list   = []
list_str     = ["Toyota Camry", "Honda Accord"]
list_int     = [10, 20, 30]
list_float   = [1.1, 2.2, 3.3]
list_bool    = [True, False, True]
mixed_list   = ["hello", 42, 3.14, True]
```

---

### Indexing
| Index | Meaning |
|-------|---------|
| `[0]` | First element |
| `[n-1]` | Last element (where `n = len(list)`) |
| `[-1]` | Last element (negative indexing) |
| `[-2]` | Second-to-last element |

- Accessing an out-of-range index raises **`IndexError: list index out of range`**
- `len(my_list)` returns the number of elements

---

### Updating Elements
```python
cars[3] = "Nissan Sentra"   # Update element at index 3
```
> You can only update **existing** index positions with `[]`. To add new elements, use methods.

---

## 3. List Methods & Operations

### Adding Elements
| Method | Description |
|--------|-------------|
| `.append(x)` | Adds **one** element to the end |
| `.insert(i, x)` | Inserts `x` at index `i`; shifts right. If `i` > length, appends to end |
| `.extend([x, y])` | Appends **multiple** elements from another list |
| `list1 + list2` | Returns a **new** concatenated list (originals unchanged) |
| `list += [x, y]` | In-place concatenation (modifies `list`) |

### Removing Elements
| Method | Description |
|--------|-------------|
| `.remove(x)` | Removes **first** occurrence of `x` (exact match required) |
| `.pop()` | Removes and returns the **last** element |
| `.pop(i)` | Removes and returns element at index `i` |
| `.clear()` | Removes **all** elements; list becomes `[]` |
| `del my_list` | **Deletes** the variable entirely from memory |

### Searching & Counting
| Method/Function | Description |
|-----------------|-------------|
| `.index(x)` | Returns index of first occurrence of `x` (case-sensitive; raises error if not found) |
| `.count(x)` | Returns number of occurrences of `x` |
| `x in my_list` | Returns `True`/`False` |

### Ordering
| Method/Function | Description |
|-----------------|-------------|
| `.sort()` | Sorts list **in-place** (ascending by default; lexicographic for strings) |
| `sorted(my_list)` | Returns a **new** sorted list; original unchanged |
| `.reverse()` | Reverses list **in-place** |

### Arithmetic / Replication
```python
list_num *= 2   # Repeats all elements twice (integer multiplier only)
```

---

### Built-in Functions on Lists
```python
max(list_num)    # Maximum value
min(list_num)    # Minimum value
sum(list_num)    # Sum of all elements
len(list_num)    # Number of elements
all(list_num)    # True if ALL elements are truthy (True for empty list)
any(list_num)    # True if ANY element is truthy (False for empty list)
```
> **Note:** `0` is falsy; any non-zero value is truthy.

---

### Copying Lists
| Method | Type | Behavior |
|--------|------|----------|
| `b = a` | **Shallow copy** | Both variables point to the **same** list — changes in one reflect in the other |
| `copy.copy(a)` | **Shallow copy** | New list, but shares references to the same contents |
| `a.copy()` or `copy.deepcopy(a)` | **Deep copy** | Completely independent copy — changes to original do **not** affect the copy |

```python
import copy
new_list = original_list.copy()   # Deep copy
```

---

## 4. List Slicing

### Syntax
```python
my_list[start : end : step]
```
- `start` — inclusive (default: `0`)
- `end` — **exclusive** (default: end of list)
- `step` — increment (default: `1`)
- Slicing **always returns a new (deep) copy**

### Examples
```python
cars[0:4]     # Elements at index 0, 1, 2, 3
cars[:3]      # First 3 elements (start omitted → 0)
cars[3:]      # From index 3 to end (end omitted → last)
cars[:]       # Entire list
cars[0:5:2]   # Every 2nd element from index 0–4 → indices 0, 2, 4
cars[::-1]    # Entire list reversed
cars[4:1:-1]  # Backwards from index 4 down to (not including) index 1
```

### Key Rules
- If range is invalid (start > end with positive step), returns `[]`
- Negative indices count from the end: `[-1]` = last, `[-2]` = second-to-last
- `step` can be negative to traverse in reverse

---

## 5. Strings as Lists of Characters

### Key Concept
A string is an **immutable, ordered sequence of characters** — essentially a read-only list.

```python
x = "World"
x[0]    # 'W'
x[3]    # 'l'
```

### Similarities to Lists
- Indexing with `[]` (0-based)
- Negative indexing
- Slicing with `[start:end:step]`
- `len()` works on strings
- `in` keyword works on strings

### Key Difference: **Immutability**
```python
x[0] = "B"   # ❌ TypeError: 'str' does not support item assignment
```

### Multiple Assignment from String
```python
a, b, c, d, e = "World"   # Each variable gets one character
a, b, _, _, _ = "World"   # Underscore = discard character
```

---

### Useful String Methods

| Method | Description |
|--------|-------------|
| `.upper()` | Returns all-uppercase copy |
| `.lower()` | Returns all-lowercase copy |
| `.startswith(s)` | Boolean — does string start with `s`? |
| `.endswith(s)` | Boolean — does string end with `s`? |
| `.count(s)` | Number of occurrences (case-sensitive) |
| `.find(s)` | Index of first occurrence; returns `-1` if not found |
| `.index(s)` | Index of first occurrence; raises **`ValueError`** if not found |
| `.split(char)` | Splits on `char`; returns a **list** of substrings (default: splits on whitespace) |
| `char.join(list)` | Joins list elements into a string using `char` as separator |

```python
place = "New York City"
place.split(" ")                     # ['New', 'York', 'City']
", ".join(['New', 'York', 'City'])   # 'New, York, City'
```

### User Input
```python
place = input("Where are you from? ")   # Returns a string
```

---

## 6. Python Tuples

### What is a Tuple?
- An **ordered, immutable** collection enclosed in `()`
- Elements are called **fields**
- Can hold mixed types; supports nesting
- Index starts at **0**

```python
empty_tuple = ()
int_tuple   = (1, 2, 3)
str_tuple   = ("Hello", "Python")
mixed_tuple = (1, 2.5, "hello")

# Implicit tuple (no brackets needed)
my_tuple = 1, 2.5, "hello"   # Python auto-creates a tuple
```

### Accessing Fields
```python
my_tuple[0]      # First field
my_tuple[-1]     # Last field
my_tuple[1][2]   # Element at index 2 of nested list at field 1
```

### Tuple Slicing
```python
my_tuple[1:]       # From index 1 to end
my_tuple[2:4]      # Fields at index 2 and 3
my_tuple[::2]      # Every second field
```
> Slicing a **tuple** returns a **tuple**; slicing a **list** returns a **list**.

### Immutability
```python
my_tuple[0] = 99    # ❌ TypeError: 'tuple' does not support item assignment
del my_tuple[0]     # ❌ TypeError: 'tuple' does not support item deletion
del my_tuple        # ✅ Deletes the whole tuple from memory
```
> **Exception:** A **mutable object inside a tuple** (e.g., a list) *can* be modified.

### Unpacking
```python
a, b, c = (1, 2, 3)
```

### Useful Tuple Methods & Functions
```python
my_tuple.index("H")   # Index of 'H'
my_tuple.count("l")   # Count occurrences
"e" in my_tuple       # True/False membership check
"e" not in my_tuple   # Negated check
sum(int_tuple)        # Sum of numeric tuple
list(int_tuple)       # Convert to list
set(int_tuple)        # Convert to set (removes duplicates)
```

---

## 7. The `zip` Function

### Concept
`zip()` pairs corresponding elements from two (or more) iterables — like a zipper.

```python
a = (1, 2, 3)
b = ('a', 'b', 'c')
zipped = zip(a, b)
```

### Converting Zipped Results
```python
tuple(zipped)   # ((1, 'a'), (2, 'b'), (3, 'c'))
list(zipped)    # [(1, 'a'), (2, 'b'), (3, 'c')]
dict(zipped)    # {1: 'a', 2: 'b', 3: 'c'}  ← great for key-value pairs
set(zipped)     # {(1, 'a'), (2, 'b'), (3, 'c')}
```

### Unzipping
```python
x, y = zip(*result)   # Recovers original tuples
```

### Zipping 3 Collections
```python
zip(numbers_list, str_list, numbers_tuple)   # Each inner tuple has 3 fields
```

---

## 8. Lists vs. Tuples — Summary

| Feature | List | Tuple |
|---------|------|-------|
| Syntax | `[1, 2, 3]` | `(1, 2, 3)` |
| Ordered | ✅ | ✅ |
| Mixed types | ✅ | ✅ |
| Indexed from `0` | ✅ | ✅ |
| Slicing | ✅ | ✅ |
| Duplicates allowed | ✅ | ✅ |
| **Mutable** | ✅ | ❌ **Immutable** |
| Add/remove elements | ✅ | ❌ |
| Use case | General-purpose collection | Fixed/read-only data (e.g., coordinates) |

---

## 9. Other Python Data Types (Quick Reference)

| Type | Syntax | Ordered | Mutable | Unique Values |
|------|--------|---------|---------|---------------|
| `list` | `[1, 2]` | ✅ | ✅ | ❌ |
| `tuple` | `(1, 2)` | ✅ | ❌ | ❌ |
| `dict` | `{"a": 1}` | ✅ (3.7+) | ✅ | Keys only |
| `set` | `{1, 2}` | ❌ | ✅ | ✅ |
