# Module 02 Quiz: Statistics & Probability

**Instructions:** Choose the single best answer for each question.

---

**Q1.** A dataset of national GDP values has mean $28 000 and median $12 000. The distribution is most likely:

A) Symmetric  
B) Left-skewed (negatively skewed)  
C) Right-skewed (positively skewed)  
D) Bimodal  

---

**Q2.** You compute `np.std(arr, ddof=1)`. What does `ddof=1` do?

A) Adds 1 to the mean before computing std dev  
B) Divides by (n − 1) instead of n, giving the sample standard deviation  
C) Uses the Bessel correction for population std dev  
D) Multiplies the result by 1 to avoid negative values  

---

**Q3.** Which measure of spread is *least* sensitive to extreme outliers?

A) Range  
B) Variance  
C) Standard deviation  
D) Interquartile range (IQR)  

---

**Q4.** A disease has 2% prevalence. A test has 90% sensitivity and 85% specificity. Which statement correctly applies Bayes' Theorem reasoning?

A) A positive test means 90% chance of disease  
B) The positive predictive value will be much lower than 90% due to the low base rate  
C) Specificity is irrelevant when prevalence is low  
D) The p-value of the test result determines disease probability  

---

**Q5.** For a normally distributed variable, approximately what percentage of values fall within ±2 standard deviations of the mean?

A) 68%  
B) 90%  
C) 95%  
D) 99.7%  

---

**Q6.** `scipy.stats.ttest_ind(group_a, group_b)` tests:

A) Whether the variances of two groups are equal  
B) Whether the means of two independent groups differ significantly  
C) Whether two variables are normally distributed  
D) Whether two paired samples have the same distribution  

---

**Q7.** A p-value of 0.03 means:

A) There is a 3% probability that the null hypothesis is true  
B) There is a 3% chance the result is due to random variation  
C) If H₀ were true, the probability of observing data at least this extreme is 3%  
D) The effect size is small (d ≈ 0.03)  

---

**Q8.** Which distribution is most appropriate for modelling the number of disease outbreaks in a given week, given a known historical average rate?

A) Normal  
B) Binomial  
C) Poisson  
D) Uniform  

---

**Q9.** Pearson's r = −0.85 between two health variables means:

A) A 1-unit increase in X causes a 0.85-unit decrease in Y  
B) 85% of the variance in Y is explained by X  
C) There is a strong negative *linear* association between X and Y  
D) The two variables are independent  

---

**Q10.** You want to test if the distribution of disease outcomes (recovered / chronic / deceased) differs between two age groups. The best test is:

A) Two-sample t-test  
B) Pearson correlation  
C) Chi-square test of independence  
D) One-sample z-test  

---

**Q11.** Cohen's d = 1.2 between two groups indicates:

A) The p-value is 0.12  
B) A large standardised difference between the two group means  
C) The correlation between the groups is 0.12  
D) A small, practically insignificant effect  

---

**Q12.** The Z-score of a value x in a distribution with mean μ and std dev σ is:

A) (x − σ) / μ  
B) (μ − x) / σ  
C) (x − μ) / σ  
D) (x × μ) / σ  

---

**Q13.** If P(A) = 0.4 and P(B) = 0.3, and A and B are **independent**, then P(A and B) equals:

A) 0.70  
B) 0.10  
C) 0.12  
D) 0.58  

---

**Q14.** Which plot is best for visually checking whether a variable is approximately normally distributed?

A) Bar chart  
B) Scatter plot  
C) Q-Q (quantile-quantile) plot  
D) Pie chart  

---

**Q15.** You test 50 hypotheses simultaneously at α = 0.05. About how many false positives would you expect by chance?

A) 0  
B) 1  
C) 2.5  
D) 5  

---

**Q16.** The Binomial distribution B(n=20, p=0.3) models:

A) The waiting time until the first success  
B) The number of successes in 20 independent trials each with 30% success probability  
C) The number of events per unit time at rate 0.3  
D) A continuous probability distribution on [0, 1]  

---

**Q17.** `scipy.stats.norm.cdf(1.96)` returns approximately:

A) 0.025  
B) 0.95  
C) 0.975  
D) 1.96  

---

**Q18.** Spearman rank correlation should be preferred over Pearson r when:

A) The sample size is very large (n > 10 000)  
B) The relationship between variables is monotonic but not linear, or outliers are present  
C) Both variables are normally distributed  
D) You want to compute p-values  

---

**Q19.** A two-sample t-test returns t = 4.7, p = 0.0001, but Cohen's d = 0.18. The most accurate interpretation is:

A) The effect is large and practically meaningful  
B) The p-value is too small to trust  
C) The difference is statistically significant but the effect size is small  
D) The test was applied incorrectly  

---

**Q20.** The 68–95–99.7 rule states that for a normal distribution, 99.7% of values fall within:

A) ±1 standard deviation of the mean  
B) ±2 standard deviations of the mean  
C) ±3 standard deviations of the mean  
D) ±4 standard deviations of the mean  

---

## Answer Key

| Q | Answer | Q | Answer |
|---|--------|---|--------|
| 1 | C | 11 | B |
| 2 | B | 12 | C |
| 3 | D | 13 | C |
| 4 | B | 14 | C |
| 5 | C | 15 | C |
| 6 | B | 16 | B |
| 7 | C | 17 | C |
| 8 | C | 18 | B |
| 9 | C | 19 | C |
| 10 | C | 20 | C |
