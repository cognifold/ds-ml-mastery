# Contributing to DS/ML Mastery

Thank you for helping improve this course. This guide explains how to contribute
new modules, fix bugs, or improve existing content.

---

## Repository Layout

```
ds-ml-mastery/
├── COURSE_MAP.md          # Full curriculum overview
├── CONTRIBUTING.md        # This file
├── requirements.txt       # Python dependencies
├── .devcontainer/         # VS Code / Codespaces config
│   └── devcontainer.json
├── module-01-python/
│   ├── pre-read.md
│   ├── nyc_311_case_study.ipynb
│   ├── assignment.ipynb
│   ├── quiz.md
│   └── post-read.md
└── module-NN-<slug>/      # One directory per module
    └── ...
```

---

## Setting Up Locally

```bash
# 1. Clone the repo
git clone https://github.com/cognifold/ds-ml-mastery.git
cd ds-ml-mastery

# 2. Create a virtual environment
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch JupyterLab
jupyter lab
```

Alternatively, open the repo in VS Code and click **Reopen in Container** — the
`.devcontainer` config handles everything automatically.

---

## Module Content Standards

### pre-read.md
- Target length: 1 200–1 800 words.
- Must explain the *why* before the *how*.
- Include at least one worked example with code block.
- End with 3–5 "Key Takeaways".

### Case Study Notebook (`*_case_study.ipynb`)
- Use real or realistic open datasets.
- Every code cell must be preceded by a Markdown cell explaining intent.
- Avoid dead-end cells — every cell should produce visible output.
- Total run time < 3 minutes on a standard laptop CPU.

### Assignment Notebook (`assignment.ipynb`)
- Exactly **6 tasks** numbered Task 1 – Task 6.
- Each task: problem statement → starter code → one or more `assert` checks.
- Assert messages must be descriptive: `assert result == 42, "Expected 42, got {result}"`.
- Provide a `# YOUR CODE HERE` placeholder where learners write their solution.
- Tasks should increase in difficulty: Tasks 1–2 = recall, 3–4 = application,
  5–6 = synthesis/analysis.

### Quiz (`quiz.md`)
- Exactly **20 questions**, all multiple-choice (A/B/C/D).
- Cover the full module topic spread — not just the notebook examples.
- Include an **Answer Key** section at the bottom.
- Aim for a mix of conceptual (≈ 40 %) and applied (≈ 60 %) questions.

### post-read.md
- Consolidate the module in ≈ 600–900 words.
- "Common Pitfalls" section (at least 3 items).
- "Further Reading" section with 4–6 curated references (book chapters, papers,
  blog posts, docs).

---

## Branch Naming

| Type | Pattern | Example |
|------|---------|---------|
| New module | `module/NN-slug` | `module/05-eda` |
| Bug fix | `fix/short-description` | `fix/quiz-typos-m02` |
| Improvement | `improve/short-description` | `improve/m01-assignment-hints` |

---

## Pull Request Checklist

Before opening a PR, verify:

- [ ] All notebook cells run top-to-bottom without errors (`Kernel → Restart & Run All`).
- [ ] Assignment `assert` tests pass with a correct solution.
- [ ] Quiz has exactly 20 questions and an answer key.
- [ ] No large binary files are committed (use `.gitignore`).
- [ ] Markdown files render cleanly (check with a preview).
- [ ] `requirements.txt` is updated if new packages are introduced.

---

## Code Style

- Python inside notebooks: follow [PEP 8](https://peps.python.org/pep-0008/).
- Variable names should be descriptive (`life_expectancy_mean` not `lem`).
- Avoid global state between cells — define all helpers at the top of the relevant cell.

---

## Questions?

Open a GitHub Issue with the label `question`. We aim to respond within 48 hours.
