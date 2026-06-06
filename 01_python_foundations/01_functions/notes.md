# Functions — Complete Notes

---

## What is a Function and Why Does it Exist?

A function is a reusable block of code that does one specific job.
The entire point is — write the logic once, use it everywhere.

In ML pipelines everything is a function.
Data cleaning, model training, evaluation, prediction —
all of it is built from functions chained together.

Without functions — repetitive and error prone:
```python
bonus1 = 80000 * 0.20
bonus2 = 60000 * 0.10
bonus3 = 45000 * 0.05
```

With functions — write once, use everywhere:
```python
def calculate_bonus(salary, rating):
    if rating >= 9:
        return salary * 0.20
    elif rating >= 7:
        return salary * 0.10
    else:
        return salary * 0.05

bonus1 = calculate_bonus(80000, 9)
bonus2 = calculate_bonus(60000, 7)
bonus3 = calculate_bonus(45000, 5)
```

---

## Basic Structure

```python
def function_name(parameter1, parameter2):
    # logic goes here
    return result
```

- `def` — tells Python you are defining a function
- parameters — the inputs the function expects
- `return` — sends a value back to whoever called it

---

## Default Parameters

When a parameter has a default value it becomes optional.
If the caller does not pass it, Python uses the default.

```python
def greet(name, role="Engineer"):
    print(f"Welcome {name}, you are a {role}")

greet("Charan", "ML Engineer")  # Welcome Charan, you are a ML Engineer
greet("Ravi")                    # Welcome Ravi, you are a Engineer
```

You see this everywhere in ML libraries.
When you write `RandomForest(n_estimators=100)` and skip
other settings, those skipped ones use their defaults
defined inside the library.

---

## Keyword Arguments

You can name your arguments explicitly when calling a function.
This makes code readable — especially important in ML where
functions have many parameters and order gets confusing.

```python
def create_model(model_type, learning_rate, epochs):
    print(f"{model_type} | lr={learning_rate} | epochs={epochs}")

# Without keyword arguments — confusing, must remember order
create_model("RandomForest", 0.01, 100)

# With keyword arguments — clear, order does not matter
create_model(model_type="RandomForest", learning_rate=0.01, epochs=100)
```

---

## *args — Variable Positional Arguments

Allows a function to accept any number of positional arguments.
They arrive inside the function as a tuple.

```python
def add_scores(*args):
    return sum(args)

print(add_scores(85, 90, 78))       # 253
print(add_scores(70, 80, 90, 100))  # 340
```

The * before args is what makes it collect multiple values.
The name args is just a convention — you could name it anything.

---

## **kwargs — Variable Keyword Arguments

Allows a function to accept any number of named arguments.
They arrive inside the function as a dictionary.
You will see this constantly in ML library code.

```python
def show_config(**kwargs):
    print("=" * 35)
    print("MODEL CONFIGURATION")
    print("=" * 35)
    for key, value in kwargs.items():
        formatted_key = key.replace("_", " ").title()
        print(f"  {formatted_key}: {value}")
    print("=" * 35)

show_config(model="XGBoost", learning_rate=0.05, max_depth=6, n_estimators=200)
```

Output:
```
===================================
MODEL CONFIGURATION
===================================
  Model: XGBoost
  Learning Rate: 0.05
  Max Depth: 6
  N Estimators: 200
===================================
```

---

## Scope — Where Variables Live

Variables created inside a function only exist inside that function.
Variables created outside can be read anywhere.
This is called scope.

```python
budget = 100000  # global — accessible everywhere

def calculate_cost(team_size):
    cost_per_person = 5000  # local — only exists inside this function
    return team_size * cost_per_person

print(calculate_cost(10))   # 50000
print(cost_per_person)      # ERROR — does not exist outside the function
```

Important rule — never modify global variables inside functions.
It creates bugs that are extremely hard to trace in large codebases.
If you need to use a global value, pass it as a parameter instead.

---

## Type Hints

Type hints tell Python and your teammates exactly what a function
expects and what it returns. Python does not enforce them but
professional codebases expect them everywhere.

```python
# Without type hints — unclear
def calculate_accuracy(correct, total):
    return (correct / total) * 100

# With type hints — clear and professional
def calculate_accuracy(correct: int, total: int) -> float:
    return (correct / total) * 100
```

Common type hints you will use:

| Hint | Means |
|---|---|
| `int` | Integer number |
| `float` | Decimal number |
| `str` | String |
| `bool` | True or False |
| `list` | A list |
| `dict` | A dictionary |
| `None` | Function returns nothing |

---

## Docstrings — Google Format

A docstring explains what a function does, what it expects,
and what it returns. Written immediately after the function
definition in triple quotes.

Non-negotiable in professional and team environments.
Without docstrings your code is unreadable to others.

```python
def calculate_accuracy(correct: int, total: int) -> float:
    """
    Calculates prediction accuracy as a percentage.

    Args:
        correct (int): Number of correct predictions.
        total (int): Total number of predictions.

    Returns:
        float: Accuracy as a percentage between 0 and 100.

    Raises:
        ValueError: If total is zero or negative.
        TypeError: If inputs are not integers.
    """
    return (correct / total) * 100
```

Three sections — Args, Returns, Raises.
This is the Google docstring format used in most ML companies.

---

## Input Validation — Defensive Programming

Never trust what gets passed into your function.
In production, functions receive unexpected inputs constantly.
Always validate before processing.

```python
def calculate_accuracy(correct: int, total: int) -> float:
    """
    Calculates prediction accuracy as a percentage.

    Args:
        correct (int): Number of correct predictions.
        total (int): Total number of predictions.

    Returns:
        float: Accuracy as a percentage.

    Raises:
        TypeError: If inputs are not integers.
        ValueError: If values are out of valid range.
    """
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

The three most common validation checks:
- is the type correct — isinstance()
- is the value in a valid range — comparisons
- is the string non-empty — if not value

---

## Logging — The Professional Way to Output

`print()` is for beginners and quick scripts.
`logging` is for production code.

Why logging beats print:
- gives you levels — DEBUG, INFO, WARNING, ERROR, CRITICAL
- you can turn off lower level logs in production
- shows where the log came from
- every corporate ML system uses logging

```python
import logging

# Setup — do this once at the top of your file
logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
logger = logging.getLogger(__name__)

def train_model(epochs: int) -> None:
    """
    Simulates model training.

    Args:
        epochs (int): Number of training epochs.

    Returns:
        None
    """
    logger.info(f"Starting training for {epochs} epochs")
    logger.info("Training complete")

train_model(10)
```

Output:
```
INFO: Starting training for 10 epochs
INFO: Training complete
```

Log levels in order of severity:
- DEBUG — detailed diagnostic information
- INFO — confirmation things are working
- WARNING — something unexpected but not breaking
- ERROR — something failed
- CRITICAL — serious failure, program may stop

---

## if __name__ == "__main__"

This pattern prevents your test code from running when
someone imports your function into another file.

```python
def calculate_accuracy(correct: int, total: int) -> float:
    """..."""
    return (correct / total) * 100


# This block ONLY runs when you execute this file directly
# It does NOT run when this file is imported elsewhere
if __name__ == "__main__":
    result = calculate_accuracy(80, 100)
    print(f"Accuracy: {result:.2f}%")
```

Without this block — every time someone imports your
function, all your test code runs automatically.
That is never what you want.

Always put your test code inside this block.

---

## Constants

Values that never change should be named in UPPER_CASE.
This signals to everyone reading the code that this
value is fixed and should not be modified.

```python
VALID_PRIORITIES = ["low", "normal", "high"]
MAX_RETRIES = 3
DEFAULT_LEARNING_RATE = 0.001

def set_priority(priority: str) -> None:
    """
    Sets task priority after validation.

    Args:
        priority (str): Priority level. Must be low, normal, or high.

    Returns:
        None

    Raises:
        ValueError: If priority is not a valid value.
    """
    if priority not in VALID_PRIORITIES:
        raise ValueError(f"priority must be one of {VALID_PRIORITIES}")
    logger.info(f"Priority set to {priority}")
```

---

## Complete Production Ready Example

Everything together in one function — this is
the standard you write to from today:

```python
import logging

logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
logger = logging.getLogger(__name__)

VALID_PRIORITIES = ["low", "normal", "high"]


def send_notification(
    recipient: str,
    message: str,
    priority: str = "normal",
) -> bool:
    """
    Sends a notification to a recipient.

    Args:
        recipient (str): Email address of the recipient.
        message (str): Notification message body.
        priority (str): Priority level — low, normal, or high.
            Defaults to normal.

    Returns:
        bool: True if notification sent successfully.

    Raises:
        TypeError: If any argument is not a string.
        ValueError: If recipient or message is empty or invalid.
        ValueError: If priority is not a valid value.
    """
    if not isinstance(recipient, str):
        raise TypeError("recipient must be a string")
    if not isinstance(message, str):
        raise TypeError("message must be a string")
    if not recipient:
        raise ValueError("recipient cannot be empty")
    if "@" not in recipient:
        raise ValueError("recipient must be a valid email address")
    if not message:
        raise ValueError("message cannot be empty")
    if priority not in VALID_PRIORITIES:
        raise ValueError(f"priority must be one of {VALID_PRIORITIES}")

    logger.info(f"Sending {priority} priority notification to {recipient}")
    logger.info(f"Message: {message}")
    return True


if __name__ == "__main__":
    # Test 1: Normal usage
    send_notification(
        recipient="charan@example.com",
        message="Your model training is complete",
        priority="high"
    )

    # Test 2: Default priority
    send_notification(
        recipient="team@example.com",
        message="Weekly report is ready"
    )

    # Test 3: Invalid priority
    try:
        send_notification(
            recipient="charan@example.com",
            message="Test",
            priority="urgent"
        )
    except ValueError as e:
        logger.error(f"Validation error: {e}")

    # Test 4: Empty recipient
    try:
        send_notification(recipient="", message="Test")
    except ValueError as e:
        logger.error(f"Validation error: {e}")
```

---

## Rules I Follow for Every Function

| Rule | Why |
|---|---|
| One function, one job | Easy to test, debug, and reuse |
| Type hints on everything | Clarity for teammates and tools |
| Docstring on every function | Non-negotiable in team environments |
| Validate all inputs | Production code cannot trust inputs |
| Use logging not print | Production systems need log levels |
| Constants in UPPER_CASE | Signals the value should not change |
| if __name__ == "__main__" | Prevents accidental execution on import |
| Descriptive names | Code should read like plain English |

---

## What Confused Me and What Clicked

*Write your own understanding here in your own words.
What was confusing at first? What clicked and when?
This section is yours — write honestly.*
```
