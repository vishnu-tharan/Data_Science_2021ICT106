# Project 09: Permutation Importance vs Built-in Importance

**Name:** B.Vishnutharan  
**Registration number:** 2021/ICT/106

## Project question
How do Random Forest's built-in feature importance and permutation importance differ when predicting adult income, and when can their rankings be misleading?

## Data and cleaning
I chose the public UCI Adult Income dataset because it contains numbers and categories. It comes from the 1994 US Census and contains 48,842 rows before cleaning. The target is income above $50,000. The original training and test files stay separate.

After removing duplicates and overlapping test input profiles, there are 32,537 training rows and 16,250 test rows. The original and cleaned data are included in `data/`. Source credit is in `data/SOURCE.md`, and column meanings are in `docs/DATA_DICTIONARY.md`.

## Model evaluation
The project compares a majority baseline, Logistic Regression, and three Random Forest settings. Five stratified group folds keep matching input profiles together. Each pipeline learns preparation from its training rows only. Mean validation ROC-AUC selects the forest. Test results do not change its settings.

The selected forest uses a minimum leaf size of 15. Its test accuracy is 86.25%, compared with 76.37% for the majority baseline and 85.06% for Logistic Regression. Forest ROC-AUC is 0.9133 and F1 is 0.6780. Fold scores, final metrics, and evaluation charts are included as evidence.

## Importance comparison
Built-in importance is grouped across encoded columns for each original input. Permutation importance shuffles each original input 10 times on a fixed sample of 2,500 test rows. Balanced accuracy is the main score, with ROC-AUC also checked. Shuffle standard deviations show variation, not confidence intervals.

Both methods rank marital-status, capital-gain, and education-num highest. Census weight ranks sixth by built-in importance and fourteenth by test permutation importance. Shuffling it slightly improves the test sample score. The comparison table, individual shuffle results, and chart are included.

Education and education-num are also shuffled together. Their joint score drop is close to education-num alone in this fitted forest. These checks describe model behaviour and do not prove cause.

## Random feature experiment
A separate experiment adds a random number and a random category to a copy of the training inputs. It uses a group split for fitting and validation and keeps the final test set out of the experiment. Two forests use minimum leaf sizes of 1 and 15.

The unrestricted forest gives the random number built-in importance of 0.1053. Its validation permutation drop is -0.0080. This shows that importance from training splits may not carry over to new rows. Both experiment settings, both controls, and fitting/validation results are saved with the chart.

## Run
Use Python 3.13. From the project folder:

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

Open `Project_09_2021ICT106.ipynb` and run the cells from top to bottom. The data is local. The notebook saves its outputs in `figures/` and `results/`.

## Limits
The data is old and from the US. It does not describe current Sri Lankan income. Scores treat rows equally and do not apply Census weights. Related inputs can share information. Importance depends on the fitted model and chosen score and does not prove cause. Public demographic data can contain historical bias. This project does not establish fair treatment and should not guide real hiring, lending, or pay decisions.
