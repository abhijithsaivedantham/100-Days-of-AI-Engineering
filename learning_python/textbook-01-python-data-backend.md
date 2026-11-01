# Textbook 01 --- Python, Data Analysis, SQL and APIs

**Days 1--20 \| Outcome:** A tested Python API backed by a database.

# Section 1 --- Intermediate Python (Days 1--5)

## Collections and mutability

-   `list`: ordered, mutable sequence.
-   `tuple`: ordered, immutable sequence.
-   `set`: unique elements; useful for membership and set operations.
-   `dict`: key-value mapping.

``` python
values = [4, 2, 4, 8]
unique = set(values)
profile = {"name": "Asha", "active": True}
coordinates = (17.4, 78.4)
```

Mutability means an object can change after creation. Assignment creates
another reference, not a copy:

``` python
a = [1, 2]
b = a
b.append(3)
assert a == [1, 2, 3]
```

A shallow copy duplicates the outer container; nested mutable values may
still be shared. `copy.deepcopy` recursively copies nested objects.

## Comprehensions

``` python
nums = [1, 2, 3, 4, 5]
squares = [n * n for n in nums]
even_squares = [n * n for n in nums if n % 2 == 0]
mapping = {n: n * n for n in nums}
```

Use comprehensions when they remain readable. Prefer a loop or named
function for complex logic.

## Functions and arguments

``` python
def describe(name: str, age: int = 0) -> str:
    return f"{name}: {age}"

def total(*values: float) -> float:
    return sum(values)

def profile(**fields: object) -> dict:
    return fields
```

`*args` collects extra positional arguments; `**kwargs` collects extra
keyword arguments. Type hints document intent and help tools, but Python
does not enforce them automatically.

Avoid mutable defaults:

``` python
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

## OOP

A class combines state and behaviour. Know: - Encapsulation: expose safe
operations around state. - Inheritance: specialise a base type; avoid
deep hierarchies. - Polymorphism: different objects support a shared
interface. - Composition: build an object from collaborators; often
simpler than inheritance. - Dataclasses: reduce boilerplate for
data-focused classes.

``` python
from dataclasses import dataclass

@dataclass
class Task:
    title: str
    priority: int = 3
    completed: bool = False
```

## Iterators, generators and decorators

A generator yields values lazily:

``` python
def batches(items, size):
    for start in range(0, len(items), size):
        yield items[start:start + size]
```

A decorator wraps a function to add reusable behaviour such as timing or
logging.

``` python
from functools import wraps
from time import perf_counter

def timed(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = perf_counter()
        result = func(*args, **kwargs)
        print(f"{func.__name__}: {perf_counter()-start:.4f}s")
        return result
    return wrapper
```

## Exceptions and files

Catch specific exceptions and use context managers for resources.

``` python
import json
from pathlib import Path

path = Path("sample.json")
path.write_text(json.dumps({"day": 1}), encoding="utf-8")
data = json.loads(path.read_text(encoding="utf-8"))
```

### Exercises

1.  Remove duplicates while preserving order.
2.  Count word frequencies.
3.  Find common values using sets.
4.  Create a `Task` dataclass.
5.  Write a batch generator.
6.  Write a timing decorator.
7.  Read JSON and handle missing/invalid files.
8.  Explain shallow vs deep copy with a nested list.

### Mini-project 1 --- CLI task tracker

Add, list, complete and delete tasks. Persist them in JSON. Validate
inputs, handle corrupt files, separate functions, and document
setup/usage.

Acceptance: persistence works; invalid input is handled; code is
modular; sample run is documented.

# Section 2 --- NumPy (Days 6--7)

NumPy provides efficient multidimensional arrays. Understand `shape`,
`dtype`, indexing, axes, aggregation, broadcasting, boolean masks and
vectorisation.

``` python
import numpy as np
arr = np.array([[10, 20], [30, 40]], dtype=float)
print(arr.shape)
print(arr.mean(axis=0))  # column means
print(arr[arr > 20])
```

Broadcasting lets compatible shapes participate in operations without
manually repeating values. Vectorisation replaces many Python-level
loops with array operations.

### Mini-project 2 --- Sensor summary

Generate synthetic daily sensor readings. Report mean, median, standard
deviation, min/max, threshold violations and per-day summaries.

# Section 3 --- Pandas and cleaning (Days 8--9)

A Series is a labelled one-dimensional structure; a DataFrame is a
labelled table.

``` python
import pandas as pd
df = pd.DataFrame({
    "team": ["A", "B", "A"],
    "hours": [5, 7, None],
    "done": [True, True, False],
})
print(df.info())
df["hours"] = df["hours"].fillna(df["hours"].median())
print(df.groupby("team")["hours"].mean())
```

Know `loc` (labels), `iloc` (positions), `groupby`, `merge`, `concat`,
`isna`, `fillna`, `dropna`, `drop_duplicates`, and `to_datetime`.
Missing values require a reasoned decision; zero may be a valid
measurement.

### Mini-project 3 --- CSV cleaner

Use synthetic/public data. Profile types, missing values and duplicates;
standardise columns, parse dates, validate ranges, export clean data and
a quality report.

# Section 4 --- EDA and visualisation (Days 10--11)

EDA asks: what does each row represent, what are the units, what is
missing, how are values distributed, are categories imbalanced, and are
there time/group effects?

Use histograms for distributions, box plots for spread, bar charts for
categories, scatter plots for relationships and line charts for time
trends.

``` python
import matplotlib.pyplot as plt
df["hours"].hist(bins=10)
plt.title("Hours distribution")
plt.tight_layout()
plt.show()
```

Correlation does not prove causation. It can be driven by a third
variable or time trend.

### Mini-project 4 --- EDA report

Analyse a public/synthetic sales or service dataset. Create five useful
charts and a one-page report with findings and caveats.

# Section 5 --- Power BI basics (Day 12)

Learn import, Power Query transformations, table relationships, cards,
bar/line charts, slicers, measures and basic DAX.

``` dax
Total Sales = SUM(Sales[Amount])
```

A measure is calculated in the current filter context. A calculated
column is computed per row and stored. Start with a simple star schema:
fact table plus descriptive dimensions.

### Mini-project 5 --- Sales dashboard

Show total sales, order count, monthly trend, category breakdown and
region filter. Explain what decisions the visuals support.

# Section 6 --- HTTP, REST and FastAPI (Days 13--16)

HTTP uses requests and responses. Common methods: GET (retrieve), POST
(create/action), PUT (replace), PATCH (partial update), DELETE (remove).

Common status codes: 200 success, 201 created, 204 no body, 400 invalid
request, 401 unauthenticated, 403 forbidden, 404 missing, 422 validation
failure, 500 server error.

REST models resources and uses standard HTTP semantics. JSON is a common
payload format.

Install:

``` bash
python -m pip install fastapi uvicorn
```

``` python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, Field

app = FastAPI()

class TaskCreate(BaseModel):
    title: str = Field(min_length=1, max_length=100)
    priority: int = Field(ge=1, le=5)

tasks = {}

@app.post("/tasks", status_code=201)
def create_task(task: TaskCreate):
    task_id = len(tasks) + 1
    tasks[task_id] = task.model_dump()
    return {"id": task_id, **tasks[task_id]}

@app.get("/tasks/{task_id}")
def get_task(task_id: int):
    if task_id not in tasks:
        raise HTTPException(404, "Task not found")
    return {"id": task_id, **tasks[task_id]}
```

Run `uvicorn main:app --reload` and open `/docs` on the local server.
Learn path/query/body parameters, response schemas, dependency
injection, routers, error handling and tests. Pydantic validates input
against declared schemas.

### Mini-project 6 --- Task API

Implement CRUD endpoints, validation, correct status codes, Postman
examples and automated tests.

# Section 7 --- SQL and Python (Days 17--20)

``` sql
SELECT department, COUNT(*) AS total
FROM employees
WHERE active = TRUE
GROUP BY department
ORDER BY total DESC;
```

Learn SELECT/WHERE/ORDER BY, aggregates, GROUP BY/HAVING, INNER/LEFT
JOIN, subqueries, CTEs, window functions, primary/foreign keys, indexes
and transactions.

Use parameterised queries; never concatenate untrusted input into SQL.

``` python
import sqlite3
with sqlite3.connect("app.db") as con:
    con.execute("INSERT INTO tasks(title) VALUES (?)", ("Study SQL",))
    con.commit()
```

SQLAlchemy concepts: engine, session, model, transaction and migrations.
Keep credentials in environment variables.

### Mini-project 7 --- Persistent task API

Extend the FastAPI task manager to SQLite, then PostgreSQL if available.
Use SQLAlchemy, database sessions, validation and tests.

## Phase 1 assessment

Can you write Python functions/classes, clean and analyse data, explain
EDA, build/test a REST API, write joins/CTEs/window queries and connect
Python to a database?
