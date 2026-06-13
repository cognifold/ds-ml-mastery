# Module 01 Post-Read: Python for Data Science

## What You Built

In this module you loaded a 1 million-row slice of the NYC 311 dataset,
cleaned borough names, parsed date columns, and answered four real questions
about complaint patterns — all with fewer than 80 lines of Pandas code.
Along the way you practised:

- Reading CSV files with `pd.read_csv` and sensible options.
- Inspecting a DataFrame with `.info()`, `.describe()`, and `.value_counts()`.
- Filtering rows with boolean masks and `.loc`.
- Aggregating with `.groupby()` and sorting results.
- Extracting date parts with `.dt` accessors.
- Writing reusable helper functions.

---

## Common Pitfalls

### 1. Chained indexing (`df["col"][mask]`)
Always use `.loc` for combined label + boolean selection:

```python
# BAD — may silently return a copy and not raise an error
df["Status"][df["Borough"] == "BROOKLYN"] = "Closed"

# GOOD
df.loc[df["Borough"] == "BROOKLYN", "Status"] = "Closed"
```

Pandas will warn you with `SettingWithCopyWarning` if you trigger chained
indexing, but the behaviour can still be surprising.

### 2. Forgetting `parse_dates`
Date columns loaded without `parse_dates=["col"]` arrive as `object` (string)
dtype. Arithmetic like `df["Closed Date"] - df["Created Date"]` will silently
fail or raise a `TypeError`. Always specify date columns at load time.

### 3. Assuming `.groupby()` preserves order
`groupby` does not guarantee output order. Always add `.sort_values()` or
`.sort_index()` after aggregation if order matters for your analysis.

### 4. Using loops where vectorisation works
```python
# BAD — O(n) Python loop, slow on large DataFrames
results = []
for i, row in df.iterrows():
    results.append(row["value"] * 2)

# GOOD — vectorised, fast
results = df["value"] * 2
```

`.iterrows()` has its place (complex multi-column logic that can't be
vectorised), but it should be a last resort.

### 5. `inplace=True` surprises
Many Pandas methods accept `inplace=True`, but it does not always behave as
expected and it prevents method chaining. Prefer reassignment:

```python
# Prefer this
df = df.dropna(subset=["Complaint Type"])

# Over this
df.dropna(subset=["Complaint Type"], inplace=True)
```

---

## Consolidation: The Mental Model

Think of a Pandas `DataFrame` as a dictionary of NumPy arrays sharing a
common index. Every column operation is a vectorised NumPy operation in
disguise. Every GroupBy is a loop you didn't have to write. The job of a
good data scientist is to express data transformations at the *column* level,
not the *row* level.

---

## Further Reading

1. **"Python for Data Analysis" (Wes McKinney, O'Reilly, 3rd ed.)** —
   Written by the creator of Pandas. Chapters 5–10 cover everything in this
   module in depth.

2. **Pandas official documentation — "10 minutes to pandas"** —
   The canonical quick-start tutorial. Covers indexing, groupby, and reshaping
   with runnable examples.

3. **NumPy official documentation — "NumPy: the absolute basics"** —
   Authoritative coverage of dtypes, broadcasting, and array creation routines.

4. **"Effective Pandas" (Matt Harrison, 2021)** —
   A concise practitioner's guide focused on the patterns experienced users
   actually reach for, including method chaining and the `.pipe()` API.

5. **NYC Open Data — 311 Service Requests** —
   The real dataset used in this module. Explore it further at
   nyc.gov/opendata. It is updated daily and extends back to 2010.

6. **Jake VanderPlas — "Python Data Science Handbook" (free online)** —
   Chapters 2 (NumPy) and 3 (Pandas) are excellent complements to this
   module, with detailed explanations of advanced indexing and broadcasting.
