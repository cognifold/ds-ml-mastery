# Module 03 Pre-Read: Data Wrangling with Pandas

## What Is Data Wrangling?

Raw data is almost never analysis-ready. In practice, a data scientist spends
the majority of project time on **wrangling** — the work of reshaping, merging,
cleaning, and restructuring data until it answers the question you actually
have.

The US Census American Community Survey (ACS) is a perfect case study: it
ships as multiple separate tables (demographics, housing, income, geography)
that must be joined together before any insight is possible. The operations
you will learn in this module — merge, reshape, pivot, and melt — are the
vocabulary of that work.

---

## 1. Merging and Joining DataFrames

Pandas `.merge()` mirrors SQL JOINs. Understanding which type to use is the
single most important merging decision.

| Join type | Rows kept | Use when |
|-----------|-----------|----------|
| `inner` | Only rows with keys in **both** tables | You need complete records |
| `left` | All rows from the left table | Left table is the "base" |
| `right` | All rows from the right table | Right table is the "base" |
| `outer` | All rows from **both** tables | You cannot afford to lose any data |

```python
import pandas as pd

# Demographics table (one row per county)
demographics = pd.DataFrame({
    'county_id': ['01001', '01003', '01005', '01007'],
    'population': [55_869, 231_767, 24_686, 21_892],
    'median_age': [39.2, 41.1, 38.7, 37.5],
})

# Income table (may not cover all counties)
income = pd.DataFrame({
    'county_id': ['01001', '01003', '01009'],   # 01005, 01007 missing
    'median_household_income': [51_200, 65_400, 44_800],
})

# Inner join: only counties present in BOTH tables
inner = demographics.merge(income, on='county_id', how='inner')
print(f'Inner: {len(inner)} rows')  # 2 rows (01001, 01003)

# Left join: keep all counties, NaN where income data is missing
left = demographics.merge(income, on='county_id', how='left')
print(f'Left:  {len(left)} rows')   # 4 rows
print(left[left['median_household_income'].isna()])
```

### Merging on multiple keys

```python
# When the join key spans two columns
merged = df_a.merge(df_b, on=['state_fips', 'county_fips'], how='left')
```

### Diagnosing merge problems

Always check the row count after a merge. A count that is higher than either
input usually means a **many-to-many merge** — each key appears multiple times
in both tables, causing a cartesian explosion.

```python
# Before merging, check for duplicate keys
assert demographics['county_id'].is_unique, "Key has duplicates in demographics"
assert income['county_id'].is_unique, "Key has duplicates in income"
```

---

## 2. Reshaping: Wide vs Long Format

Data arrives in two shapes:

- **Wide format** — one row per subject, one column per time point or variable.
  Easy for humans to read; harder for Pandas to analyse.
- **Long (tidy) format** — one row per observation, columns for subject,
  variable name, and value. Required by most plotting libraries and many
  statistical methods.

### `.melt()` — wide → long

```python
# Wide: one column per year
wide = pd.DataFrame({
    'county':    ['Autauga', 'Baldwin', 'Barbour'],
    'pop_2010':  [54_571, 182_265, 27_457],
    'pop_2015':  [55_395, 203_709, 25_976],
    'pop_2020':  [58_805, 231_767, 24_686],
})

long = wide.melt(
    id_vars='county',
    value_vars=['pop_2010', 'pop_2015', 'pop_2020'],
    var_name='year_col',
    value_name='population',
)
long['year'] = long['year_col'].str.extract(r'(\d{4})').astype(int)
long = long.drop('year_col', axis=1)
print(long.head(6))
```

### `.pivot_table()` — long → wide (with aggregation)

```python
# Summarise: mean income by state and education level
pivot = df.pivot_table(
    values='median_income',
    index='state',
    columns='education_level',
    aggfunc='mean',
    fill_value=0,
)
```

### `.pivot()` — long → wide (no aggregation, requires unique keys)

```python
# Reshape when keys are already unique
wide = long.pivot(index='county', columns='year', values='population')
```

---

## 3. Handling Missing Data

Missing data is not a single problem — it has three distinct causes that
require different responses.

| Missing type | Cause | Strategy |
|--------------|-------|----------|
| MCAR (Missing Completely At Random) | Random dropout, data entry | Drop rows or impute with mean/median |
| MAR (Missing At Random) | Missingness depends on observed data | Impute using a model or group median |
| MNAR (Missing Not At Random) | Missingness related to the missing value itself | Flag as a separate category; do not simply impute |

```python
# Check missing patterns
missing_pct = df.isnull().mean().sort_values(ascending=False) * 100
print(missing_pct[missing_pct > 0])

# Drop columns with > 40% missing
df = df.loc[:, df.isnull().mean() < 0.40]

# Impute numeric columns with column median
for col in df.select_dtypes(include='number').columns:
    df[col] = df[col].fillna(df[col].median())
```

---

## 4. String Operations with `.str`

The `.str` accessor gives you vectorised string methods on any object-typed
column — no loop required.

```python
# Normalise county names
df['county'] = df['county'].str.strip().str.title()

# Extract state abbreviation from "county, ST" format
df['state'] = df['county_state'].str.split(',').str[-1].str.strip()

# Boolean filter: counties whose name contains "Washington"
washington_counties = df[df['county'].str.contains('Washington', case=False)]

# Pad FIPS codes to 5 digits (e.g. "1001" → "01001")
df['fips'] = df['fips'].astype(str).str.zfill(5)
```

---

## 5. The `.pipe()` Pattern — Readable Pipelines

When you chain many transformations, `.pipe()` lets you name each step as a
function and read the pipeline top-to-bottom:

```python
def drop_low_population(df, min_pop=1000):
    return df[df['population'] >= min_pop]

def add_density(df):
    df = df.copy()
    df['pop_density'] = df['population'] / df['land_area_sq_km']
    return df

def normalise_names(df):
    df = df.copy()
    df['county'] = df['county'].str.strip().str.title()
    return df

clean = (
    raw_df
    .pipe(normalise_names)
    .pipe(drop_low_population, min_pop=5000)
    .pipe(add_density)
)
```

Each function takes a DataFrame and returns a DataFrame. The result is
auditable, testable, and easy to re-order.

---

## 6. Aggregation with `groupby` + `agg`

Beyond simple `.count()` and `.mean()`, `agg` lets you compute multiple
statistics in one pass:

```python
summary = (
    df.groupby('state')
    .agg(
        n_counties       = ('county_id', 'count'),
        total_population = ('population', 'sum'),
        mean_income      = ('median_household_income', 'mean'),
        median_income    = ('median_household_income', 'median'),
        max_income       = ('median_household_income', 'max'),
    )
    .round(0)
    .sort_values('total_population', ascending=False)
)
```

Named aggregations (the `col = (source, func)` syntax) give clean output
column names without a subsequent `.rename()` call.

---

## Key Takeaways

1. **Check row counts before and after every merge** — unexpected row
   multiplication is the most silent and destructive merging bug.
2. **Prefer left joins** when you have a canonical "base" table — it makes
   missing matches visible as NaN rather than silently dropping rows.
3. **Tidy data = one observation per row** — get your data into long format
   before plotting or modelling; `.melt()` is the tool for this.
4. **Classify missing data before imputing** — imputing MNAR data with the
   column mean introduces systematic bias.
5. **Use `.pipe()` for multi-step cleaning** — it turns a wall of chained
   operations into a readable, testable sequence of named steps.
