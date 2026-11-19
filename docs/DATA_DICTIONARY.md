# Data guide
**Source:** UCI Adult, Becker and Kohavi (1996). DOI: 10.24432/C5XW20.

| Column | Simple meaning | Type and handling |
| --- | --- | --- |
| age | Age in years | Number |
| workclass | Type of employer or work | Category, missing values allowed |
| fnlwgt | Census sample weight | Number, used as an input, not a row weight |
| education | Education level name | Category |
| education-num | Education level as a number | Number, closely linked to education |
| marital-status | Recorded marriage status | Category |
| occupation | Work category | Category, missing values allowed |
| relationship | Position in the household | Category |
| race | Recorded race category | Category, sensitive information |
| sex | Recorded sex category | Category, sensitive information |
| capital-gain | Recorded capital gain | Number |
| capital-loss | Recorded capital loss | Number |
| hours-per-week | Reported working hours per week | Number |
| native-country | Recorded country of origin | Category, missing values allowed |
| income | <=50K or >50K per year | Target, encoded as 0 or 1 |

The original source uses historical category names. They are kept as source values. A question mark means missing information. The notebook fills missing inputs using training data only.

`random_number` and `random_category` exist only in the separate controlled experiment. They are generated without using the target. They are not original Census columns and are not included in the main final model.
