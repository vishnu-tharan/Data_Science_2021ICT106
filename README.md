# Project 09: Permutation Importance vs Built-in Importance

**Name:** B.Vishnutharan  
**Registration number:** 2021/ICT/106

## Project question
How do Random Forest's built-in feature importance and permutation importance differ when predicting adult income, and when can their rankings be misleading?

## Plan
Start with data checks and cleaning. Compare a majority-class baseline and Logistic Regression with Random Forest. Use cross-validation for model choice and keep a separate test set. Compare built-in and permutation importance, check random inputs, and review at least ten wrong predictions.

## Environment
Use Python 3.13. From the project folder:

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

## Data
I chose the public [UCI Adult Income dataset](https://archive.ics.uci.edu/dataset/2/adult) because it contains both numbers and categories. It has 48,842 rows and 14 input columns before cleaning. The target is annual income above $50,000. The data comes from the 1994 US Census.

The original training file, test file, and source guide are in `data/raw/`. Source credit is in `data/SOURCE.md`. Column meanings are in `docs/DATA_DICTIONARY.md`.

The original training and test files will stay separate. Only public data is used. This old US dataset should not be treated as current Sri Lankan income data.

## Current stage
Original data and its source information are included. Cleaning and validation checks come next.
