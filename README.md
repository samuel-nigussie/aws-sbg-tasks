# AWS Student Builder Group (SBG) - Weekly Assignments

A repository containing weekly practical coding tasks, assignments, and data engineering projects developed for the AWS Student Builder Group (SBG) program.

[![CI Pipeline](https://github.com/samuel-nigussie/aws-sbg-tasks/actions/workflows/ci.yml/badge.svg)](https://github.com/samuel-nigussie/aws-sbg-tasks/actions/workflows/ci.yml)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![NumPy](https://img.shields.io/badge/NumPy-Vectorized-013243?logo=numpy)](https://numpy.org/)

---

## 📁 Repository Structure

The repository structure reflects the contents outlined in the provided image.

```text
aws-sbg-tasks/
├── .gitignore                    # Ignored virtual environments, checkpoints, and caches
├── week-01/                      # Week 01: Diabetes Data Processor
│   ├── Diabetes_Data_Processor.ipynb  # Executed Jupyter notebook with data analysis
│   ├── README.md                 # Week 01 documentation
│   ├── cleaned_diabetes.csv      # Output cleaned & enriched data
│   └── requirements.txt          # Dependencies specific to Week 01
└── week-02/                      # Week 02: Mini Semantic Search
    ├── test/                     # Automated unit tests for Week 02
    ├── README.md                 # Week 02 documentation
    ├── mini_semantic_search.ipynb # Executed Jupyter notebook with outputs
    ├── mini_semantic_search.py   # Core semantic search engine script
    └── requirements.txt          # Dependencies specific to Week 02
```

---

## 📅 Weekly Task Index

| Week | Task Title | Primary File | Interactive Notebook | Status |
| :---: | :--- | :--- | :--- | :---: |
| **01** | **Diabetes Data Processor** | `cleaned_diabetes.csv` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/samuel-nigussie/aws-sbg-tasks/blob/main/week-01/Diabetes_Data_Processor.ipynb) | ✅ Completed |
| **02** | **Mini Semantic Search** | `mini_semantic_search.py` | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/samuel-nigussie/aws-sbg-tasks/blob/main/week-02/mini_semantic_search.ipynb) | ✅ Completed |
| **03** | *Pending Assignment* | TBD | TBD | ⏳ Upcoming |

---

## 📌 Submission Standards & Best Practices

*   **Modular Architecture**: Each task is contained within its own isolated directory (`week-01/`, `week-02/`) featuring dedicated documentation and dependency requirements.
*   **Reproducibility**: Weekly tasks include executed Jupyter Notebooks detailing cell outputs, data visualizations, and systematic processing steps.
*   **Cloud Accessibility**: Google Colab integration enables zero-install browser execution for all primary data processing and search tasks.
*   **Version Control**: Operations follow standard version control practices with logical commit structuring and incremental progress tracking.

---

## 🚀 Local Quickstart Guide

### 1. Clone the Repository
```bash
git clone https://github.com/samuel-nigussie/aws-sbg-tasks.git
cd aws-sbg-tasks
```

### 2. Configure the Virtual Environment

**Windows (PowerShell):**
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```
*(Note: If execution is restricted, run `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`)*

**macOS / Linux:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Module Dependencies
Navigate to the specific week's directory and install the required packages.
```bash
cd week-01
pip install -r requirements.txt
```

### 4. Execute Tasks

**Launch the Week 01 Interactive Notebook:**
```bash
jupyter notebook Diabetes_Data_Processor.ipynb
```

**Run the Week 02 Semantic Search Script:**
```bash
cd ../week-02
python mini_semantic_search.py
```
