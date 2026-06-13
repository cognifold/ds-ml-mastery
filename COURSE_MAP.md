# DS/ML Mastery — Course Map

A hands-on, project-driven curriculum that takes learners from Python fundamentals
to production-ready machine learning. Every module follows the same rhythm:
**pre-read → case study notebook → assignment → quiz → post-read**.

---

## Phase 1 — Foundations (Modules 01–04)

| # | Module | Case Study Dataset | Key Skills |
|---|--------|--------------------|------------|
| 01 | Python for Data Science | NYC 311 Service Requests | NumPy, Pandas I/O, list comprehensions, functions |
| 02 | Statistics & Probability | WHO Global Health Observatory | Descriptive stats, distributions, hypothesis testing |
| 03 | Data Wrangling with Pandas | US Census ACS | Merge/join, reshape, missing-data strategies |
| 04 | Data Visualization | Gapminder | Matplotlib, Seaborn, chart selection, storytelling |

---

## Phase 2 — Core Data Science (Modules 05–08)

| # | Module | Case Study Dataset | Key Skills |
|---|--------|--------------------|------------|
| 05 | Exploratory Data Analysis | Airbnb Listings | EDA workflow, outlier detection, feature distributions |
| 06 | Feature Engineering | Bike Share DC | Encoding, scaling, datetime features, pipelines |
| 07 | SQL for Data Science | Northwind Database | SELECT, JOIN, aggregation, window functions |
| 08 | Communicating with Data | Climate NOAA | Narrative structure, Plotly interactive charts |

---

## Phase 3 — ML Core (Modules 09–13)

| # | Module | Case Study Dataset | Key Skills |
|---|--------|--------------------|------------|
| 09 | Intro to Machine Learning | California Housing | Scikit-learn API, train/test split, cross-validation |
| 10 | Linear & Logistic Regression | Heart Disease UCI | OLS, regularisation (Ridge/Lasso), logistic loss |
| 11 | Decision Trees & Ensembles | Telco Churn | Random Forest, Gradient Boosting, feature importance |
| 12 | Model Evaluation & Tuning | Credit Card Fraud | Metrics (AUC, F1, MCC), GridSearchCV, imbalanced data |
| 13 | Unsupervised Learning | Mall Customer Segments | K-Means, DBSCAN, PCA, silhouette score |

---

## Phase 4 — Advanced ML (Modules 14–17)

| # | Module | Case Study Dataset | Key Skills |
|---|--------|--------------------|------------|
| 14 | Natural Language Processing | Amazon Reviews | TF-IDF, word embeddings, sentiment classification |
| 15 | Time Series Forecasting | Store Sales | ARIMA, ETS, Prophet, feature-based forecasting |
| 16 | Neural Networks & Deep Learning | MNIST / CIFAR-10 | Keras, CNNs, transfer learning |
| 17 | Recommender Systems | MovieLens | Collaborative filtering, matrix factorisation |

---

## Phase 5 — Real-World Practice (Modules 18–20)

| # | Module | Case Study Dataset | Key Skills |
|---|--------|--------------------|------------|
| 18 | ML Pipelines & MLOps | End-to-end churn model | scikit-learn Pipelines, MLflow tracking |
| 19 | Ethics, Fairness & Interpretability | COMPAS / Lending | SHAP, Fairlearn, bias auditing |
| 20 | Capstone Project | Learner's choice | Full DS/ML lifecycle: problem → model → presentation |

---

## Module Structure

Every module contains the following artefacts:

```
module-NN-<slug>/
├── pre-read.md          # Conceptual framing (≈ 1 500 words)
├── <topic>_case_study.ipynb   # Guided notebook with narrative
├── assignment.ipynb     # 6 auto-graded tasks with assert tests
├── quiz.md              # 20 multiple-choice questions + answer key
└── post-read.md         # Consolidation + curated further reading
```

---

## Learning Outcomes

By the end of Phase 1, learners can wrangle real-world tabular data and apply
statistical reasoning to draw defensible conclusions.

By the end of Phase 3, learners can build, evaluate, and tune supervised ML
models using the full scikit-learn workflow.

By the end of Phase 5, learners can design end-to-end ML systems, reason about
fairness, and communicate results to non-technical stakeholders.
