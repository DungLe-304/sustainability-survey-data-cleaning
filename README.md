# 🌱 Sustainability Survey — Data Cleaning & Tidying

![R](https://img.shields.io/badge/R-4.5.2-276DC3?style=flat&logo=r&logoColor=white)
![Quarto](https://img.shields.io/badge/Quarto-Document-4A90D9?style=flat&logo=quarto&logoColor=white)
![tidyverse](https://img.shields.io/badge/tidyverse-2.0.0-1A162D?style=flat)
![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=flat&logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat)

A data cleaning and tidying project using **R**, **Quarto**, and **Excel** on a real-world sustainability survey dataset collected at Truman State University (Fall 2015).

---

## 📋 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Workflow](#workflow)
- [Key Cleaning Steps](#key-cleaning-steps)
- [Visualization](#visualization)
- [Key Findings](#key-findings)
- [Tools & Technologies](#tools--technologies)
- [How to Run](#how-to-run)

---

## Overview

This project demonstrates a complete **data cleaning pipeline** — from raw, dirty survey data to a clean, analysis-ready dataset. The workflow combines preliminary data preparation in **Excel** with systematic cleaning and transformation in **R**, documented in a reproducible **Quarto** report.

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
│   ├── sustainability_dirtydata.xlsx         # Raw dataset (dirty)
│   └── Working.csv                           # Exported from Excel after column renaming
│
├── 📂 cleaned_data/                          # Output files after cleaning
│   ├── sustainability_working.xlsx           # Excel workbook: Dictionary + Working sheets
│   └── sustainability_cleaned_TonyLe.csv    # Final cleaned dataset (R output)
│
└── 📂 src/                                   # Source code & report
    ├── sustainability_cleaning_TonyLe.qmd   # Quarto source (R code + documentation)
    └── HW03_TonyLe.pdf                       # Rendered PDF report
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
│  • Read Working.csv             │
│  • 8 Cleaning Steps             │
│  • Visualization                │
│  • Export cleaned .csv          │
└─────────────────────────────────┘
              │
              ▼
cleaned_data/sustainability_cleaned_TonyLe.csv
           (182 rows × 38 columns)
```

---

## Key Cleaning Steps

### Step 1 — Excel: Data Dictionary & Column Renaming

Created a **Data Dictionary** sheet mapping all 35 original column names to short, code-friendly names, descriptions, and coding schemes. New names were then transposed back into the Working sheet header row.

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

## Visualization

A stacked bar chart showing the distribution of students' willingness to increase tuition, broken down by their first-listed school. Schools are ordered from **least to most willing** based on mean willingness score.

> Red shades = less willing · Blue shades = more willing

---

## Key Findings

- 🏫 **Business** had the **least** willingness to increase tuition (mean score = **1.83** out of 5, 30.4% "Not willing at all")
- 🔬 **Science & Math** had the **most** willingness (mean score = **2.62**, only 17.0% "Not willing at all")
- 📊 Across **all schools**, the two lowest categories ("Not willing at all" and "$1–$10") dominated responses — suggesting cost resistance is a **university-wide phenomenon**, not school-specific
- 👥 **50 out of 182 students** (27.5%) were affiliated with the School of Science & Mathematics (SAM)
- 📢 **105 students** (57.7%) had heard about Sustainability Week; only a subset had actually participated

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| **R 4.5.2** | Data cleaning, transformation, visualization |
| **tidyverse** | `dplyr`, `tidyr`, `ggplot2`, `stringr` |
| **Quarto** | Reproducible report (code + narrative → PDF) |
| **Microsoft Excel** | Data dictionary, column renaming, CSV export |

---

## How to Run

1. **Clone the repository**
```bash
git clone https://github.com/DungLe-304/sustainability-survey-data-cleaning.git
cd sustainability-survey-data-cleaning
```

2. **Ensure R packages are installed**
```r
install.packages("tidyverse")
```

3. **Render the Quarto document** from the `src/` folder
```bash
quarto render src/sustainability_cleaning_TonyLe.qmd --to pdf
```

Or open in **RStudio** and click **Render**.

> ⚠️ The `.qmd` file reads from `../raw_data/Working.csv` and exports to `../cleaned_data/`. Make sure the folder structure is intact before rendering.

---

## Author

**Tony Le** · Data Science Student @ Truman State University  
📎 [GitHub: DungLe-304](https://github.com/DungLe-304)
