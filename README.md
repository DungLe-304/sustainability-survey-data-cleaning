# 🌱 Sustainability Survey — Data Cleaning, Tidying & Visualization

![R](https://img.shields.io/badge/R-4.5.2-276DC3?style=flat&logo=r&logoColor=white)
![Quarto](https://img.shields.io/badge/Quarto-HTML%20%7C%20PDF-4A90D9?style=flat&logo=quarto&logoColor=white)
![tidyverse](https://img.shields.io/badge/tidyverse-2.0.0-1A162D?style=flat)
![plotly](https://img.shields.io/badge/plotly-4.12.0-3F4F75?style=flat&logo=plotly&logoColor=white)
![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat)

A complete data cleaning, tidying, and visualization project using **R**, **Quarto**, and **Excel** on a real-world sustainability survey dataset collected at Truman State University (Fall 2015).

---

## 📋 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Workflow](#workflow)
- [Key Cleaning Steps](#key-cleaning-steps)
- [Visualizations](#visualizations)
- [Key Findings](#key-findings)
- [Tools & Technologies](#tools--technologies)
- [How to Run](#how-to-run)

---

## Overview

This project demonstrates a complete **data science pipeline** — from raw, dirty survey data to a clean, analysis-ready dataset with interactive visualizations. The workflow combines preliminary data preparation in **Excel** with systematic cleaning, transformation, and visualization in **R**, all documented in a fully reproducible **Quarto** report.

The dataset comes from a student survey on attitudes toward a potential Sustainability Office at Truman State University, conducted by Dr. Alberts' STAT 376 class.

---

## Dataset

| Property | Details |
|----------|---------|
| **Source** | Truman State University — STAT 376, Fall 2015 |
| **Observations** | 182 students |
| **Raw Variables** | 35 columns |
| **Cleaned Variables** | 38 columns |
| **Topics Covered** | Gender, grade level, school affiliation, sustainability awareness, activity participation, office preference, tuition willingness |

---

## Project Structure

```
sustainability-survey-data-cleaning/
│
├── 📄 README.md
│
├── 📂 raw_data/                              # Original files before any processing
│   ├── sustainability_dirtydata.xlsx         # Raw dataset (dirty) — original Excel
│   ├── sustainability_dirtydata.csv          # Raw dataset — CSV version for quick preview
│   └── Working.csv                           # Exported from Excel after column renaming
│
├── 📂 cleaned_data/                          # Output files after cleaning
│   ├── sustainability_working.xlsx           # Excel workbook: Dictionary + Working sheets
│   └── sustainability_cleaned_TonyLe.csv    # Final cleaned dataset (R output)
│
└── 📂 src/                                   # Source code & reports
    ├── sustainability_cleaning_TonyLe.qmd   # Quarto source (cleaning + visualization)
    ├── Data project TonyLe.html              # Interactive HTML report (plotly)
    ├── Data Cleaning, Tidying and            # Full PDF report
    │   Visualization.pdf
    └── sustainability_cleaning_TonyLe_files/ # HTML assets (plotly, bootstrap, etc.)
```

---

## Workflow

```
raw_data/sustainability_dirtydata.xlsx
              │
              ▼
┌─────────────────────────────────┐
│             EXCEL               │
│  • Copy Raw Data → Working      │
│  • Build Data Dictionary        │
│  • Rename all 35 columns        │
│  • Export Working → .csv        │
└─────────────────────────────────┘
              │
              ▼
    raw_data/Working.csv
              │
              ▼
┌─────────────────────────────────┐
│           R / QUARTO            │
│  • 8 Data Cleaning Steps        │
│  • EDA Visualizations           │
│  • Interactive Plots (plotly)   │
│  • Statistical Analysis         │
│  • Export cleaned .csv          │
└─────────────────────────────────┘
              │
        ┌─────┴─────┐
        ▼           ▼
   HTML Report   PDF Report
  (interactive) (static)
```

---

## Key Cleaning Steps

### Step 1 — Excel: Data Dictionary & Column Renaming

Created a **Data Dictionary** sheet mapping all 35 original column names to short, code-friendly names, descriptions, and coding schemes.

| New Name | Original Name (example) |
|----------|------------------------|
| `ID` | `ResponseID` |
| `Gender` | `What is your gender?` |
| `Grade` | `What grade level are you? (...)` |
| `Heard_sustain` | `Check all activities that you've heard of... _Sustainability week` |

### Step 2 — R: Data Cleaning Pipeline

| Step | Variable | Problem | Solution |
|------|----------|---------|----------|
| (a) | `Gender` | Long character string | Recode to Factor with 3 short levels |
| (b) | `Grade` | Character strings + hidden trailing spaces | `trimws()` + `case_when()` → numeric 1–5 |
| (c) | `School` | Multiple values in one cell (`"HSE SAM BUS"`) | `separate()` → `School1`, `School2`, `School3` |
| (d) | `SAM` | No binary indicator for school membership | Create `SAM = 1/0` checking all 3 School columns |
| (e) | `Heard_sustain` | Text token vs. 0/1 inconsistency with `Part_*` | `case_when()` → recode to 0/1 |
| (f) | `Effective` | Typo: `"Neutrale"` | `str_replace()` with regex anchors `^$` |
| (g) | `Increase_rate` | Unordered character — alphabetical default is wrong | `factor(..., ordered = TRUE)` with 5 correct levels |
| (h) | — | Export | `write.csv()` → `cleaned_data/sustainability_cleaned_TonyLe.csv` |

---

## Visualizations

The interactive HTML report includes **8 plotly charts** and **3 statistical tests**:

| # | Chart | Type |
|---|-------|------|
| 1 | Gender Distribution | Interactive Donut Chart |
| 2 | Grade Level Distribution | Interactive Bar Chart |
| 3 | Heard About vs. Participated in Activities | Grouped Bar Chart |
| 4 | Overall Confidence in Sustainability | Bar Chart |
| 5 | Confidence Level by School | Stacked Bar Chart |
| 6 | Office Preference (Physical vs. Online) | Donut Chart |
| 7 | Office Preference by School | Stacked Bar Chart |
| 8 | Tuition Willingness by First-Listed School | Stacked Bar Chart |

**Statistical Tests:**
- Chi-square: Office preference vs. School affiliation
- Chi-square: Gender vs. Tuition willingness  
- Spearman correlation: Confidence level vs. Tuition willingness (+ Heatmap)

---

## Key Findings

- 🏫 **Business** had the **least** willingness to increase tuition (mean = **1.83**/5, 30.4% "Not willing at all")
- 🔬 **Science & Math** had the **most** willingness (mean = **2.62**/5, only 17.0% "Not willing at all")
- 📢 **105 students** (57.7%) had *heard about* Sustainability Week, but very few actually *participated* — a clear **awareness-to-engagement gap**
- 🏢 Most students preferred a **Physical Office** over an online one, despite the extra cost
- 📊 Cost resistance to tuition increases is **university-wide**, not driven by any specific school
- 👥 **50 out of 182 students** (27.5%) were affiliated with the School of Science & Mathematics

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| **R 4.5.2** | Data cleaning, transformation, visualization |
| **tidyverse** | `dplyr`, `tidyr`, `ggplot2`, `stringr` |
| **plotly** | Interactive visualizations in HTML |
| **Quarto** | Reproducible report (HTML + PDF output) |
| **Microsoft Excel** | Data dictionary, column renaming, CSV export |

---

## How to Run

1. **Clone the repository**
```bash
git clone https://github.com/DungLe-304/sustainability-survey-data-cleaning.git
cd sustainability-survey-data-cleaning
```

2. **Install R packages**
```r
install.packages(c("tidyverse", "plotly", "scales"))
```

3. **Render the Quarto document**
```bash
quarto render src/sustainability_cleaning_TonyLe.qmd --to html
```

Or open in **RStudio** → click **Render**.

> ⚠️ The `.qmd` file reads from `../raw_data/Working.csv` and exports to `../cleaned_data/`. Keep the folder structure intact before rendering.

---

## Author

**Tony Le** · Data Science Student @ Truman State University  
📎 [GitHub: DungLe-304](https://github.com/DungLe-304)
