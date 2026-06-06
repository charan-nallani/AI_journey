# My AI Journey 🚀

A structured path from Python fundamentals to ML Engineering,
GenAI/LLM Engineering, and AI Research Engineering.

**Goal:** ML Engineer → GenAI/LLM Engineer → AI Research Engineer

---

## Learning Path

| Stage | Topics | Status |
|---|---|---|
| 01 Python Foundations | Functions, OOP, Strings, Files, Errors | 🔄 In Progress |
| 02 Mathematics for ML | Linear Algebra, Statistics, Calculus | ⏳ Upcoming |
| 03 Data Handling | NumPy, Pandas, Visualization | ⏳ Upcoming |
| 04 Machine Learning | Classical ML, Scikit-learn, Model Evaluation | ⏳ Upcoming |
| 05 Deep Learning | Neural Networks, CNNs, Transformers, PyTorch | ⏳ Upcoming |
| 06 MLOps | Docker, FastAPI, MLflow, Cloud | ⏳ Upcoming |
| 07 Generative AI | LLMs, RAG, Agents, Fine-tuning | ⏳ Upcoming |

---

## Stage 01 — Python Foundations 🔄

---

### Functions

**What:** A reusable block of code that does one specific job.

**Why it matters in ML:**
Every ML pipeline is built from functions. Data cleaning,
model training, evaluation, prediction — all functions.
Writing them correctly from day one builds the right habits.

**The important stuff:**

`*args` and `**kwargs` — you will see these everywhere in
ML libraries. When you call `model.fit(X, epochs=10)`,
that `epochs=10` is kwargs under the hood.

```python
def train_model(*args, **kwargs):
    # args  → positional inputs
    # kwargs → named settings like epochs, lr, batch_size
```

**Type hints** — tell Python and your teammates exactly
what a function expects and returns. Not optional in
professional code.

```python
def calculate_accuracy(correct: int, total: int) -> float:
```

**Logging over print** — `print()` is for scripts.
`logging` is for production. It gives you log levels
(INFO, WARNING, ERROR) and timestamps. Every corporate
ML system uses logging.

```python
import logging
logger = logging.getLogger(__name__)
logger.info("Model training started")
```

**`if __name__ == "__main__"`** — prevents your test code
from running when someone imports your function into
another file. Always use this.

**Defensive programming** — always validate inputs.
A function that trusts its inputs blindly will crash
in production at the worst possible moment.

```python
if total <= 0:
    raise ValueError("Total must be greater than zero")
```

---

### Object Oriented Programming

**What:** A way to bundle related data and behavior into
a single reusable blueprint called a class.

**Why it matters in ML:**
Every ML library is built with OOP. When you write
`model = RandomForest()` you are creating an object.
When you write `model.fit(X, y)` you are calling a method.
Understanding OOP means understanding how the tools
you use every day actually work.

**The important stuff:**

**`self`** — the most confusing thing for beginners.
It simply means "this specific object." When you have
100 model objects, self is how each one refers to its
own data.

```python
class MLModel:
    def __init__(self, name):
        self.name = name  # THIS object's name
```

**`__init__` vs method calls** — two completely
different moments in time.

```python
# Moment 1 — setting up the object's identity (permanent)
model = ClassificationModel("RandomForest", num_classes=3)

# Moment 2 — giving the object something to work on (temporary)
model.predict([1, 2, 3])
```

**Encapsulation** — prefix with `_` to signal that an
attribute is internal. Protects object integrity.

```python
self._is_trained = False  # don't touch this from outside
```

**Inheritance** — build new classes from existing ones.
This is exactly how PyTorch works. Every model you build
in PyTorch inherits from `nn.Module`.

```python
class MyModel(nn.Module):  # inherits from PyTorch's base
    def __init__(self):
        super().__init__()  # always call parent's init
```

---

### String Operations

**What:** Tools for manipulating and cleaning text data.

**Why it matters in ML:**
Real world data is messy. Column names have spaces,
values have inconsistent casing, text has extra whitespace.
Before any model sees data, strings need cleaning.
This is called preprocessing and it's 60% of real ML work.

**The important stuff:**

**`split()` and `join()`** — the most used pair in data work.

```python
# split a CSV row into values
row = "Charan,ML Engineer,India"
parts = row.split(",")   # ['Charan', 'ML Engineer', 'India']

# join them back cleanly
result = " | ".join(parts)  # 'Charan | ML Engineer | India'
```

**Column name cleaning** — you will write this in
every real ML project.

```python
def clean_column(name: str) -> str:
    return name.strip().lower().replace(" ", "_")
# "  Customer Name  " → "customer_name"
```

**f-strings with formatting** — for clean aligned reports.

```python
print(f"{'Model':<20} {'Accuracy':>10}")
print(f"{'RandomForest':<20} {0.94:>10.2f}")
```

**String immutability** — strings never change in place.
Always capture the return value or nothing happens.

```python
name = "charan"
name.upper()        # does nothing useful
name = name.upper() # correct — reassign the result
```

---

## Professional Standards I Follow

| Practice | Why |
|---|---|
| Type hints on everything | Clarity for teammates and IDEs |
| Google format docstrings | Industry standard documentation |
| `logging` not `print()` | Production systems need log levels |
| Input validation always | Production code cannot trust inputs |
| `if __name__ == "__main__"` | Safe imports, clean module design |
| Constants in `UPPER_CASE` | Python convention, signals immutability |
| One function, one job | Easier to test, debug, and reuse |
| Docstring on every class and function | Non-negotiable in team environments |

---

## Active Projects

**Multi-Object Tracking with Re-Identification**
Real-time detection and identity-persistent tracking
across occlusions and multi-camera setups.
Stack: YOLOv8 · ByteTrack · torchreid · PyTorch
Status: 🔄 In Progress

---

## Platforms

| Platform | Profile |
|---|---|
| GitHub | [charan-nallani](https://github.com/charan-nallani) |
| Kaggle | [charan16692](https://kaggle.com/charan16692) |
| LinkedIn | [charan-nallani](https://linkedin.com/in/charan-nallani) |
| Portfolio | charannallani.github.io — coming soon |

---

*Updated after every completed topic.*
