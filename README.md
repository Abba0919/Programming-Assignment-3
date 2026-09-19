# ECE2112---PROGRAMMING-ASSIGNMENT-3

## Merwin Abba B. Llorin

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Dataset Description](#-dataset-description)
- [Output Summary](#-output-summary)
- [Problem Specifications & Solutions](#-problem-specifications--solutions)
  - [A. Positional and Label Based Slicing](#a-positional-and-label-based-slicing)
  - [B. Model Lookup](#b-model-lookup)
  - [C. Multi Model Subsetting](#c-multi-model-subsetting)
- [Project File Structure](#-project-file-structure)
- [Prerequisites & Requirements](#-prerequisites--requirements)
- [Edge Cases Handled](#-edge-cases-handled)

---

## 🔍 Overview

This project contains the Python solution for **Programming Assignment 3** using the `cars` dataset.
The tasks demonstrate fundamental Pandas indexing and subsetting concepts including:

- Inspecting a DataFrame (`.shape`, `.columns.tolist()`)
- Positional slicing with `.iloc[]`
- Label based selection with `.loc[]`
- Boolean masking on a string column
- Membership filtering with `.isin()`
- Non-destructive subsetting using `.copy()`

All rows and values shown are taken directly from the dataset. Nothing is typed manually.

---

## 📊 Dataset Description

The dataset `cars.csv` contains **32 rows and 12 columns** of car specifications.

| Column | Description |
|---|---|
| `Model` | Name of the car model |
| `mpg` | Miles per gallon |
| `cyl` | Number of cylinders |
| `disp` | Displacement (cu. in.) |
| `hp` | Gross horsepower |
| `drat` | Rear axle ratio |
| `wt` | Weight (1000 lbs) |
| `qsec` | Quarter mile time |
| `vs` | Engine shape (0 = V-shaped, 1 = straight) |
| `am` | Transmission (0 = automatic, 1 = manual) |
| `gear` | Number of forward gears |
| `carb` | Number of carburetors |

---

## ⚙️ Output Summary

| Object | Type | Key Logic |
|---|---|---|
| `cars` | `DataFrame` | Source dataset loaded with `pd.read_csv('cars.csv')` |
| `cars_6_to_10` | `DataFrame` | `cars.iloc[5:10].copy()` — rows 6 to 10 by position |
| `toyota` | `DataFrame` | Boolean mask on `Model` for `Toyota Corolla`, all columns |
| `pontiac` | `DataFrame` | `.loc[]` with a mask and a column list for `Pontiac Firebird` |
| `selected_cars` | `DataFrame` | `.isin()` on three models, then selects 5 columns |

---

## 🧩 Problem Specifications & Solutions

### A. Positional and Label Based Slicing

**Specification:** Load the dataset, display its shape and all column names, get rows 6 to 10, then
display only the columns `Model`, `mpg`, `cyl`, `hp`, and `gear` from those rows.

**Solution:**

```python
import pandas as pd

# Load the csv file
cars = pd.read_csv('cars.csv')

# Display the shape of the DataFrame
print("Shape of cars:", cars.shape)

# A. Display all column names
print("Column names:")
print(cars.columns.tolist())

# B. Get rows 6 to 10
cars_6_to_10 = cars.iloc[5:10].copy()

# C. Display selected columns from rows 6 to 10
cars_6_to_10[['Model', 'mpg', 'cyl', 'hp', 'gear']]
```

**Key logic:** `.iloc[5:10]` is used because Python indexing starts at 0, so rows 6 to 10 of the
dataset correspond to positions 5 up to (but not including) 10. `.copy()` makes `cars_6_to_10` an
independent object instead of a view of `cars`.

**Result:** Shape `(32, 12)`; the slice returns 5 rows (Valiant to Merc 280).

---

### B. Model Lookup

**Specification:** Display the full row for `Toyota Corolla`, then display only the `Model`, `mpg`,
`hp`, and `wt` columns for `Pontiac Firebird`.

**Solution:**

```python
# B(a) Toyota Corolla
toyota = cars[cars['Model'].str.strip() == 'Toyota Corolla'].copy()

display(toyota)

# B(b) Pontiac Firebird
pontiac = cars.loc[cars['Model'].str.strip() == 'Pontiac Firebird', ['Model', 'mpg', 'hp', 'wt']].copy()

display(pontiac)
```

**Key logic:** `.str.strip()` removes any stray leading or trailing spaces in the `Model` column so
the comparison does not silently fail. Part (a) uses a plain boolean mask to keep every column, while
part (b) uses `.loc[mask, column_list]` to filter rows and select columns in one step.

**Result:** One row each — Toyota Corolla (index 19) and Pontiac Firebird (index 24).

---

### C. Multi Model Subsetting

**Specification:** Create a DataFrame named `selected_cars` containing `Datsun 710`, `Lotus Europa`,
and `Ferrari Dino`, keeping only `Model`, `mpg`, `cyl`, `hp`, and `gear`. Display it and its shape.

**Solution:**

```python
# Create selected_cars DataFrame
selected_cars = cars[cars['Model'].isin(['Datsun 710', 'Lotus Europa', 'Ferrari Dino'])][['Model', 'mpg', 'cyl', 'hp', 'gear']]

# Display DataFrame and shape
print("Selected Cars:")
print(selected_cars)
print("\nShape of selected_cars:", selected_cars.shape)
```

**Key logic:** `.isin()` checks the three model names in a single expression, which is cleaner than
chaining three `==` comparisons with `|`. The column list is applied after the filtering.

**Result:** Shape `(3, 5)`.

---

## 📁 Project File Structure

```
ECE2112---PROGRAMMING-ASSIGNMENT-3/
│
├── LLORIN_ECE2112_PA3.ipynb   # Main notebook (all cells executed)
├── cars.csv                   # Source dataset
└── README.md                  # This file
```

---

## 🧾 Prerequisites & Requirements

| Requirement | Version used |
|---|---|
| Python | 3.13 |
| pandas | 2.0+ |
| Jupyter Notebook / JupyterLab | any recent version |

Install the libraries with:

```bash
pip install pandas notebook
```

---

## ⚠️ Edge Cases Handled

- **Zero based indexing** — rows 6 to 10 are taken with `.iloc[5:10]`, accounting for the fact that
  the upper bound of a positional slice is exclusive.
- **Whitespace in model names** — `.str.strip()` is applied before comparing, so an entry stored as
  `"Toyota Corolla "` is still matched.
- **Views vs copies** — `.copy()` is used on the sliced results so later edits never raise a
  `SettingWithCopyWarning` or modify the original `cars` DataFrame.
- **Original DataFrame preserved** — `cars` is only read from; every subset is stored under a new name.
- **Multiple model lookup** — `.isin()` handles the three-model filter in one expression instead of
  chained `|` conditions, which keeps the mask readable and avoids operator precedence mistakes.
- **Relative file path** — `pd.read_csv('cars.csv')` works on any machine as long as the dataset sits
  beside the notebook, so no absolute path needs editing.
