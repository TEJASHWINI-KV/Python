# Python Logging Module - Complete Notes
> Module learned: Feb 24, 2026

---

## 1. Why Use Logging Instead of Print?

### The Problem with Print:
- Clutters code with repeated lines
- Must be manually removed before production
- No timestamps, no levels, no filtering

### The Solution - Logging:
- Clean, 1-line log statements
- Timestamps added automatically
- Filter messages by severity with ONE line change
- Save logs to files permanently

### Memory Trick:
> **"Print = sticky notes scattered everywhere. Logging = organized diary."**

---

## 2. Setting Up a Logger (Step by Step)

```python
import logging
import sys

# Step 1: Create logger
logger = logging.getLogger(__name__)

# Step 2: Create handler (where logs go)
stream_handler = logging.StreamHandler(sys.stdout)

# Step 3: Add handler to logger
logger.addHandler(stream_handler)

# Step 4: Set log level
logger.setLevel(logging.DEBUG)
```

### What Each Line Does:
| Line | Purpose |
|------|---------|
| `getLogger(__name__)` | Creates your personal messenger for this file |
| `StreamHandler(sys.stdout)` | Tells logger to output to console |
| `addHandler()` | Connects handler to logger |
| `setLevel()` | Sets the minimum severity to show |

### Memory Trick:
> **"Logger = Messenger. Handler = GPS Destination. setLevel = Volume Control."**

---

## 3. Log Levels

| Level | Value | Use When... |
|-------|-------|-------------|
| NOTSET | 0 | No level set, inherits from parent |
| DEBUG | 10 | Fixing/testing code (developer only) |
| INFO | 20 | Normal updates, everything is fine |
| WARNING | 30 | Something unusual, app still runs |
| ERROR | 40 | Something broke, feature doesn't work |
| CRITICAL | 50 | App might crash entirely |

### Print the Values:
```python
import logging
print(logging.NOTSET)    # 0
print(logging.DEBUG)     # 10
print(logging.INFO)      # 20
print(logging.WARNING)   # 30
print(logging.ERROR)     # 40
print(logging.CRITICAL)  # 50
```

### Memory Trick (Alarm Analogy):
> - **DEBUG (10)** = 🔍 Detective mode - tiny details  
> - **INFO (20)** = ℹ️ Status update - all good  
> - **WARNING (30)** = ⚠️ Yellow light - something odd  
> - **ERROR (40)** = 🚨 Red alert - something broke  
> - **CRITICAL (50)** = 💥 Emergency - app may crash  

---

## 4. Logging Methods

```python
logger.debug("Detailed debug info")
logger.info("General update")
logger.warning("Something unusual!")
logger.error("Something broke!")
logger.critical("App may crash!")

# Alternative way using log(level, msg):
logger.log(logging.CRITICAL, "App may crash!")  # Same as logger.critical()
```

### Real Example - Division App:
```python
def division():
    try:
        dividend = float(input("Enter dividend: "))
        logger.info("Dividend entered: {}".format(dividend))
        divisor = float(input("Enter divisor: "))
        logger.info("Divisor entered: {}".format(divisor))
    except ValueError:
        logger.log(logging.CRITICAL, "No dividend or divisor value entered!")
        return None
    if divisor == 0:
        logger.error("Attempting to divide by 0!")
        return None
    else:
        return dividend / divisor

result = division()
if result is None:
    logger.warning("The result value is None!")
```

---

## 5. Setting Log Levels (setLevel)

### The Default Problem:
> Python **automatically** sets the default level to **WARNING (30)** behind the scenes.  
> So DEBUG (10) and INFO (20) are **hidden** unless you change it.

```python
logger.setLevel(logging.DEBUG)    # Show ALL messages (10+)
logger.setLevel(logging.INFO)     # Show INFO and above (20+)
logger.setLevel(logging.WARNING)  # Show WARNING and above (30+) ← DEFAULT
logger.setLevel(logging.ERROR)    # Show ERROR and above (40+)
logger.setLevel(logging.CRITICAL) # Show only CRITICAL (50)
```

### Rule:
> **Logger only shows messages where level number ≥ set level number**

### Height Requirement Analogy:
> Setting level to WARNING (30) = "You must be at least 30 to ride"  
> INFO is only 20 → ❌ Not allowed in!

### When to Use Which Level:
| Phase | Set Level | Why |
|-------|-----------|-----|
| 🔧 Development | `DEBUG` | See everything |
| 🚀 Production | `WARNING` | Only see real problems |
| 🚨 Crash fixing | `CRITICAL` | Only see app-breaking errors |

---

## 6. Logging to a File

```python
# Saves logs permanently to a file
file_handler = logging.FileHandler("output.log")
logger.addHandler(file_handler)
```

### Where is the file saved?
> **Same folder as your Python script** (by default)

```python
# Default - same folder as script
file_handler = logging.FileHandler("output.log")

# Custom path
file_handler = logging.FileHandler("/Users/yourname/logs/output.log")
```

### Console vs File:
| | Console | File |
|--|---------|------|
| See logs while running | ✅ | ❌ |
| Logs saved after program ends | ❌ | ✅ |
| Search through old logs | ❌ | ✅ |

### Memory Trick:
> **"Console logs = temporary. File logs = permanent."**

---

## 7. Multiple Handlers (Console + File Together)

```python
import logging
import sys

logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

# Handler 1 - Save to file
file_handler = logging.FileHandler("calculator.log")
logger.addHandler(file_handler)

# Handler 2 - Show on console
stream_handler = logging.StreamHandler(sys.stdout)
logger.addHandler(stream_handler)
```

### Visual:
```
logger.error("Something broke!")
         ↓
    ┌────┴────┐
    ↓         ↓
Console     calculator.log
(instant)   (permanent)
```

### Memory Trick:
> **"More handlers = more places your logs go!"**

---

## 8. Formatting Logs

### Default Format (no customization):
```
WARNING:script:This is a warning!
```

### Common Placeholders:
| Placeholder | Shows |
|-------------|-------|
| `%(asctime)s` | Date & time |
| `%(levelname)s` | WARNING / ERROR etc. |
| `%(name)s` | Logger/module name |
| `%(lineno)d` | Line number in code |
| `%(message)s` | Your actual message |

### How to Apply:
```python
# Step 1: Create formatter
formatter = logging.Formatter("[%(asctime)s] {%(levelname)s} %(name)s: #%(lineno)d - %(message)s")

# Step 2: Apply to handler
stream_handler.setFormatter(formatter)
```

### Output:
```
[2026-02-24 10:30:45] {WARNING} script: #25 - This is a warning!
```

### Different Formats for Different Handlers:
```python
# Detailed format for file
formatter1 = logging.Formatter("[%(asctime)s] {%(levelname)s} %(name)s: #%(lineno)d - %(message)s")
file_handler.setFormatter(formatter1)

# Simple format for console
formatter2 = logging.Formatter("[%(asctime)s] {%(levelname)s} - %(message)s")
stream_handler.setFormatter(formatter2)
```

### News Headline Analogy:
> Without format: *"There was a fire"*  
> With format: *"🔴 BREAKING [10:30 PM, Feb 24] - Fire at Main Street, Block 5"*

---

## 9. basicConfig() - Quick Setup in 1 Line

### Without basicConfig() (6 lines):
```python
logger = logging.getLogger(__name__)
stream_handler = logging.StreamHandler(sys.stdout)
formatter = logging.Formatter("[%(asctime)s] %(levelname)s - %(message)s")
stream_handler.setFormatter(formatter)
logger.addHandler(stream_handler)
logger.setLevel(logging.DEBUG)
```

### With basicConfig() (1 line!):
```python
logging.basicConfig(
    filename='basic_config.log',
    level=logging.INFO,
    format='[%(asctime)s] %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)
```

### Arguments:
| Argument | Purpose |
|----------|---------|
| `filename=` | File to save logs to |
| `level=` | Minimum log level to show |
| `format=` | Template for log messages |

> **No arguments?** → Auto creates StreamHandler to `sys.stderr` with default format.

### Memory Trick:
> **"basicConfig() = All-in-one setup kit for logging!"**

---

## 10. Complete Example (Full Division App)

```python
import logging
import sys

# Setup
logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

# File handler - detailed format
file_handler = logging.FileHandler("calculator.log")
formatter1 = logging.Formatter("[%(asctime)s] {%(levelname)s} %(name)s: #%(lineno)d - %(message)s")
file_handler.setFormatter(formatter1)
logger.addHandler(file_handler)

# Console handler - simple format
stream_handler = logging.StreamHandler(sys.stdout)
formatter2 = logging.Formatter("[%(asctime)s] {%(levelname)s} - %(message)s")
stream_handler.setFormatter(formatter2)
logger.addHandler(stream_handler)

# App logic
def division():
    try:
        dividend = float(input("Enter the dividend: "))
        logger.info("Dividend entered: {}".format(dividend))
        divisor = float(input("Enter the divisor: "))
        logger.info("Divisor entered: {}".format(divisor))
    except ValueError:
        logger.log(logging.CRITICAL, "No dividend or divisor value entered!")
        return None
    if divisor == 0:
        logger.error("Attempting to divide by 0!")
        return None
    else:
        return dividend / divisor

result = division()
if result is None:
    logger.warning("The result value is None!")
```

---

## Quick Reference Cheat Sheet

```python
import logging
import sys

# 1. Create logger
logger = logging.getLogger(__name__)

# 2. Set level
logger.setLevel(logging.DEBUG)  # DEBUG=10, INFO=20, WARNING=30, ERROR=40, CRITICAL=50

# 3. Handlers
stream_handler = logging.StreamHandler(sys.stdout)   # → Console
file_handler = logging.FileHandler("app.log")        # → File

# 4. Format
formatter = logging.Formatter("[%(asctime)s] %(levelname)s - %(message)s")
stream_handler.setFormatter(formatter)
file_handler.setFormatter(formatter)

# 5. Add handlers
logger.addHandler(stream_handler)
logger.addHandler(file_handler)

# 6. Log messages
logger.debug("debug msg")      # Dev use only
logger.info("info msg")        # General info
logger.warning("warning msg")  # Unusual event
logger.error("error msg")      # Something broke
logger.critical("critical msg") # App may crash

# OR use basicConfig() for quick setup:
logging.basicConfig(filename='app.log', level=logging.DEBUG, format='[%(asctime)s] %(levelname)s - %(message)s')
```

---
*Notes compiled from Codecademy - Logging in Python module*

