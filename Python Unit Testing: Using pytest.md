# 🧪 pytest — Complete Revision Notes
> Module: Python Testing with pytest  
> Last Updated: February 2026

---

## 📌 Table of Contents
1. [Why Testing?](#1-why-testing)
2. [Setup & Installation](#2-setup--installation)
3. [The `assert` Statement](#3-the-assert-statement)
4. [Test Discovery](#4-test-discovery)
5. [Separating Source from Tests](#5-separating-source-from-tests)
6. [Testing Exceptions](#6-testing-exceptions)
7. [Running Specific Tests](#7-running-specific-tests)
8. [Controlling Test Failures](#8-controlling-test-failures)
9. [Markers (Tagging Tests)](#9-markers-tagging-tests)
10. [Skipping Tests](#10-skipping-tests)
11. [Debugging with PDB](#11-debugging-with-pdb)
12. [Parameterization](#12-parameterization)
13. [Fixtures](#13-fixtures)
14. [conftest.py — Shared Fixtures](#14-conftestpy--shared-fixtures)
15. [Quick Reference Cheatsheet](#15-quick-reference-cheatsheet)

---

## 1. Why Testing?

**What:** Automated tests verify your code works correctly — automatically, every time you change something.

**Analogy:** 🔒 Tests are like a smoke alarm. You don't notice them when everything is fine. But the moment something breaks, they immediately alert you before it reaches the end user.

**Key Point:** As apps are updated and deployed frequently (CI/CD), manually checking everything is impossible. Automated tests = quality guarantee at speed.

---

## 2. Setup & Installation

```bash
pip install -U pytest    # -U = upgrade if older version exists
```

Run tests:
```bash
pytest          # scan current directory and run all tests
py.test         # exact alias — same result
```

> 🧠 **Memory Trick:** `pip install -U` → think "U = Update to latest"

---

## 3. The `assert` Statement

**What:** The word `assert` tells Python: *"This MUST be true. If it's not — crash and fail the test."*

**Why pytest wins here:** In `unittest`, you need to memorize many functions.  
pytest replaces ALL of them with just `assert` + plain Python:

| unittest (old way) | pytest (simple way) |
|---|---|
| `assertEqual(a, b)` | `assert a == b` |
| `assertIsInstance(x, int)` | `assert isinstance(x, int)` |
| `assertIn(item, list)` | `assert item in list` |
| `assertGreater(a, b)` | `assert a > b` |

```python
# Examples
assert 2 + 2 == 4              # basic equality
assert isinstance(42, int)     # type check
assert "u" in "sun"            # membership check
assert len([1,2,3]) == 3       # length check
```

> 🧠 **Memory Trick:** `assert` = "I ASSERT this is true, prove me wrong!"  
> One word replaces everything.

---

## 4. Test Discovery

**What:** pytest automatically finds your tests — you don't point it to them manually.

**The Rules:**
1. Scans all Python files in the current folder
2. Picks up files with `test` in the name → `test_calc.py` ✅ or `calc_test.py` ✅
3. Inside those files, runs any function whose name **starts with** `test_`

```python
def test_perimeter():    # ✅ starts with "test_" → pytest runs this
    assert 2 * (5 + 7) == 24

def perimeter_test():    # ❌ does NOT start with "test_" → ignored
    assert 2 * (5 + 7) == 24
```

**Analogy:** 🐕 Test discovery is like a sniffer dog — trained to find anything that "smells like" a test (starts with `test_`). Rename it and the dog walks right past it.

> 🧠 **Memory Trick:** **"test_ prefix = test gets fixed (picked up)"**

---

## 5. Separating Source from Tests

**Best Practice:** Keep your actual code in one file, tests in a separate file.

```
project/
├── cal.py          ← real code (no tests)
└── cal_test.py     ← only tests (imports cal.py)
```

```python
# cal.py
def add(a, b):
    return a + b

def product(a, b):
    return a * b
```

```python
# cal_test.py
import cal

def test_add():
    assert cal.add(2, 5) == 7

def test_product():
    assert cal.product(3, 4) == 12
```

> 🧠 **Memory Trick:** Think of it like a kitchen 🍳  
> `cal.py` = the recipe (the actual work)  
> `cal_test.py` = the food critic (checks if the recipe worked)

---

## 6. Testing Exceptions

**What:** Sometimes you *want* your code to raise an error. You test that it raises the *right* error.

**Analogy:** 🔥 A fire extinguisher is "supposed" to spray foam when triggered. You test that it actually does. The "error" (foam spraying) is the *correct* behavior.

```python
import pytest

def divide(a, b):
    return a / b

# ✅ Test that dividing by zero raises ZeroDivisionError
def test_divide_by_zero():
    with pytest.raises(ZeroDivisionError):
        divide(18, 0)

# ✅ Test that wrong types raise TypeError
def test_divide_wrong_type():
    with pytest.raises(TypeError):
        divide(18, "a")
```

**How `with pytest.raises()` works:**
- Code inside the `with` block is expected to raise that error
- If it DOES raise it → ✅ TEST PASSES
- If it does NOT raise it → ❌ TEST FAILS

> 🧠 **Memory Trick:** `with pytest.raises(Error)` = "I raise my hand and say: I EXPECT this error here!"

---

## 7. Running Specific Tests

**Commands:**

```bash
# Run everything
pytest

# Run one specific file
pytest cal_test.py

# Run one specific function inside a file
pytest cal_test.py::test_add

# Run tests whose name contains "add"
pytest -k "add"

# Run tests whose name contains "add" OR "string"
pytest -k "add or string"

# Run tests whose name contains BOTH "add" AND "string"
pytest -k "add and string"

# Verbose output (shows each test name)
pytest -v

# Run tests in a specific directory
pytest tests/
```

**Verbose output example:**
```
cal_test.py::test_add        PASSED
cal_test.py::test_product    PASSED
```

> 🧠 **Memory Trick:**  
> `-k` = **k**eyword filter  
> `-v` = **v**erbose (see everything)  
> `::` = "inside this file, find this function"

---

## 8. Controlling Test Failures

**By default:** pytest runs ALL tests even when some fail.

```bash
pytest              # run all, show all failures at the end
pytest -x           # STOP at first failure (good for solo devs)
pytest --maxfail=2  # stop after 2 failures (middle ground)
pytest --tb=no      # hide error details (traceback)
pytest --tb=short   # show brief error info
pytest --tb=long    # show full error info
pytest -q           # quiet mode — minimal output
```

**Analogy:** 🏃 Imagine running a race with hurdles:
- Default → jump every hurdle even if you trip, report all falls at the end
- `-x` → stop the moment you trip on the first hurdle
- `--maxfail=2` → stop only after tripping on 2 hurdles

> **`traceback`** = the detailed error report Python prints when something crashes. Shows which line broke and why.

> 🧠 **Memory Trick:**  
> `-x` = e**X**it on first failure  
> `-q` = **q**uiet (less noise)  
> `--tb` = **t**race**b**ack control

---

## 9. Markers (Tagging Tests)

**What:** Markers are labels/tags you attach to tests so you can run specific groups.

**Analogy:** 🏷️ Like colour-coded folders. Tag some tests "number", others "string" — then pull out only the "number" folder when you need it.

```python
import pytest

@pytest.mark.number          # tag as "number"
def test_type():
    assert type(1 + 2) == int

@pytest.mark.number          # tag as "number"
def test_add_int():
    assert 5 + 2 == 7

@pytest.mark.string          # tag as "string"
def test_string():
    assert "u" in "sun"

@pytest.mark.string          # tag as "string"
def test_add_string():
    assert "hello " + "world" == "hello world"
```

```bash
pytest -m number    # runs only test_type and test_add_int
pytest -m string    # runs only test_string and test_add_string
```

**Built-in markers vs Custom markers:**
- `@pytest.mark.skip` → built-in (skip this test)
- `@pytest.mark.parametrize` → built-in (run with multiple inputs)
- `@pytest.mark.number` → custom (you made this up)

> 🧠 **Memory Trick:** `-m` = **m**arker filter

---

## 10. Skipping Tests

**What:** Temporarily disable a test without deleting it.

**When to use:**
- Infrastructure is being upgraded
- Feature is being rebuilt
- Test is known to be flaky temporarily

```python
@pytest.mark.skip(reason="Temporarily disabled — server maintenance")
def test_string():
    assert "u" in "sun"
```

```bash
pytest -v           # shows test was SKIPPED (marked with 's')
pytest -v -rs       # shows skip reason in summary
pytest -v -rs -rf   # shows skip reason + failure details
```

**Output symbols:**
```
.  = PASSED
F  = FAILED
s  = SKIPPED
```

> 🧠 **Memory Trick:**  
> `-r` = show su**r**face **r**eport  
> `s` after `-r` = include **s**kips  
> `f` after `-r` = include **f**ailures

---

## 11. Debugging with PDB

**PDB** = Python DeBugger. Pauses test execution so you can inspect variables and find what went wrong.

```bash
pytest --pdb      # pause at every FAILURE, open debugger
pytest --trace    # pause at the START of every test
```

**Inside PDB:**
```
c   or   cont   → continue running (resume tests)
q               → quit
p variable_name → print a variable's value
```

**Analogy:** 🔬 PDB is like pressing pause on a movie at the exact frame where something goes wrong — so you can zoom in and inspect every detail before pressing play again.

> 🧠 **Memory Trick:**  
> `--pdb` = pause at **p**ro**b**lem  
> `--trace` = pause to **trace** every step  
> `c` = **c**ontinue

---

## 12. Parameterization

**Problem:** You want to test `add()` with integers, floats, and strings — so you write 3 separate functions that are almost identical. Messy!

**Solution:** `@pytest.mark.parametrize` — run ONE test function with MANY different inputs automatically.

**Without parameterization (messy):**
```python
def test_add_int():
    assert cal.add(1, 2) == 3

def test_add_float():
    assert cal.add(5.2, 2.4) == 7.6

def test_add_string():
    assert cal.add("winter", " season") == "winter season"
```

**With parameterization (clean):**
```python
import pytest
import cal

@pytest.mark.parametrize("input_1, input_2, result", [
    (1,       2,         3),               # integers
    (5.2,     2.4,       7.6),             # floats
    ("winter", " season", "winter season") # strings
])
def test_add(input_1, input_2, result):
    assert cal.add(input_1, input_2) == result
```

pytest runs `test_add` **3 times** — once per row in the list.

**How to read the decorator:**
```
@pytest.mark.parametrize(
    "var1, var2, var3",   ← names of variables (comma-separated string)
    [                      ← list of test cases
        (val1, val2, val3),  ← each tuple = one test run
        (val1, val2, val3),
    ]
)
```

**Analogy:** 🎰 Like a slot machine test. You set up the machine once, then feed it different coin combinations. It runs the same check for each combination automatically.

> 🧠 **Memory Trick:** "Para" = many. Parametrize = run the same test with many different parameters.

---

## 13. Fixtures

**Problem:** Multiple tests all need the same setup code (e.g., open a file, connect to a DB). Copy-pasting setup into every test = messy and error-prone.

**Solution:** A **fixture** — a special setup function that runs automatically before each test that uses it.

**Analogy:** 🍽️ A restaurant prep cook (fixture) chops vegetables and sets up the station BEFORE each chef (test) starts cooking. Each chef gets a fresh, prepared station without doing the setup themselves.

```python
import pytest

@pytest.fixture                  # ← this decorator = "I am a fixture"
def open_file():
    print("Setting up: opening file")
    f = open("data.txt", "a")
    yield f                      # ← hand file to the test
    print("Tearing down: closing file")
    f.close()                    # ← runs AFTER test finishes

def test_write(open_file):       # ← "open_file" injected automatically
    open_file.write("hello\n")
    # assertions here...

def test_size(open_file):        # ← same fixture, fresh each time
    # open_file is available here too
    pass
```

### The `yield` keyword in fixtures

`yield` splits a fixture into two parts:

```
Code BEFORE yield  →  runs BEFORE the test  (setup)
     yield value   →  the value given to the test
Code AFTER yield   →  runs AFTER the test   (teardown)
```

**Analogy:** 🏨 Hotel analogy:
- Before `yield` = housekeeping cleans the room (setup)
- `yield` = guest checks in and uses the room (test runs)
- After `yield` = housekeeping cleans up after checkout (teardown)

### Fixture Scope

Controls **how often** the fixture runs:

```python
@pytest.fixture(scope="function")  # default — runs before EVERY test
@pytest.fixture(scope="module")    # runs ONCE per file — shared by all tests in it
```

| Scope | Runs... | Analogy |
|---|---|---|
| `function` (default) | Before every single test | Fresh coffee per person ☕ |
| `module` | Once at file start, shared | One big coffee pot for everyone ☕☕ |

```python
# scope="module" example — fixture runs only once for the whole file
@pytest.fixture(scope="module")
def db_connection():
    conn = connect_to_db()
    yield conn
    conn.close()
```

> 🧠 **Memory Trick:**  
> `yield` = "pause here, run the test, then come back"  
> `scope="module"` = "one setup for the whole module"

---

## 14. conftest.py — Shared Fixtures

**Problem:** You have 10 test files all needing the same fixture. Copying it into each file = nightmare to maintain.

**Solution:** Put fixtures in a file named **`conftest.py`**. pytest loads it automatically — no imports needed anywhere.

```
project/
├── conftest.py          ← fixtures defined here (shared toolbox)
├── test_file_ops.py     ← uses fixtures automatically
└── test_data_ops.py     ← also uses fixtures automatically
```

```python
# conftest.py
import pytest

@pytest.fixture
def write_file():
    f = open("test.txt", "w")
    for i in range(10):
        f.write(f"\nline {i}")
    f.flush()
    yield f
    f.close()

@pytest.fixture
def readonly_file():
    # write content first, then open as read-only
    with open("readonly.txt", "w") as f:
        for i in range(10):
            f.write(f"\nline {i}")
    f = open("readonly.txt", "r")
    yield f
    f.close()
```

```python
# test_file_ops.py — NO import of conftest needed!
def test_write_ten_lines(write_file):      # pytest finds it from conftest.py
    for i in range(10):
        write_file.write(f"\nnew line {i}")
    # assertions...

def test_field_count(readonly_file):       # uses different fixture
    readonly_file.readline()               # skip blank first line
    line = readonly_file.readline()
    fields = line.split(" ")
    assert len(fields) == 4
```

**Key Rule:** `conftest.py` applies to all test files in the **same directory and subdirectories**.

**Analogy:** 🧰 conftest.py is a shared toolbox in the middle of a workshop. Every worker (test file) in that workshop can grab tools (fixtures) from it without asking.

> 🧠 **Memory Trick:** `conftest` = **con**figuration + **test** = shared test configuration file

---

## 15. Quick Reference Cheatsheet

### CLI Commands
```bash
pytest                        # run all tests
pytest file.py                # run one file
pytest file.py::func          # run one function
pytest -k "keyword"           # filter by name
pytest -m marker_name         # filter by marker
pytest -v                     # verbose output
pytest -q                     # quiet output
pytest -x                     # stop on first failure
pytest --maxfail=N            # stop after N failures
pytest --tb=no/short/long     # traceback detail level
pytest -rs                    # show skip reasons
pytest -rf                    # show failure reasons
pytest --pdb                  # debug on failure
pytest --trace                # debug every test
pytest -s                     # show print statements
pytest -v -s                  # verbose + show prints
```

### Decorators
```python
@pytest.fixture               # mark as fixture
@pytest.fixture(scope="module")  # module-scoped fixture
@pytest.mark.skip(reason="...")  # skip this test
@pytest.mark.number           # custom marker
@pytest.mark.parametrize("a,b,result", [(1,2,3), (4,5,9)])  # parameterize
```

### Exception Testing
```python
with pytest.raises(ZeroDivisionError):
    divide(10, 0)
```

### Project Structure (Best Practice)
```
project/
├── conftest.py          ← shared fixtures
├── source/
│   ├── cal.py           ← your real code
│   └── utils.py
└── tests/
    ├── test_cal.py      ← tests for cal.py
    └── test_utils.py    ← tests for utils.py
```

---

## 🧠 Master Memory Map

```
pytest
  │
  ├── FIND tests ──────────── Discovery (test_ prefix rule)
  │
  ├── WRITE tests ─────────── assert keyword (replaces all unittest asserts)
  │
  ├── RUN tests ───────────── pytest / pytest file.py / pytest -k / pytest -m
  │
  ├── CONTROL failures ────── -x / --maxfail / --tb
  │
  ├── ORGANISE tests ──────── Markers (@pytest.mark.custom)
  │
  ├── SKIP tests ──────────── @pytest.mark.skip
  │
  ├── TEST errors ─────────── with pytest.raises(ErrorType)
  │
  ├── REDUCE repetition ───── @pytest.mark.parametrize
  │
  ├── SETUP/TEARDOWN ──────── @pytest.fixture + yield
  │     └── Scope ────────── function (default) / module
  │
  └── SHARE fixtures ──────── conftest.py
```

---

*Notes compiled from: Python Testing Learning Path — pytest Module*
