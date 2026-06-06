# Functions

---

## What is a Function?

A function is a reusable block of code that does one specific job.
Write the logic once — use it anywhere, anytime.

In ML every step is a function.
Data cleaning, model training, evaluation, prediction —
all of it is functions chained together.

```python
def calculate_bonus(salary: float, rating: int) -> float:
    if rating >= 9:
        return salary * 0.20
    elif rating >= 7:
        return salary * 0.10
    else:
        return salary * 0.05
```

---

## Default Parameters

A parameter with a default value becomes optional.
If the caller does not pass it, Python uses the default.

You see this in every ML library.
`RandomForest(n_estimators=100)` — all the settings
you did not pass have defaults inside the library.

```python
def connect_database(host: str, port: int = 5432) -> str:
    return f"Connected to {host} on port {port}"

connect_database("localhost", 3306)  # uses 3306
connect_database("localhost")        # uses default 5432
```

---

## Keyword Arguments

Name your arguments explicitly when calling a function.
Makes code readable — no need to remember order.

```python
def train_model(model_type: str, learning_rate: float, epochs: int) -> None:
    print(f"{model_type} | lr={learning_rate} | epochs={epochs}")

train_model(model_type="XGBoost", learning_rate=0.01, epochs=100)
```

---

## *args — Variable Positional Arguments

Accepts any number of positional arguments.
Arrives inside the function as a tuple.

```python
def total_score(*args: int) -> int:
    return sum(args)

total_score(85, 90, 78, 92)  # works with any number of values
```

---

## **kwargs — Variable Keyword Arguments

Accepts any number of named arguments.
Arrives inside the function as a dictionary.
Used constantly in ML configuration and logging.

```python
def show_model_config(**kwargs) -> None:
    for key, value in kwargs.items():
        print(f"  {key.replace('_', ' ').title()}: {value}")

show_model_config(model="XGBoost", learning_rate=0.05, max_depth=6)
```

---

## Scope

Variables created inside a function only exist inside it.
Variables created outside can be read anywhere.

```python
max_retries = 3  # global — accessible everywhere

def fetch_data(url: str) -> str:
    timeout = 30  # local — only exists inside this function
    return f"Fetching {url} with timeout {timeout}"

print(timeout)  # ERROR — does not exist outside
```

Rule — never modify global variables inside functions.
Pass them as parameters instead.

---

## Type Hints

Tells Python and your teammates exactly what a function
expects and what it returns.
Expected in all professional and corporate code.

```python
def calculate_accuracy(correct: int, total: int) -> float:
    return (correct / total) * 100
```

| Hint | Means |
|---|---|
| `int` | Integer |
| `float` | Decimal |
| `str` | String |
| `bool` | True or False |
| `list` | List |
| `dict` | Dictionary |
| `None` | Returns nothing |

---

## Docstrings — Google Format

Explains what a function does, what it expects, and what it returns.
Non-negotiable in professional and team environments.

```python
def calculate_accuracy(correct: int, total: int) -> float:
    """
    Calculates prediction accuracy as a percentage.

    Args:
        correct (int): Number of correct predictions.
        total (int): Total number of predictions.

    Returns:
        float: Accuracy percentage between 0 and 100.

    Raises:
        ValueError: If total is zero or negative.
    """
    return (correct / total) * 100
```

Three sections — Args, Returns, Raises.
This is the Google format used in most ML companies.

---

## Input Validation

Never trust what gets passed into your function.
Production code receives unexpected inputs constantly.
Always validate before processing.

```python
def calculate_accuracy(correct: int, total: int) -> float:
    if not isinstance(correct, int) or not isinstance(total, int):
        raise TypeError("correct and total must be integers")
    if total <= 0:
        raise ValueError("total must be greater than zero")
    if correct < 0:
        raise ValueError("correct cannot be negative")
    if correct > total:
        raise ValueError("correct cannot exceed total")
    return (correct / total) * 100
```

Three checks to always write:
- correct type — `isinstance()`
- valid range — comparisons
- non-empty string — `if not value`

---

## Logging

`print()` is for scripts. `logging` is for production.

Why logging beats print:
- gives you levels — DEBUG, INFO, WARNING, ERROR, CRITICAL
- can turn off lower levels in production
- shows exactly where the message came from
- every corporate ML system uses it

```python
import logging

logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
logger = logging.getLogger(__name__)

def train_model(epochs: int) -> None:
    logger.info(f"Training started for {epochs} epochs")
    logger.info("Training complete")
```

---

## if __name__ == "__main__"

Prevents test code from running when someone imports
your function into another file.

```python
def calculate_accuracy(correct: int, total: int) -> float:
    return (correct / total) * 100


if __name__ == "__main__":
    # This ONLY runs when you execute this file directly
    # Does NOT run when imported elsewhere
    result = calculate_accuracy(80, 100)
    print(f"Accuracy: {result:.2f}%")
```

Always put your test code inside this block.

---

## Constants

Values that never change go in UPPER_CASE.
Signals to everyone — this value is fixed, do not modify it.

```python
VALID_PRIORITIES = ["low", "normal", "high"]

def set_priority(priority: str) -> None:
    if priority not in VALID_PRIORITIES:
        raise ValueError(f"priority must be one of {VALID_PRIORITIES}")
    logger.info(f"Priority set to {priority}")
```

---

## Rules to Follow for Every Function

| Rule | Why |
|---|---|
| One function, one job | Easy to test, debug, and reuse |
| Type hints on everything | Clarity for teammates and tools |
| Docstring on every function | Non-negotiable in teams |
| Validate all inputs | Production code cannot trust inputs |
| Use logging not print | Production systems need log levels |
| Constants in UPPER_CASE | Signals the value should not change |
| `if __name__ == "__main__"` | Prevents accidental execution on import |
| Descriptive names | Code should read like plain English |

---

## My Understanding

*Write here in your own words.
What was confusing? What clicked and when?
Be honest — this is for you.*
