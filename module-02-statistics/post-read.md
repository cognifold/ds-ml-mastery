# Module 02 Post-Read: Statistics & Probability

## What You Built

In this module you applied classical statistical methods to WHO-style global
health data, discovering the striking inequality in life expectancy across
income groups. Concretely you practised:

- Computing and interpreting descriptive statistics (mean, median, IQR, std dev).
- Visualising distributions with histograms, box plots, and Q-Q plots.
- Applying probability rules and Bayes' theorem to diagnostic testing scenarios.
- Fitting and sampling from Normal, Binomial, and Poisson distributions.
- Running two-sample t-tests and chi-square tests and interpreting the results.
- Computing Pearson correlation and understanding its limitations.
- Calculating Cohen's d as an effect size alongside a p-value.

---

## Common Pitfalls

### 1. Reporting means without measures of spread
`"The average life expectancy is 72 years"` is almost meaningless without
context. Always report variability:
```
"Life expectancy: 72.1 ± 8.4 years (mean ± std dev)"
```
Better still, show the distribution visually.

### 2. Interpreting p < 0.05 as "important"
With n = 100 000 observations, even a difference of 0.01 years in life
expectancy will produce p < 0.0001. Always compute **Cohen's d** (or
equivalent) to judge practical significance:

| |d| | Interpretation |
|-----|----------------|
| 0.2 | Small effect |
| 0.5 | Medium effect |
| 0.8 | Large effect |

### 3. Using Pearson r on non-linear data
Pearson correlation only captures *linear* relationships. If a scatter plot
shows a clear curve, use Spearman rank correlation instead:

```python
from scipy.stats import spearmanr
r_s, p_s = spearmanr(x, y)   # monotonic (not just linear) relationship
```

### 4. Assuming normality without checking
Many statistical tests (t-test, ANOVA) assume the data are approximately
normal. Check with a Q-Q plot or Shapiro-Wilk test. For small non-normal
samples, use non-parametric alternatives:

| Parametric | Non-parametric alternative |
|------------|---------------------------|
| Two-sample t-test | Mann-Whitney U |
| One-way ANOVA | Kruskal-Wallis |
| Pearson r | Spearman ρ |

### 5. Multiple comparisons without correction
If you test 20 hypotheses at α = 0.05, you expect one false positive by
chance alone. Apply **Bonferroni correction** (divide α by number of tests)
or use the **Benjamini-Hochberg** false discovery rate procedure.

```python
from statsmodels.stats.multitest import multipletests
reject, p_adj, _, _ = multipletests(p_values, method='fdr_bh')
```

---

## Consolidation: The Statistical Mindset

Statistics is not a checklist of tests to run — it is a way of thinking about
uncertainty. Before choosing any test, ask:

1. **What type of data do I have?** Continuous, ordinal, categorical?
2. **What is my question?** Difference, association, prediction, description?
3. **What assumptions does my method require?** Can the data meet them?
4. **What will I do with the result?** A decision? A report? A model feature?

Answering these questions first will stop you from mechanically running a
t-test on count data or reporting a correlation as causation.

---

## Further Reading

1. **"Naked Statistics" (Charles Wheelan, Norton, 2013)** —
   The best non-technical introduction to statistical thinking. Chapters 2
   (mean/variance), 8 (hypothesis testing), and 10 (correlation) map
   directly to this module.

2. **"Statistics" (Freedman, Pisani, Purves, 4th ed.)** —
   The gold standard undergraduate statistics text. Rigorous but readable;
   Chapters 18–27 cover probability, sampling, and tests in depth.

3. **SciPy stats documentation** —
   Every distribution and test used in this module has a well-documented
   SciPy function. The scipy.stats subpackage reference is essential reading.

4. **"The Art of Statistics" (David Spiegelhalter, 2019)** —
   A Chief Statistician's guide to extracting meaning from data. Excellent
   treatment of p-values, confidence intervals, and the limits of statistical
   inference.

5. **WHO Global Health Observatory** —
   The real data source behind this module's case study. Interactive data
   explorer available at who.int/data/gho. Includes life expectancy,
   mortality, disease burden, and health system indicators for 194 countries.

6. **"Statistical Rethinking" (Richard McElreath, CRC Press, 2nd ed.)** —
   A Bayesian alternative perspective. Even if you work primarily with
   frequentist methods, Chapter 1 alone will reshape how you think about
   p-values and model uncertainty.
