# Project 09: Permutation Importance vs Built-in Importance

**Name:** B.Vishnutharan  
**Registration number:** 2021/ICT/106

## Problem
How do Random Forest's built-in feature importance and permutation importance differ when predicting adult income, and when can their rankings be misleading?

## Data
I chose the public [UCI Adult Income dataset](https://archive.ics.uci.edu/dataset/2/adult) because it has numerical and categorical inputs. It contains 48,842 rows and 14 inputs before cleaning. It comes from the 1994 US Census. The target is annual income above $50,000.

The original training and test files stay separate. Removing exact duplicates and test profiles already found in training leaves 32,537 training rows and 16,250 test rows. Original files and source credit are included in `data/`.

Data credit: Becker, B. and Kohavi, R. (1996). Adult. UCI Machine Learning Repository. DOI: [10.24432/C5XW20](https://doi.org/10.24432/C5XW20). License: CC BY 4.0.

## Method
The notebook compares a majority-class baseline, Logistic Regression, and three Random Forest settings. Five-fold cross-validation chooses the forest using mean ROC-AUC. Matching input profiles stay in the same fold. Data preparation is fitted inside each pipeline. Final test results do not change model settings.

Built-in importance is grouped by original input. Permutation importance shuffles each original input 10 times on a fixed sample of 2,500 test rows. A separate training/validation experiment adds random inputs to check whether the forest gives importance to noise. A joint shuffle checks the related education inputs. Ten wrong test predictions are reviewed with written explanations.

## Results
The selected forest uses a minimum leaf size of 15.

| Model | Test accuracy | Balanced accuracy | F1 | ROC-AUC |
| --- | --- | --- | --- | --- |
| Majority baseline | 76.37% | 0.5000 | 0.0000 | 0.5000 |
| Logistic Regression | 85.06% | 0.7596 | 0.6499 | 0.9038 |
| Random Forest | 86.25% | 0.7762 | 0.6780 | 0.9133 |

The forest improves accuracy by 9.88 percentage points over the majority baseline. Both importance methods rank marital-status, capital-gain, and education-num highest. However, fnlwgt ranks sixth by built-in importance and fourteenth by test permutation importance. Shuffling it slightly improves the score in the selected test sample.

In the separate experiment, the unrestricted forest gives a random numerical input built-in importance of 0.1053, but its validation permutation drop is -0.0080. Its training importance does not carry over to new rows. The full results, charts, error reviews, and explanations are saved inside the notebook. Each error review also gives matching training-profile counts, their income rates, and missing-input counts.

## Files
- `Project_09_2021ICT106.ipynb`: complete notebook with saved outputs.
- `Project_09_2021ICT106.html`: a browser copy of the checked notebook, including outputs.
- `Project_09_Presentation.pptx`: project presentation with speaker notes.
- `data/raw/`: original public dataset files and their original guide.
- `data/SOURCE.md`: data source, license, and cleaning details.
- `requirements.txt`: packages needed to run the notebook.
- `data/adult_train_clean.csv` and `data/adult_test_clean.csv`: the cleaned data used in the checked run.
- `figures/`: four exported charts as evidence.
- `results/`: data checks, cross-validation scores, final metrics, importance results, predictions, ten error reviews, package versions, and source checksums.
- `docs/REPORT.md`: detailed explanation of the methods, findings, and errors.
- `docs/Project_09_Report.pdf`: printable report with tables, charts, and all ten error reviews.
- `docs/DATA_DICTIONARY.md`: column meanings and handling.
- `.gitignore`: excludes local environments, temporary files, and notebook checkpoints.

## Run the notebook
Use Python 3.13 for the closest match to the checked run. Open PowerShell in this project folder:

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m jupyterlab
```

Open `Project_09_2021ICT106.ipynb`, choose the Python environment created above, and select **Restart Kernel and Run All Cells**. Wait for the last cell to print `Run complete`, then save the notebook. Model comparisons and repeated shuffles can take several minutes.

All data files are included. After package installation, the notebook runs without internet access. The seed is 106. The notebook saves the package versions and source-file checksums when it runs.

The checked outputs are included as evidence and are ready to commit to GitHub. Rerunning the notebook refreshes `results/`, `figures/`, and the cleaned CSV files. Save the notebook and commit the updated evidence together if you rerun it.

## Evidence guide
| Check | Evidence |
| --- | --- |
| Data cleaning and repeated rows | `results/data_audit.csv`, cleaned CSVs |
| Model selection | `results/cross_validation.csv`, `results/cross_validation_folds.csv` |
| Final evaluation | `results/test_metrics.csv`, `results/test_predictions.csv`, evaluation chart |
| Importance comparison | `results/importance_comparison.csv`, repeated shuffle results, comparison chart |
| Random input experiment | `results/random_feature_importance.csv`, `results/random_experiment_metrics.csv`, random-column chart |
| Related education inputs | `results/education_group_importance.csv` |
| Error analysis | `results/ten_wrong_predictions.csv`, `results/errors_by_age.csv`, notebook and report |
| Reproducibility | `results/environment.json`, `results/source_checksums.json`, notebook seed, requirements |

## Limits
The data is old and from the US. It does not describe current Sri Lankan income. Importance depends on the model, data sample, and scoring method. Related inputs can share information. Shuffle error bars show variation across shuffles, not all uncertainty. Importance does not prove cause.

Scores treat each row equally. The Census weight is an input, not a row weight. The data includes sex and race, and this work does not establish fair treatment. Only public data is used. This learning project should not guide real hiring, lending, or pay decisions.

## References
- [UCI Adult dataset](https://archive.ics.uci.edu/dataset/2/adult)
- [scikit-learn: Permutation feature importance](https://scikit-learn.org/stable/modules/permutation_importance.html)
- [scikit-learn: Permutation vs Random Forest importance](https://scikit-learn.org/stable/auto_examples/inspection/plot_permutation_importance.html)
- [scikit-learn: Correlated feature importance](https://scikit-learn.org/stable/auto_examples/inspection/plot_permutation_importance_multicollinear.html)

## Final requirement check
The local project work and evidence are complete. The notebook was rerun from a fresh kernel during the final review. The saved metrics, fold summaries, importance repeats, and ten error cases were independently checked against the evidence. See `results/verification.json`.

GitHub publication and the viva remain. Upload this folder to your repository, submit the link, and practise explaining the notebook before the viva.
