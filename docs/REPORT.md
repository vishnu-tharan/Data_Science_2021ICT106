# Project report
**B.Vishnutharan**  
**2021/ICT/106**

## 1. Question
How do Random Forest's built-in feature importance and permutation importance differ when predicting adult income, and when can their rankings be misleading?

## 2. Why this project matters
A model can make good predictions but give feature rankings that are easy to misunderstand. I chose Random Forest because it provides built-in importance and can also be checked using permutation importance. The aim is to compare these measures, rather than treating either ranking as the final truth.

## 3. Data and cleaning
The UCI Adult dataset contains 48,842 records with 14 inputs and one income target. The records come from the 1994 US Census. The source supplies separate training and test files.

The original training file has 32,561 rows. Cleaning removes 24 exact duplicates. The original test file has 16,281 rows. Cleaning removes 5 exact duplicates and 26 test profiles already found in training. The final sets contain 32,537 training rows and 16,250 test rows.

A question mark is read as a missing value. Text spaces and trailing full stops in test labels are cleaned. Missing categories use the training mode. Numerical preparation uses the training median if needed. One-hot encoding handles categories. Logistic Regression scales numerical inputs. Each fold learns these steps from its own training rows.

I checked repeated profiles because the same inputs in fitting and evaluation can make the result too easy. Matching training profiles stay in the same cross-validation fold. Removing overlaps makes the test stricter, but also changes the set of people covered by the score.

## 4. Baselines and model choice
The majority baseline always predicts the most common class. Logistic Regression provides a stronger simple comparison. The forest settings use 120 trees, maximum feature share 0.7, and minimum leaf sizes of 1, 5, or 15. The seed is 106. Other forest options use the documented package defaults.

Five stratified group folds use training data only. The forest with the largest mean ROC-AUC is selected. The threshold stays at 0.5. The chosen model is **Random Forest leaf 15**, with mean validation ROC-AUC **0.9159** and fold standard deviation **0.0043**.

| model | roc_auc_mean | roc_auc_std | balanced_accuracy_mean | f1_mean |
| --- | --- | --- | --- | --- |
| Majority baseline | 0.5000 | 0.0000 | 0.5000 | 0.0000 |
| Logistic Regression | 0.9057 | 0.0054 | 0.7635 | 0.6574 |
| Random Forest leaf 1 | 0.9051 | 0.0039 | 0.7761 | 0.6749 |
| Random Forest leaf 5 | 0.9139 | 0.0041 | 0.7796 | 0.6844 |
| Random Forest leaf 15 | 0.9159 | 0.0043 | 0.7770 | 0.6814 |

The forest is the main model because the question concerns tree importance. Logistic Regression remains a useful comparison even if a particular metric favours it. Cross-validation differences are descriptive and do not establish statistical significance.

## 5. Final test evaluation
The chosen settings are fixed before final testing. Each final model fits all cleaned training rows.

| model | accuracy | balanced_accuracy | precision | recall | f1 | roc_auc |
| --- | --- | --- | --- | --- | --- | --- |
| Majority baseline | 0.7637 | 0.5000 | 0.0000 | 0.0000 | 0.0000 | 0.5000 |
| Logistic Regression | 0.8506 | 0.7596 | 0.7280 | 0.5870 | 0.6499 | 0.9038 |
| Random Forest leaf 15 | 0.8625 | 0.7762 | 0.7592 | 0.6125 | 0.6780 | 0.9133 |

The forest's test average precision is **0.7994**. It improves accuracy by **9.88 percentage points** over the majority baseline. Its ROC-AUC difference from Logistic Regression is **+0.0095**.

Training accuracy is 0.8863, compared with test accuracy 0.8625. This gap helps check how far fitting performance carries over to new rows. There are 2,234 incorrect test predictions. The confusion matrix, ROC curve, and precision-recall curve appear in the notebook.

## 6. Importance comparison
Built-in importance sums improvements in training class splits. It is normalised to add up to 1. Encoded categories are added together to return to the original inputs.

Permutation importance measures the score before a shuffle minus the score after it. The main comparison uses balanced accuracy. ROC-AUC is also recorded. Each original input is shuffled 10 times on a fixed sample of 2,500 test rows. One standard deviation across shuffles shows variation. It is not a confidence interval.

| feature | mdi | mdi_rank | permutation_mean | permutation_std | permutation_rank |
| --- | --- | --- | --- | --- | --- |
| marital-status | 0.2803 | 1 | 0.0786 | 0.0062 | 1 |
| capital-gain | 0.1754 | 2 | 0.0586 | 0.0054 | 2 |
| education-num | 0.1746 | 3 | 0.0525 | 0.0039 | 3 |
| age | 0.0738 | 4 | 0.0321 | 0.0074 | 4 |
| occupation | 0.0372 | 9 | 0.0228 | 0.0059 | 5 |
| hours-per-week | 0.0460 | 8 | 0.0196 | 0.0045 | 6 |
| capital-loss | 0.0469 | 7 | 0.0130 | 0.0017 | 7 |
| relationship | 0.0681 | 5 | 0.0100 | 0.0028 | 8 |
| native-country | 0.0012 | 14 | 0.0018 | 0.0011 | 9 |
| workclass | 0.0157 | 11 | 0.0009 | 0.0020 | 10 |
| race | 0.0021 | 13 | 0.0006 | 0.0014 | 11 |
| sex | 0.0046 | 12 | -0.0032 | 0.0013 | 12 |
| education | 0.0161 | 10 | -0.0040 | 0.0026 | 13 |
| fnlwgt | 0.0581 | 6 | -0.0075 | 0.0029 | 14 |

The top built-in inputs are marital-status, capital-gain, education-num. The top test permutation inputs are marital-status, capital-gain, education-num. Rank correlation is 0.710. These measures have different units, so their bar sizes cannot be compared directly.

### What the comparison shows
Both methods agree on the first three inputs: marital-status, capital-gain, and education-num. The disagreement is clearer lower in the ranking. The Census weight, fnlwgt, ranks sixth by built-in importance and fourteenth by test permutation importance. Its average balanced-accuracy drop is -0.00745. In this sample, shuffling it improves the score slightly. This does not prove the column is always useless, but its training importance needs care.

The ROC-AUC results also put fnlwgt below zero. Occupation moves from ninth by built-in importance to fifth by balanced-accuracy permutation importance. The rank correlation of about 0.710 shows partial agreement, rather than completely opposite rankings.

## 7. Random input experiment
Two random columns are added to a copy of the training data without using income labels. One is numerical with many distinct values. The other has three categories. A separate group split reserves validation rows. The original test set is not used. Two forests use leaf sizes 1 and 15.

| leaf_size | split | feature | mdi | permutation_mean | permutation_std |
| --- | --- | --- | --- | --- | --- |
| 1 | fitting | random_number | 0.1053 | 0.0851 | 0.0042 |
| 1 | fitting | random_category | 0.0282 | 0.0204 | 0.0015 |
| 1 | validation | random_number | 0.1053 | -0.0080 | 0.0031 |
| 1 | validation | random_category | 0.0282 | -0.0021 | 0.0018 |
| 15 | fitting | random_number | 0.0398 | 0.0160 | 0.0021 |
| 15 | fitting | random_category | 0.0105 | 0.0051 | 0.0018 |
| 15 | validation | random_number | 0.0398 | -0.0024 | 0.0024 |
| 15 | validation | random_category | 0.0105 | 0.0005 | 0.0017 |

For leaf size 1, the random numerical input has MDI 0.1053 and validation permutation drop -0.0080. A useful-looking training importance with a small validation drop is a warning that the model may be remembering chance patterns. The numerical and categorical controls also differ in how many values and encoded columns they have. This experiment illustrates the combined effects of fitting noise and available split choices. It does not isolate every source of bias or prove that all high-cardinality inputs are useless.

## 8. Related education inputs
Education and education-num contain the same education-level information in different forms. The joint shuffle gives a mean balanced-accuracy drop of 0.0519. Individual and joint values appear in `results/education_group_importance.csv`. If the model can use one input as a substitute for another, shuffling only one may hide their shared value. Group shuffling also has limits because it can break links with other inputs.

### What the education check shows
The joint education drop is about 0.05190, close to the education-num drop of 0.05253. Shuffling education alone gives a small negative drop of -0.00398. For this fitted forest, education-num carries most of the measured education value. The joint check does not show a clearly larger drop here. Related inputs can make interpretation difficult, but this result should not be described as evidence of a large hidden joint effect.

## 9. Error analysis
The selection rule takes five highest-confidence false positives and five highest-confidence false negatives. These cases show confident failures, not a random sample of errors. Probability means the model's score for >50K. These scores have not been separately calibrated.

| source_row_index | error_type | age | education | hours-per-week | capital-gain | probability_above_50k |
| --- | --- | --- | --- | --- | --- | --- |
| 3605 | False positive | 36 | HS-grad | 90 | 41310 | 0.9882 |
| 13783 | False positive | 51 | Some-college | 40 | 0 | 0.9363 |
| 10880 | False positive | 40 | Masters | 50 | 0 | 0.9320 |
| 2926 | False positive | 65 | Masters | 40 | 7978 | 0.9275 |
| 6706 | False positive | 67 | Masters | 40 | 0 | 0.9127 |
| 3059 | False negative | 22 | Some-college | 25 | 0 | 0.0000 |
| 5953 | False negative | 20 | 12th | 35 | 0 | 0.0000 |
| 6246 | False negative | 32 | HS-grad | 38 | 0 | 0.0000 |
| 7720 | False negative | 23 | Some-college | 40 | 0 | 0.0000 |
| 10408 | False negative | 22 | Some-college | 15 | 0 | 0.0000 |

**Case 1, original test row index 3605:** Age 36, HS-grad, 90 hours per week, capital gain 41,310, capital loss 0. The model favoured >50K, but the recorded income was <=50K. Among 467 training rows with the same education, marriage status and occupation, 49.7% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 2, original test row index 13783:** Age 51, Some-college, 40 hours per week, capital gain 0, capital loss 1,902. The model favoured >50K, but the recorded income was <=50K. Among 481 training rows with the same education, marriage status and occupation, 57.4% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 3, original test row index 10880:** Age 40, Masters, 50 hours per week, capital gain 0, capital loss 0. The model favoured >50K, but the recorded income was <=50K. Among 335 training rows with the same education, marriage status and occupation, 87.5% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 4, original test row index 2926:** Age 65, Masters, 40 hours per week, capital gain 7,978, capital loss 0. The model favoured >50K, but the recorded income was <=50K. Among 7 training rows with the same education, marriage status and occupation, 0.0% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 5, original test row index 6706:** Age 67, Masters, 40 hours per week, capital gain 0, capital loss 2,457. The model favoured >50K, but the recorded income was <=50K. Among 448 training rows with the same education, marriage status and occupation, 77.0% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 6, original test row index 3059:** Age 22, Some-college, 25 hours per week, capital gain 0, capital loss 0. The model favoured <=50K, but the recorded income was >50K. Among 429 training rows with the same education, marriage status and occupation, 0.9% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 7, original test row index 5953:** Age 20, 12th, 35 hours per week, capital gain 0, capital loss 0. The model favoured <=50K, but the recorded income was >50K. Among 61 training rows with the same education, marriage status and occupation, 0.0% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 8, original test row index 6246:** Age 32, HS-grad, 38 hours per week, capital gain 0, capital loss 0. The model favoured <=50K, but the recorded income was >50K. Among 332 training rows with the same education, marriage status and occupation, 1.8% had income above 50K. This test row has 0 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 9, original test row index 7720:** Age 23, Some-college, 40 hours per week, capital gain 0, capital loss 0. The model favoured <=50K, but the recorded income was >50K. Among 199 training rows with the same education, marriage status and occupation, 3.0% had income above 50K. This test row has 1 missing input values. This comparison gives context, not a proven reason for the model decision.

**Case 10, original test row index 10408:** Age 22, Some-college, 15 hours per week, capital gain 0, capital loss 0. The model favoured <=50K, but the recorded income was >50K. Among 347 training rows with the same education, marriage status and occupation, 0.6% had income above 50K. This test row has 3 missing input values. This comparison gives context, not a proven reason for the model decision.

These examples show that similar recorded details can still lead to different income labels. The records omit much of a person's full situation. The notes describe the cases but do not claim to prove each decision's cause. Global feature rankings are not explanations of individual predictions.

### Patterns in the ten reviewed errors
The source row indices are zero-based positions in the original test table, not text-file line numbers.

Among the five selected false positives, three have a Master's education and one has a capital gain of 41,310 with 90 working hours each week. Another records a capital loss of 1,902. These details may look like patterns linked to higher income in the training data, but all five actual labels are <=50K. The selected cases show that these patterns are not rules for every person.

Four of the five selected false negatives are aged 20 to 23. One works 15 hours each week and another works 25. One row has missing workclass, occupation, and native-country. Another has missing native-country. The 32-year-old case is also a false negative, so age alone cannot explain this group. All five model scores for >50K are zero in this run. A zero score from a fitted forest does not make the prediction certain or prove the source label is wrong.

These are observations and possible questions to follow up. They are not local feature explanations or confirmed causes. Each case now compares similar training profiles and reports missing inputs. These checks add evidence, but they do not prove the reason for a prediction. A wider study of similar profiles could investigate the errors further. The test cases were not used to change the model.

### Training-data evidence for each error
The comparison uses education, marriage status, and occupation together. It reports the number of matching training rows and their share with income above $50,000. It also counts missing test inputs. Small matching groups need extra care. These values help describe the cases, but do not prove which inputs caused an individual model decision.

| source_row_index | training_match_count | training_above_50k_share | missing_input_count |
| --- | --- | --- | --- |
| 3605 | 467 | 0.4968 | 0 |
| 13783 | 481 | 0.5738 | 0 |
| 10880 | 335 | 0.8746 | 0 |
| 2926 | 7 | 0.0000 | 0 |
| 6706 | 448 | 0.7701 | 0 |
| 3059 | 429 | 0.0093 | 0 |
| 5953 | 61 | 0.0000 | 0 |
| 6246 | 332 | 0.0181 | 0 |
| 7720 | 199 | 0.0302 | 1 |
| 10408 | 347 | 0.0058 | 3 |

The whole test set also gives this age-band check:

| age_band | rows | errors | error_rate |
| --- | --- | --- | --- |
| 17-29 | 4785 | 181 | 0.0378 |
| 30-44 | 6190 | 1079 | 0.1743 |
| 45-59 | 3864 | 765 | 0.1980 |
| 60+ | 1411 | 209 | 0.1481 |

Age bands differ in sample size. Their error rates are descriptive. A separate study would be needed to test fairness and understand differences properly.

## 10. Conclusion
Built-in and permutation importance answer different questions. Built-in importance describes training splits. Permutation importance describes the loss in a chosen score after an input is shuffled. Comparing unseen-row results with random controls gives a clearer check than relying on a built-in ranking alone. Related inputs need special care.

## 11. Limits and responsible use
The data is old and from the US. It should not be treated as current Sri Lankan evidence. Income classes are unequal. The analysis gives unweighted row-level scores and treats fnlwgt as an input, not as a Census weight. It does not estimate population income rates. Importance depends on model settings, sample, and score. Shuffle variation does not measure all uncertainty. Importance does not establish cause.

Public demographic data can contain historical bias. Including sex and race here does not establish fair treatment. No private student or staff data is used. This is a learning study and should not be used for real employment, credit, or pay decisions.

## 12. Reproducibility and submission status
The full notebook was run again from a fresh kernel during the final review. All code cells completed without errors. The review independently checked source-file hashes, cleaned row counts, duplicate profiles, fold summaries, the model selection rule, final test metrics, permutation repeats, and the ten error cases against the saved evidence. The checks are recorded in `results/verification.json`.

The seed is 106 and the checked Python version is 3.13.2. Exact analysis-package versions appear in `requirements.txt` and `results/environment.json`. The data, notebook, saved evidence, README, and presentation are included. The PDF is a readable report of the checked run.

GitHub publication remains an account action: upload this project folder to a repository and submit the link. The viva also remains: read the notebook, rerun it, and practise explaining every code cell. The completed files do not replace those two requirements.

## 13. References
1. Becker, B. and Kohavi, R. (1996). Adult. UCI Machine Learning Repository. https://doi.org/10.24432/C5XW20. CC BY 4.0.
2. scikit-learn, Permutation feature importance. https://scikit-learn.org/stable/modules/permutation_importance.html
3. scikit-learn, Permutation Importance vs Random Forest Feature Importance. https://scikit-learn.org/stable/auto_examples/inspection/plot_permutation_importance.html
4. scikit-learn, Permutation Importance with Multicollinear or Correlated Features. https://scikit-learn.org/stable/auto_examples/inspection/plot_permutation_importance_multicollinear.html
