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

## Current stage
The project question, environment requirements, and ignore rules are set up. Data and notebook work follow in later commits.
