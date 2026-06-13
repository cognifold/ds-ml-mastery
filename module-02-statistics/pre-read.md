# Module 02 Pre-Read: Statistics & Probability

## Why Statistics?

A model trained on data is only as trustworthy as the analysis behind it.
Before any machine learning, every experienced data scientist asks:
*What does the data actually look like? Is this difference real or noise?
How certain can I be?* Those questions are statistics.

This module builds the statistical vocabulary you will reach for in every
subsequent module — from spotting skewed distributions in feature engineering
to interpreting p-values in A/B tests to understanding why regularisation
works.

---

## 1. Descriptive Statistics — Summarising What You Have

Descriptive statistics compress a column of thousands of numbers into a
handful of values you can reason about.

### Measures of centre

| Statistic | Formula | When to use |
|-----------|---------|-------------|
| Mean (μ) | Σx / n | Symmetric distributions, no severe outliers |
| Median | Middle value when sorted | Skewed data, outliers present |
| Mode | Most frequent value | Categorical data or heavily peaked distributions |

**Example — life expectancy:**

```python
import numpy as np

# Simplified global life expectancy (years) for 10 countries
life_exp = np.array([53.1, 62.4, 71.8, 76.2, 80.5, 72.1, 65.3, 83.4, 58.9, 77.6])

print(f"Mean:   {np.mean(life_exp):.1f}")   # 70.1
print(f"Median: {np.median(life_exp):.1f}") # 71.9
```

When mean ≠ median, the distribution is **skewed**. A mean far above the
median indicates a right skew (a few very high values pulling the average up) —
common in income, wealth, and healthcare cost data.

### Measures of spread

| Statistic | Meaning |
|-----------|---------|
| Variance (σ²) | Average squared deviation from the mean |
| Standard deviation (σ) | Square root of variance — in the original unit |
| IQR | Q3 − Q1 — the range of the middle 50% |
| Range | Max − Min — sensitive to outliers |

```python
print(f"Std dev: {np.std(life_exp, ddof=1):.1f}")  # sample std dev, ddof=1
print(f"IQR:     {np.percentile(life_exp, 75) - np.percentile(life_exp, 25):.1f}")
```

> **ddof=1** uses the *sample* standard deviation formula (divides by n−1),
> appropriate when your data is a sample from a larger population. NumPy's
> default `ddof=0` gives the *population* std dev.

---

## 2. Probability — Formalising Uncertainty

A **probability** is a number between 0 and 1 expressing how likely an event
is. The rules are simple; the applications are not.

### Core rules

| Rule | Formula | Reading |
|------|---------|---------|
| Complement | P(not A) = 1 − P(A) | If a disease has 5% prevalence, 95% of people don't have it |
| Addition (mutually exclusive) | P(A or B) = P(A) + P(B) | P(rolling 1 or 2) = 1/6 + 1/6 |
| Multiplication (independent) | P(A and B) = P(A) × P(B) | P(two heads) = 0.5 × 0.5 |
| Conditional | P(A | B) = P(A and B) / P(B) | Probability of A given we know B occurred |

### Bayes' Theorem

Perhaps the most important equation in statistics for data scientists:

```
P(A | B) = [P(B | A) × P(A)] / P(B)
```

**Medical example:** A disease has 1% prevalence (P(disease) = 0.01).
A test is 95% sensitive (P(positive | disease) = 0.95) and 90% specific
(P(negative | no disease) = 0.90, so P(positive | no disease) = 0.10).

What is P(disease | positive test)?

```python
p_disease = 0.01
p_pos_given_disease = 0.95
p_pos_given_no_disease = 0.10

p_positive = p_pos_given_disease * p_disease + p_pos_given_no_disease * (1 - p_disease)
p_disease_given_pos = (p_pos_given_disease * p_disease) / p_positive

print(f"P(disease | positive test) = {p_disease_given_pos:.1%}")  # ≈ 8.7%
```

Only ~8.7% — far lower than most people's intuition. This is the **base rate
fallacy**: ignoring how rare the disease is dramatically inflates perceived risk.

---

## 3. Probability Distributions

A distribution describes how probability is spread across all possible values
of a variable.

### Normal (Gaussian) distribution

The bell curve. Defined entirely by mean (μ) and std dev (σ).

**68–95–99.7 rule:**
- 68% of data falls within ±1σ of the mean
- 95% within ±2σ
- 99.7% within ±3σ

```python
from scipy import stats

# Is a country with life expectancy of 85 years unusual?
mu, sigma = 72.0, 8.0
z_score = (85 - mu) / sigma   # 1.625
p_above = 1 - stats.norm.cdf(z_score)
print(f"P(life expectancy > 85): {p_above:.1%}")  # ~5.2%
```

### Binomial distribution

Models the number of successes in n independent trials, each with
probability p of success.

```python
# Probability of ≥ 3 countries in a sample of 10 having life exp > 80
n, p = 10, 0.20   # ~20% of countries have life exp > 80
print(f"P(X >= 3): {1 - stats.binom.cdf(2, n, p):.1%}")  # ~32%
```

### Poisson distribution

Models the number of events in a fixed time/space interval when events
occur at a known average rate.

```python
# Hospital receives avg 4 emergency admissions per hour. P(exactly 6)?
lam = 4
print(f"P(X = 6): {stats.poisson.pmf(6, lam):.1%}")  # ~10.4%
```

---

## 4. Hypothesis Testing

Hypothesis testing is the formal procedure for deciding whether an observed
difference in data is real or due to chance.

### The framework

1. **Null hypothesis (H₀):** "There is no effect / no difference."
2. **Alternative hypothesis (H₁):** "There is an effect / a difference."
3. **Test statistic:** A number computed from your data.
4. **p-value:** Probability of seeing data *at least as extreme* as yours
   if H₀ were true.
5. **Decision:** If p-value < α (usually 0.05), reject H₀.

### Two-sample t-test

Is the mean life expectancy in high-income countries different from
low-income countries?

```python
high_income = np.array([80.2, 81.5, 79.8, 83.1, 78.4, 82.0, 77.9, 81.2])
low_income  = np.array([58.3, 62.1, 55.7, 60.4, 57.9, 63.2, 59.8, 56.4])

t_stat, p_value = stats.ttest_ind(high_income, low_income)
print(f"t = {t_stat:.2f}, p = {p_value:.4f}")
# p << 0.05 → reject H₀ → real difference
```

### What p-values are NOT

- **Not** the probability that H₀ is true.
- **Not** the probability your result was due to chance.
- **Not** a measure of effect size or practical significance.

A large dataset can produce a tiny p-value for a completely trivial
difference. Always pair p-values with **effect size** (e.g. Cohen's d).

---

## 5. Correlation

Correlation measures the **linear** relationship between two numeric variables,
ranging from −1 (perfect negative) to +1 (perfect positive), with 0 meaning
no linear relationship.

```python
gdp_per_capita    = np.array([1200, 3400, 8900, 15000, 28000, 42000, 56000])
life_expectancy   = np.array([55.1, 62.3, 68.9, 72.4, 77.8, 80.1, 82.3])

r, p_val = stats.pearsonr(gdp_per_capita, life_expectancy)
print(f"r = {r:.3f}, p = {p_val:.4f}")   # strong positive correlation
```

> **Correlation ≠ causation.** GDP correlates with life expectancy, but
> many confounders exist (healthcare access, nutrition, sanitation). A high
> correlation is a clue to investigate, not a conclusion.

---

## Key Takeaways

1. **Always start with the mean AND median** — if they diverge, your distribution
   is skewed and the mean may be misleading.
2. **Standard deviation is the mean's natural companion** — always report them
   together (e.g. "72.1 ± 8.4 years").
3. **Bayes' theorem matters in healthcare** — low base rates can make even
   accurate tests produce mostly false positives.
4. **p < 0.05 is not the finish line** — report effect sizes and confidence
   intervals alongside p-values.
5. **Correlation measures linear association only** — Pearson r will be 0 for
   a perfect U-shaped relationship.
