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

## Data cleaning and checks
The notebook reads the original local files, treats question marks as missing values, trims text, and removes trailing full stops from test labels. Exact duplicate rows are removed. Any test input profile already present in training is removed from the test set. The cleaned sets contain 32,537 training rows and 16,250 test rows.

The notebook checks valid labels, numerical ranges, missing values, and train/test profile overlap. Matching training profiles receive group keys for later cross-validation. No models have been fitted in this committed notebook stage.

## Evidence for this stage
- `Project_09_2021ICT106.ipynb`: executed loading and cleaning cells.
- `data/adult_train_clean.csv` and `data/adult_test_clean.csv`: cleaned datasets.
- `results/data_audit.csv`: original counts and removed rows.
- `results/source_checksums.json`: SHA-256 hashes of the three original files.

Open the notebook from this project folder and run its cells from top to bottom. The notebook uses local data and a fixed seed of 106. Model comparisons, importance experiments, error analysis, and final reports follow in later commits.
