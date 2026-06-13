# Module 01 Pre-Read: Python for Data Science

## Why Python?

Python did not win the data science world by accident. In the early 2010s,
data scientists were splitting their time between R (for statistics), MATLAB
(for numerical work), and shell scripts (for glue). Python offered something
none of those could: a single, readable language that could handle every step
of a project — ingestion, cleaning, modelling, visualisation, and deployment —
without switching tools.

Today, NumPy and Pandas underpin virtually every data science library in
existence. Understanding them at a mechanical level is the difference between
writing code that works and writing code you understand.

---

## 1. NumPy — Thinking in Arrays

The central insight of NumPy is **vectorised computation**: instead of looping
over elements one at a time, you express operations on entire arrays, and NumPy
executes them in compiled C code underneath.

```python
import numpy as np

# Pure Python — slow for large N
temperatures_list = [22.1, 19.4, 28.3, 31.0, 17.6]
fahrenheit_list = [(t * 9/5) + 32 for t in temperatures_list]

# NumPy — fast, concise, identical result
temperatures = np.array([22.1, 19.4, 28.3, 31.0, 17.6])
fahrenheit = (temperatures * 9/5) + 32
```

The second form is not just shorter — on arrays of a million elements it can
be **50–100× faster** than the Python loop because there is no interpreter
overhead per element.

### Key array concepts

| Concept | What it means | Example |
|---------|---------------|---------|
| `dtype` | The data type stored in every cell | `np.float64`, `np.int32` |
| `shape` | Dimensions as a tuple | `(1000,)`, `(100, 5)` |
| `axis` | The direction of an operation | `axis=0` = along rows |
| Broadcasting | Automatic shape alignment for arithmetic | `arr + 1` adds 1 to every element |

### Boolean indexing — the pattern you'll use every day

```python
ages = np.array([23, 45, 18, 67, 31, 55])

# Which ages are over 30?
mask = ages > 30               # array([False, True, False, True, True, True])
older_than_30 = ages[mask]     # array([45, 67, 31, 55])

# One-liner form
older_than_30 = ages[ages > 30]
```

---

## 2. Pandas — Labelled, Tabular Data

Where NumPy excels at numeric arrays, Pandas excels at **heterogeneous tables**:
columns with different types, meaningful row and column labels, and operations
that mirror what analysts do in spreadsheets — but at scale.

### The two core objects

**Series** — a 1-D labelled array (like a single spreadsheet column):

```python
import pandas as pd

population = pd.Series(
    [8_336_817, 2_693_976, 1_304_191],
    index=["New York City", "Brooklyn", "Queens"]
)
print(population["Brooklyn"])   # 2693976
```

**DataFrame** — a 2-D table of Series sharing an index (like a spreadsheet):

```python
df = pd.DataFrame({
    "borough": ["Manhattan", "Brooklyn", "Queens", "Bronx", "Staten Island"],
    "population": [1_629_153, 2_693_976, 2_405_464, 1_472_654, 495_747],
    "area_sq_km": [59.1, 183.0, 281.1, 109.0, 151.4],
})
df["density"] = df["population"] / df["area_sq_km"]
```

### Reading data

```python
# CSV (most common)
df = pd.read_csv("data.csv")

# With options
df = pd.read_csv(
    "311_service_requests.csv",
    parse_dates=["Created Date"],
    low_memory=False,
)
```

### Selecting data — three patterns

```python
# 1. Column selection
complaint_types = df["Complaint Type"]

# 2. .loc — label-based (rows and columns by name)
subset = df.loc[df["Borough"] == "BROOKLYN", ["Complaint Type", "Created Date"]]

# 3. .iloc — position-based (rows and columns by integer)
first_five = df.iloc[:5, :3]
```

### GroupBy — the workhorse of tabular analysis

```python
# How many complaints per borough?
complaint_counts = (
    df.groupby("Borough")["Unique Key"]
    .count()
    .sort_values(ascending=False)
)
```

`groupby` follows a **split → apply → combine** pattern:
1. **Split** the DataFrame into groups by `"Borough"`.
2. **Apply** `.count()` to each group.
3. **Combine** results back into a single Series.

---

## 3. Functions and Code Organisation

Data science scripts quickly become notebooks full of copy-pasted blocks.
Functions are the antidote.

```python
def clean_borough_name(name: str) -> str:
    """Normalise a borough string to title case, handling NaN."""
    if pd.isna(name):
        return "Unknown"
    return name.strip().title()

df["Borough"] = df["Borough"].apply(clean_borough_name)
```

A well-named function is self-documenting. Prefer short, single-purpose
functions over long procedural blocks.

---

## 4. List Comprehensions and Generator Expressions

Comprehensions are idiomatic Python for building collections:

```python
# All unique complaint types that mention "noise"
noise_types = [t for t in df["Complaint Type"].unique() if "noise" in t.lower()]

# Dictionary mapping borough name → complaint count (from a groupby result)
borough_counts = {borough: count for borough, count in complaint_counts.items()}
```

Prefer comprehensions over `map`/`filter` for readability. Use generator
expressions (`(... for ...)`) when you only need to iterate once and don't
need to store the list.

---

## 5. The Data Science Workflow in Python

Every project follows roughly the same sequence:

```
Load → Inspect → Clean → Explore → Answer → Communicate
```

| Step | Key tools |
|------|-----------|
| Load | `pd.read_csv`, `pd.read_json`, `pd.read_sql` |
| Inspect | `.shape`, `.dtypes`, `.head()`, `.info()`, `.describe()` |
| Clean | `.dropna()`, `.fillna()`, `.astype()`, `.str` accessor |
| Explore | `.groupby()`, `.value_counts()`, `.corr()`, `matplotlib` |
| Answer | Slicing, aggregation, statistical tests |
| Communicate | Markdown, charts, summary DataFrames |

---

## Key Takeaways

1. **Use NumPy arrays for numeric computation** — vectorised operations are
   orders of magnitude faster than Python loops.
2. **Use Pandas DataFrames for tabular data** — labels, mixed types, and
   missing-value handling are built in.
3. **`.loc` selects by label; `.iloc` selects by position** — mixing them up is
   the single most common beginner error.
4. **GroupBy = split → apply → combine** — master this pattern and most
   aggregation problems become trivial.
5. **Write functions early** — even a three-line helper function will save you
   hours of debugging copy-pasted code.
