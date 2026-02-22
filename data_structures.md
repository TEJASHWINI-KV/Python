# Python Data Structures - Complete Learning Notes 📚

## Table of Contents
1. [Sets](#sets)
2. [Namedtuple](#namedtuple)
3. [Generators](#generators)
4. [Zip Function](#zip-function)
5. [Collections Module](#collections-module)
6. [Dictionaries](#dictionaries)
7. [Built-in Libraries](#built-in-libraries)
8. [Quick Reference](#quick-reference)

---

## SETS

### What is it?
A collection of **unique, unordered items** that uses **hash tables** for O(1) lookup time.

### Why it matters
- Membership checking is 1000x faster than lists
- Automatically removes duplicates
- Perfect for deduplication and fast lookups

### How it works
```python
# Hash table calculates location instantly
data = {"first", "second", "third"}
if "second" in data:  # O(1) operation - instant!
    print("Found")

# Add item - hash determines position
data.add("fourth")

# Remove duplicates
log_entries = ["ERROR", "INFO", "ERROR", "WARNING", "INFO"]
unique_logs = set(log_entries)  # {'ERROR', 'INFO', 'WARNING'}
```

### Memory Trick
**"SET = SPEED"** - Use sets when you need speed  
**"No Duplicates, No Index"** - Can't have duplicates, can't use [0]  
**"Hash = Fast"** - Hash tables make it fast

### Analogy
**The Locker Room Analogy:**
- **List** = Row of 1,000 unmarked lockers. You check each one: #1? No. #2? No. ... #847? YES! ❌ Slow
- **Set** = Numbered lockers with a magic formula. You calculate #847 and go directly there. ✅ Fast

### Real Examples
```python
# Fast membership checking
servers_set = {"web1", "web2", "web3", "db1"}
print("web1" in servers_set)  # O(1) - instant!

# Remove duplicates
duplicate_servers = ["web1", "web1", "db1", "web1"]
unique = set(duplicate_servers)  # {'web1', 'db1'}

# Find common elements
group_a = {"web1", "web2", "web3"}
group_b = {"web3", "db1"}
shared = group_a & group_b  # {'web3'}

# Find differences
all_servers = {"srv1", "srv2", "srv3"}
patched = {"srv1", "srv3"}
need_patching = all_servers - patched  # {'srv2'}
```

### When to Use
✅ Checking "Does this exist?"  
✅ Removing duplicates  
✅ Finding common items  

### Common Mistakes
```python
# ❌ Trying to access by index
my_set = {1, 2, 3}
# print(my_set[0])  # TypeError

# ❌ Trying to add unhashable items
# my_set.add([1, 2])  # TypeError: unhashable type: 'list'

# ✅ Convert to list if you need indexing
my_list = list(my_set)
print(my_list[0])
```

---

## NAMEDTUPLE

### What is it?
A **tuple with named fields** that you can access using **dot notation** instead of indices.

### Why it matters
- Makes code self-documenting (readable without comments)
- Immutable like tuples (safe from changes)
- Uses same memory as tuples (efficient)
- Perfect for returning multiple values from functions

### How it works
```python
from collections import namedtuple

# Define structure
Server = namedtuple("Server", ["hostname", "ip", "port"])

# Create instance
web_server = Server(hostname="web1", ip="192.168.1.10", port=80)

# Access by name (readable)
print(web_server.hostname)  # "web1"

# Still works like tuple
print(web_server[0])  # "web1"
```

### Memory Trick
**"Named = Readable"** - Named fields make code readable  
**"Tuple + Names = namedtuple"** - It's a tuple with names  
**"Return with Context"** - Use when returning multiple values

### Analogy
**Labeled vs Unmarked Boxes:**
- **Tuple** = Three boxes: (85, 92, 78) - What do these numbers mean? 🤔
- **Namedtuple** = Labeled boxes: math=85, english=92, science=78 - CLEAR! ✅

### Real Examples
```python
from collections import namedtuple

# Example 1: Return multiple values with context
Grades = namedtuple("Grades", ["math", "english", "science"])

def get_student_grades():
    return Grades(math=85, english=92, science=78)

grades = get_student_grades()
print(f"Math: {grades.math}")  # 85 - CLEAR!

# Example 2: Configuration data
Config = namedtuple("Config", ["db_host", "db_port", "timeout"])
prod_config = Config(db_host="prod-db.com", db_port=5432, timeout=30)

# Example 3: Process multiple records
Student = namedtuple("Student", ["name", "role", "grade"])
students = [
    Student("web1", "webserver", "10th"),
    Student("db1", "database", "11th")
]

# Example 4: Convert dict to namedtuple
server_dict = {"hostname": "web1", "ip": "10.0.0.5"}
Server = namedtuple("Server", server_dict.keys())
server = Server(**server_dict)
print(server.hostname)  # "web1"
```

### When to Use
✅ Return multiple values with labels  
✅ Create immutable data containers  
✅ Make code more readable  

### Common Mistakes
```python
# ❌ Trying to modify (immutable)
Server = namedtuple("Server", ["hostname", "port"])
server = Server("web1", 80)
# server.port = 443  # AttributeError

# ✅ Create new instance with changes
new_server = server._replace(port=443)
```

---

## GENERATORS

### What is it?
A function that returns values **one at a time** using `yield` instead of loading all values into memory.

### Why it matters
- Prevents memory exhaustion with large datasets
- Can process infinite sequences
- Starts producing values immediately (lazy evaluation)
- Critical for processing databases, files, API pagination

### How it works
```python
# Generator gives one value at a time
def count_up_to(max):
    count = 1
    while count <= max:
        yield count  # Returns value, pauses, remembers state
        count += 1

for num in count_up_to(5):
    print(num)  # 1, 2, 3, 4, 5 (one at a time)
```

### Memory Trick
**"Yield = One at a Time"** - Yield gives one value at a time  
**"List = All, Generator = Lazy"** - Lists load all, generators are lazy  
**"Memory Saver"** - Use generators to save memory

### Analogy
**Reading a Book:**
- **List** = Print entire book in memory first, then read pages ❌ Heavy
- **Generator** = Read one page at a time, don't load whole book ✅ Light

### Real Examples
```python
# Example 1: Process large log file without memory overload
def read_logs(filename):
    with open(filename) as f:
        for line in f:
            yield line.strip()

for log_line in read_logs("/var/log/syslog"):
    if "ERROR" in log_line:
        print(log_line)  # Only ONE line in memory!

# Example 2: Database query results (1 billion rows)
def fetch_servers_from_db(connection):
    cursor = connection.cursor()
    cursor.execute("SELECT hostname, ip FROM servers")
    for row in cursor:
        yield {"hostname": row[0], "ip": row[1]}

# Process without loading 1 billion rows
# for server in fetch_servers_from_db(conn):
#     process_server(server)

# Example 3: Generate sequences
def fibonacci(n):
    a, b = 0, 1
    count = 0
    while count < n:
        yield a
        a, b = b, a + b
        count += 1

for num in fibonacci(10):
    print(num)  # 0, 1, 1, 2, 3, 5, 8, 13, 21, 34

# Example 4: Filter and transform
def get_active_servers(servers):
    for server in servers:
        if server.get("status") == "active":
            yield server["hostname"]

servers = [
    {"hostname": "web1", "status": "active"},
    {"hostname": "web2", "status": "inactive"},
    {"hostname": "db1", "status": "active"}
]

active = list(get_active_servers(servers))  # ['web1', 'db1']
```

### When to Use
✅ Reading huge files  
✅ Getting data from databases  
✅ Processing data streams  
✅ Creating infinite sequences  

### Common Mistakes
```python
# ❌ Trying to access by index
gen = (x for x in range(5))
# print(gen[0])  # TypeError

# ❌ Reusing generator (exhausted after first use)
gen = (x for x in range(5))
print(list(gen))  # [0, 1, 2, 3, 4]
print(list(gen))  # [] - exhausted!

# ✅ Convert to list if you need multiple access
numbers = list(gen)
print(sum(numbers))  # Works
print(sum(numbers))  # Works again
```

---

## ZIP FUNCTION

### What is it?
A built-in function that **pairs elements** from multiple iterables into tuples.

### Why it matters
- Combines parallel lists without manual indexing
- Cleaner code when processing multiple lists together
- Automatically handles different lengths (stops at shortest)

### How it works
```python
hosts = ["web1", "web2", "web3"]
ips = ["10.0.0.1", "10.0.0.2", "10.0.0.3"]

for host, ip in zip(hosts, ips):
    print(f"{host} -> {ip}")
# web1 -> 10.0.0.1
# web2 -> 10.0.0.2
# web3 -> 10.0.0.3
```

### Memory Trick
**"Zip = Zipper"** - Zips two things together like a zipper  
**"Parallel Processing"** - Process multiple lists in parallel  
**"Stops at Shortest"** - Stops when shortest list ends

### Analogy
**The Zipper:**
- **List** = Two separate rows of teeth. Match them manually ❌ Tedious
- **Zip** = Zipper brings them together automatically ✅ Automatic

### Real Examples
```python
# Example 1: Combine server data
hostnames = ["web1", "db1", "cache1"]
ips = ["192.168.1.10", "192.168.1.20", "192.168.1.30"]
roles = ["webserver", "database", "redis"]

for host, ip, role in zip(hostnames, ips, roles):
    print(f"{host}: {ip} ({role})")

# Example 2: Create dictionary from two lists
keys = ["hostname", "ip", "port"]
values = ["web1", "10.0.0.5", 80]
config = dict(zip(keys, values))
# {'hostname': 'web1', 'ip': '10.0.0.5', 'port': 80}

# Example 3: Compare two lists
before = [80, 443, 22, 3306]
after = [80, 443, 22, 5432]

for old_port, new_port in zip(before, after):
    if old_port != new_port:
        print(f"Port changed: {old_port} -> {new_port}")

# Example 4: Unzip (reverse operation)
servers = [("web1", "10.0.0.1"), ("web2", "10.0.0.2")]
hostnames, ips = zip(*servers)
print(list(hostnames))  # ['web1', 'web2']
```

### When to Use
✅ Combine parallel lists  
✅ Create dictionaries from keys and values  
✅ Process multiple lists together  

### Common Mistakes
```python
# ❌ Expecting zip to pad with None
list1 = [1, 2, 3, 4, 5]
list2 = [10, 20]
result = list(zip(list1, list2))
print(result)  # [(1, 10), (2, 20)] - stops at shortest!

# ✅ Use itertools.zip_longest for padding
from itertools import zip_longest
result = list(zip_longest(list1, list2, fillvalue=0))
# [(1, 10), (2, 20), (3, 0), (4, 0), (5, 0)]
```

---

## COLLECTIONS MODULE

### Counter - Tally Items

**What it does:** Counts how many times each item appears

```python
from collections import Counter

# Count votes
votes = ["Fortnite", "Minecraft", "Fortnite", "Among Us", "Fortnite"]
vote_count = Counter(votes)
print(vote_count)
# Counter({'Fortnite': 3, 'Minecraft': 1, 'Among Us': 1})

# Find most common
print(vote_count.most_common(1))  # [('Fortnite', 3)]

# Count word frequency
from collections import Counter
sentence = "hello world hello python hello"
word_count = Counter(sentence.split())
print(word_count)  # Counter({'hello': 3, 'world': 1, 'python': 1})
```

---

### defaultdict - No KeyError

**What it does:** Returns default value for missing keys instead of raising KeyError

```python
from collections import defaultdict

# Regular dict - CRASH!
regular = {}
# print(regular["missing"])  # KeyError

# defaultdict - Works!
student_grades = defaultdict(int)  # Default is 0
print(student_grades["Alex"])  # 0 (not KeyError!)

# Grouping students by grade
students = [("Alex", "10th"), ("Jordan", "11th"), ("Sam", "10th")]
by_grade = defaultdict(list)

for name, grade in students:
    by_grade[grade].append(name)

print(by_grade)
# defaultdict(list, {'10th': ['Alex', 'Sam'], '11th': ['Jordan']})
```

---

### deque - Queue/Stack Operations

**What it does:** Add and remove items from BOTH ends efficiently (O(1))

```python
from collections import deque

queue = deque(["Alex", "Jordan", "Sam"])

# Add to right (back)
queue.append("Taylor")
print(queue)  # deque(['Alex', 'Jordan', 'Sam', 'Taylor'])

# Add to left (front)
queue.appendleft("Morgan")
print(queue)  # deque(['Morgan', 'Alex', 'Jordan', 'Sam', 'Taylor'])

# Remove from right (back)
last = queue.pop()  # 'Taylor'

# Remove from left (front)
first = queue.popleft()  # 'Morgan'

print(queue)  # deque(['Alex', 'Jordan', 'Sam'])
```

---

### OrderedDict - Insertion Order (Python < 3.6)

**Note:** In Python 3.7+, regular `dict` maintains insertion order. Use OrderedDict only for pre-3.6 code.

```python
from collections import OrderedDict

languages = OrderedDict()
languages["Python"] = 1990
languages["Java"] = 1995
languages["Ruby"] = 1995

# Get keys in insertion order
print([k for k in languages.keys()])
# ['Python', 'Java', 'Ruby']
```

---

## DICTIONARIES

### What is it?
A **key-value collection** for fast lookups and mappings.

### Why it matters
- O(1) lookup time (like sets, but with values)
- Self-documenting (keys describe data)
- Most used data structure in Python

### How it works
```python
student = {
    "name": "Alex",
    "age": 15,
    "grade": "10th",
    "gpa": 3.8
}

# Access by key
print(student["name"])  # "Alex" - CLEAR!
```

### Analogy
**Real Dictionary:**
- You look up a word → Get its definition
- You look up a name → Get their info

### Real Examples
```python
# Example 1: Phone book
phone_book = {
    "Alex": "555-1234",
    "Jordan": "555-5678",
    "Sam": "555-9999"
}
print(phone_book["Alex"])  # "555-1234" - FAST!

# Example 2: Student grades
grades = {
    "Math": "A",
    "English": "B",
    "Science": "A"
}

# Example 3: Dictionary as switch statement
def usa_tax(amount):
    return amount * 0.10

def uk_tax(amount):
    return amount * 0.20

tax_rates = {
    "USA": usa_tax,
    "UK": uk_tax
}

def calculate_tax(country, amount):
    return tax_rates[country](amount)

print(calculate_tax("USA", 1000))  # 100
```

---

### Merge Dictionaries

#### Python 3.9+ (Best)
```python
dict1 = {"name": "Alex", "age": 15}
dict2 = {"grade": "10th"}
merged = dict1 | dict2
```

#### Python 3.5+ (Common)
```python
dict1 = {"name": "Alex"}
dict2 = {"grade": "10th"}
merged = {**dict1, **dict2}
```

#### Older Python
```python
dict1 = {"name": "Alex"}
dict2 = {"grade": "10th"}
merged = dict1.copy()
merged.update(dict2)
```

---

### Pretty Print Dictionary

```python
import json

data = {
    "name": "Alex",
    "grades": {"math": "A", "english": "B"},
    "age": 15
}

# Pretty format
print(json.dumps(data, indent=4, sort_keys=True))

# Output:
# {
#     "age": 15,
#     "grades": {
#         "english": "B",
#         "math": "A"
#     },
#     "name": "Alex"
# }
```

---

## BUILT-IN LIBRARIES

### json - Handle JSON Data
```python
import json

# Python to JSON
data = {"name": "Alex", "age": 15}
json_string = json.dumps(data)  # Convert to string

# JSON to Python
parsed = json.loads(json_string)  # Convert back to dict
print(parsed["name"])  # "Alex"
```

### datetime - Work with Dates
```python
from datetime import datetime

now = datetime.now()
print(f"Year: {now.year}")
print(f"Month: {now.month}")
print(f"Is weekend? {now.weekday() >= 5}")
```

### math - Math Operations
```python
import math

print(math.pi)        # 3.14159...
print(math.sqrt(16))  # 4.0
print(math.ceil(4.3)) # 5
print(math.floor(4.8))# 4
```

### re - Regular Expressions
```python
import re

# Find email addresses
text = "Contact: email@gmail.com or help@school.org"
emails = re.findall(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b', text)
print(emails)  # ['email@gmail.com', 'help@school.org']
```

### itertools - Advanced Looping
```python
from itertools import combinations, islice

# All possible pairs
students = ["Alex", "Jordan", "Sam"]
for pair in combinations(students, 2):
    print(pair)

# Get first N items from generator
def get_servers():
    for i in range(1000000):
        yield f"server{i}"

first_five = list(islice(get_servers(), 5))
```

### functools - Function Tools
```python
from functools import lru_cache

@lru_cache(maxsize=128)
def expensive_function(n):
    return n ** 2

result1 = expensive_function(5)  # Computes
result2 = expensive_function(5)  # Returns cached result (instant!)
```

---

## QUICK REFERENCE

### When to Use Each Data Structure

| Data Structure | Best For | Speed | Example |
|---|---|---|---|
| **List** | Ordered data, indexing | O(n) lookup | `[1, 2, 3]` |
| **Set** | Fast lookup, no duplicates | O(1) lookup | `{1, 2, 3}` |
| **Tuple** | Immutable, ordered data | O(n) lookup | `(1, 2, 3)` |
| **Namedtuple** | Labeled immutable data | O(n) lookup | `Point(x=1, y=2)` |
| **Dictionary** | Key-value mapping | O(1) lookup | `{"key": "value"}` |
| **Generator** | Large data streams | O(1) per item | `yield value` |
| **Counter** | Count occurrences | O(1) lookup | `Counter([1, 1, 2])` |
| **defaultdict** | Grouping/counting | O(1) lookup | `defaultdict(list)` |
| **deque** | Queue/Stack ops | O(1) both ends | `deque([1, 2])` |

---

### Memory Tricks Summary

**SETS:** "SET = SPEED" → "No Duplicates, No Index" → "Hash = Fast"

**NAMEDTUPLE:** "Named = Readable" → "Tuple + Names" → "Return with Context"

**GENERATORS:** "Yield = One at a Time" → "List = All, Generator = Lazy" → "Memory Saver"

**ZIP:** "Zip = Zipper" → "Parallel Processing" → "Stops at Shortest"

**COUNTER:** "Count = Frequency" → Count how many of each

**DEFAULTDICT:** "Default = No Error" → No KeyError for missing keys

**DEQUE:** "Deque = Both Ends" → Add/remove from both sides O(1)

**DICTIONARY:** "Dict = Lookup" → O(1) key-value lookup

---

### Hash Tables for O(1) Lookup

**What is O(1)?**
Constant time - takes same time regardless of data size.

**How Hash Tables Work:**
1. Calculate hash of item: `hash("web1")` → 1234567890
2. Use hash to find memory location instantly
3. Go directly to that location
4. Instant access!

**Locker Room Analogy:**
- **List:** Walk down row checking each locker 1, 2, 3... 🐌 Slow
- **Hash Table:** Use formula to go directly to locker 847 ⚡ Fast

---

## ALL IN ONE RUNNABLE FILE

```python
#!/usr/bin/env python3
"""
Python Data Structures - Complete Demo
All concepts in one file!
"""

from collections import namedtuple, Counter, defaultdict, deque
import json
import math

print("=" * 70)
print("PYTHON DATA STRUCTURES - COMPLETE DEMO")
print("=" * 70)

# 1. SETS
print("\n1️⃣  SETS - Fast Searching")
print("-" * 70)
games = {"Fortnite", "Minecraft", "Fortnite", "Among Us"}
print(f"Games (auto-removed duplicate): {games}")
print(f"Is Minecraft here? {'Minecraft' in games} (O(1) - instant!)")

# 2. NAMEDTUPLE
print("\n2️⃣  NAMEDTUPLE - Labeled Data")
print("-" * 70)
Student = namedtuple("Student", ["name", "age", "grade", "gpa"])
alex = Student(name="Alex", age=15, grade="10th", gpa=3.8)
print(f"{alex.name}: Grade {alex.grade}, GPA {alex.gpa}")

# 3. GENERATORS
print("\n3️⃣  GENERATORS - One at a Time")
print("-" * 70)
def count_to_million():
    for i in range(1000000):
        yield i

print("First 5 from 1 million (only uses memory for 1 at a time):")
for i, num in enumerate(count_to_million()):
    if i < 5:
        print(f"  {num}", end=" ")
    else:
        break
print("\n✓ Constant memory usage!")

# 4. ZIP
print("\n4️⃣  ZIP - Pairing Lists")
print("-" * 70)
names = ["Alex", "Jordan", "Sam"]
grades_list = ["A", "B", "A"]
for name, grade in zip(names, grades_list):
    print(f"  {name}: {grade}")

# 5. COUNTER
print("\n5️⃣  COUNTER - Count Occurrences")
print("-" * 70)
votes = ["Fortnite", "Minecraft", "Fortnite", "Among Us", "Fortnite"]
vote_count = Counter(votes)
print(f"Votes: {vote_count}")
print(f"Most popular: {vote_count.most_common(1)[0][0]}")

# 6. DEFAULTDICT
print("\n6️⃣  DEFAULTDICT - No KeyError")
print("-" * 70)
grades_dict = defaultdict(list)
grades_dict["Math"].append("A")
grades_dict["Math"].append("B")
grades_dict["Science"].append("A")
print(f"Grades by subject: {dict(grades_dict)}")
print(f"Missing subject (Python): {grades_dict['Python']} (not KeyError!)")

# 7. DEQUE
print("\n7️⃣  DEQUE - Queue Operations")
print("-" * 70)
queue = deque(["Alex", "Jordan", "Sam"])
queue.append("Taylor")
queue.appendleft("Morgan")
print(f"Queue: {queue}")
print(f"Remove left: {queue.popleft()}")
print(f"Remove right: {queue.pop()}")
print(f"Remaining: {queue}")

# 8. DICTIONARIES
print("\n8️⃣  DICTIONARIES - Key-Value Lookup")
print("-" * 70)
student_info = {
    "name": "Alex",
    "age": 15,
    "grade": "10th",
    "classes": ["Math", "Science", "History"]
}
print(f"Student: {student_info['name']}")
print(f"Classes: {', '.join(student_info['classes'])}")

# 9. MERGE DICTIONARIES
print("\n9️⃣  MERGE DICTIONARIES")
print("-" * 70)
dict1 = {"name": "Alex", "age": 15}
dict2 = {"grade": "10th"}
merged = dict1 | dict2  # Python 3.9+
print(f"Merged: {merged}")

# 10. PRETTY PRINT
print("\n🔟  PRETTY PRINT - JSON Format")
print("-" * 70)
complex_data = {
    "student": "Alex",
    "grades": {"math": "A", "english": "B"},
    "age": 15
}
print(json.dumps(complex_data, indent=2, sort_keys=True))

# 11. BUILT-IN LIBRARIES
print("\n1️⃣1️⃣  BUILT-IN LIBRARIES")
print("-" * 70)
print(f"math.sqrt(16) = {math.sqrt(16)}")
print(f"math.pi = {math.pi:.4f}")

from datetime import datetime
now = datetime.now()
print(f"Current year: {now.year}")

print("\n" + "=" * 70)
print("✓ All concepts demonstrated! Save this for reference.")
print("=" * 70)
```

---

## QUICK STUDY CHECKLIST ✅

### Must Know
- [ ] Sets use hash tables for O(1) lookup
- [ ] Namedtuple adds names to tuples for readability
- [ ] Generators use `yield` and save memory
- [ ] Zip pairs up parallel lists
- [ ] Counter counts occurrences
- [ ] defaultdict doesn't raise KeyError
- [ ] Dictionary is O(1) key-value lookup
- [ ] Hash tables enable fast searching

### Interview Questions
1. What's difference between list and set lookup speed?
2. When would you use namedtuple vs regular tuple?
3. Why use generators over lists for large data?
4. How does zip() work with different length lists?
5. When use Counter vs defaultdict?

---

**Happy Learning! 🎓**
