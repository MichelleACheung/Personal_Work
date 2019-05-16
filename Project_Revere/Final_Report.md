# Final Report: Detecting Anomalies in Municipal Expenditures
---

## Project Summary
---

Mangement of municipal finances yields large amounts of data annually. Detection of transaction anomoalies proves difficult with time and resources available given the amount of data.

Data analytics and machine learning can help process and evaluate the large amount of municipal financial transactions to find means of cost efficiency as well as any outliers that may be of fraudulent nature.

This project includes the in-depth analysis and machine learning of a municipal's financial accounts. Benford's Law was applied to explore the data and a DBSCAN clusterning algorithm was used as an additional anomaly detector.

## Data
---

An `XML` file is provided by the City of Revere, Massachusetts for the 2017 fiscal year (June 30, 2016 through July 1, 2017). Within this `XML` file, data includes all transactions seen by the City Auditor made in the reference fiscal year.

`ACCOUNT` codes were used to find expenditures only, removing all other transaction types. `ACCOUNT` codes are structured to show fund source, department number, and expenditure object. The `ACCOUNT` codes were separated into different columns by delimeter `-` for sorting and filtering purposes. Through communication with the municipality, `seg7` (segment 8 of the `ACCOUNT` code) numbers which started with a **5** represented an expenditure. Any rows blank (` `), null (`None`), or zero (`00`) found in `seg7` were dropped.

### Data Dictionary

Column | Description | Unit | Note
:--- | :---: | :---: | ---:
ORG | Organization | n/a | -
ACCOUNT | Accounting Code | n/a | -
ACCOUNT DESC | Account Description | n/a | -
YR/PR | Year/Period | Period = calendar month | -
JNL | Journal | n/a | Journal Number
EFF DATE | Effective Date | Calendar Date | Effective date of transaction
SRC | Source | - | -
REF1 | Reference 1 | n/a | -
REF2 | Reference 2 | n/a | -
REF3 | Reference 3 | n/a | -
CHECK # | Check Number | n/a | -
OB | Object | n/a | -
JOURNAL AMOUNT | Journal/Transaction Amount | US Dollars | -
SOY BALANCE | Start of Year Balance | US Dollars | -
NET LEDGER BALANCE | Net Ledger Balance | US Dollars | -
ORIGINAL BUDGET | Original Budget | US Dollars | -
REVISED BUDGET | Revised Budget | US Dollars | -
NET BUDGET BALANCE | Net Budget Balance | US Dollars | -

## Data Analysis by Benford's Law
---

### Data Processing

For data processing by Benford's Law, the all `JOURNAL AMOUNTS` less than 0 (negative numbers) were dropped. Data was then sorted by `ACCOUNT DESC` to find largest accounts (accounts with more than 500 expenditures) for analysis.

### Data Exploration and Analysis

A graph was produce for each large account showing actual `FIRST_DIGIT` percentages in `JOURNAL AMOUNT` against Benford's Law single digit (1-9) percentages. Further analysis into expenditures where any `FIRST_DIGIT` percentages exceeded Benford's Law percentages was performed to granually explore anomalies.

![alt text](https://git.generalassemb.ly/MichelleC/dsi-7-submissions/blob/master/Capstone_Project/classroom_teachers_benfords_law.png)

Detailed exploration of anomalies in expenditures includes evaluating distribution of `JOURNAL AMOUNT` by `FIRST_DIGIT` and specifically looking at transactions that clearly deviated way from 50%-75% of `JOURNAL AMOUNT` values.

## Data Processing and Modeling by DBSCAN
---

### Data Processing/Preparation

Data preparation for the DBSCAN model included passing `seg3` and `seg7` of the `ACCOUNT` codes as integers (these account numbers were originally read as strings because of the `-` delimeters). Large accounts (accounts with more than 500 expenditures) where filtered as in the processing for Benford's Law analysis. Each `ACCOUNT DESC` was converted to dummy variables.

Features used for the model included `seg3` (department number), `seg7` (expenditure object number), `JOURNAL AMOUNT`, `ACCOUNT DESC` categorical columns. As units differed and DBSCAN is a distance-based model, all features were standardized prior to modeling.

### DBSCAN Parameters
As criteria for filtering large accounts included accounts having at least 500 expenditure transactions, the number of clusters (`n_samples`) was set to 500. The clustering done by the DBSCAN model was not considered to approximately replicate the `ACCOUNT DESC` categories as `JOURNAL AMOUNT` values were expected to cluster primarily, independent of which `ACCOUNT DESC` a `JOURNAL AMOUNT` belonged to. Therefore, the DBSCAN model was used as another means of detecting outliers in the data.

Numerous values were passed for the epsilon to determine a value that yieled larger amounts of outliers than found through the Benford's Law analysis. An epsilon value was also determined by finding the range of epsilon values that resulted in a close amount of respective outliers (epsilon value of 'a' yielded similar amounts of outliers with epsilon value of 'b').

Remaining parameters were set to default.

DBSCAN(algorithm='auto', eps=0.004, leaf_size=30, metric='euclidean',
    metric_params=None, min_samples=18, n_jobs=1, p=None)

## Summary
---

### Large accounts (accounts with at least 500 expenditure transactions)

ACCOUNT DESC | JOURNAL AMOUNT: Counts
:--- | :---: 
PERMANENT SALARIES |               2671
EDUCATIONAL INCENTIVE |            1106
REVOLVING ACCOUNT EXPENSES |       1024
CLASSROOM TEACHERS |               1003
PURCHASE OF SERVICES |              957
ADDITIONAL DIFFERENTIAL |           859
OFFICE SUPPLIES |                   777
TUITION TO NON PUBLIC SCHOOLS |     702
MAINTENANCE OF BUILDINGS |          617
BUILDING SECRETARIES |              583
PRINCIPALS |                        583
BUILDING MAINTENANCE & REPAIR |     548
TAX TITLE |                         544
TUITION TO MASS SCHOOLS |           541
ATHLETIC SUPPLIES & MATS |          531
SPED OUTSIDE TRANSPORTATION |       525
ASSISTANT PRINCIPALS |              505
INSTRUCTIONAL MATERIALS |           504

### Anomalies found by Benford's Law

>`CLASSROOM TEACHERS`
 
Anomalies were detected with `FIRST_DIGIT` values of 3, 4, 5, 6, and 7. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |      1003.00
mean |      36869.34
std |       60326.05
min |         689.47
25% |        7518.82
50% |       18992.82
75% |       47916.03
max |     1201879.59

- 3
 - 135 expenses with 50% percentile of \$33,925.25\. However, 3 expenses averaging $361,510.79 were made.
 
- 4
 - 177 expenses with 50% percentile of \$45,737.99\. However, 4 expenses averaging $447,703.47 were made.
 
- 5
 - 140 expenses with 50% percentile of \$52,028.56\. However, 35 expenses averaging $5,842.88 were made.
 
- 6
 - 132 expenses with 50% percentile of \$60,065.38\. Distribution of expenses were about split between averaging $6,404.76 (58 count) and $60,716.15 (74 count).
 
- 7
 - 90 expenses with 50% percentile of \$7,328.24\. However, 1 expense of $72,538.16 was made.

>`ADDITIONAL DIFFERENTIAL`
 
Anomalies were detected with a `FIRST_DIGIT` value of 3. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |    859.00
mean |     673.90
std |     1216.44
min |       12.57
25% |       73.08
50% |      126.93
75% |      307.85
max |     4778.80

- 3
 - 245 expenses with 50% percentile of \$39.25\. However, 79 expenses averaging $3,452.04 were made.

`OFFICE SUPPLIES`
 
Anomalies were detected with `FIRST_DIGIT` values of 5 and 6. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |     777.00
mean |      381.56
std |       794.01
min |         1.25
25% |        49.98
50% |       195.00
75% |       584.13
max |     13054.92

- 5
 - 101 expenses with 50% percentile of \$60,065.38\. Distribution of expenses were about split between averaging $30.70 (46 count) and $547.35 (55 count).
 
- 6
 - 153 expenses with 50% percentile of \$600\. However, 11 expenses averaging $58.27 were made.

>`TUITION TO NON PUBLIC SCHOOLS`
 
Anomalies were detected with `FIRST_DIGIT` values of 5, 6, 7, 8, and 9. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |     702.00
mean |     6843.22
std |      3651.10
min |        66.31
25% |      4751.50
50% |      6744.50
75% |      8745.98
max |     21343.06

- 5
 - 89 expenses with 50% percentile of \$5,455.65\. However, 3 expenses averaging $520.00 were made.

- 6
 - 104 expenses with 50% percentile of \$5,455.65\. However, 2 expenses averaging $354.90 were made.

- 7
 - 104 expenses with 50% percentile of \$7,565\. However, 1 expense of $77.70 was made.
 
- 8
 - 92 expenses with 50% percentile of \$8,609.48\. However, 5 expenses averaging $544.62 were made.

- 9
 - 59 expenses with 50% percentile of \$9,413.64\. However, 2 expenses averaging $504.90 were made.


>`TAX TITLE`
 
Anomalies were detected with a `FIRST_DIGIT` value of 7. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |     544.00
mean |      943.86
std |      4055.33
min |         3.00
25% |        75.00
50% |        75.00
75% |       375.00
max |     34409.52

- 7
 - 368 expenses with 50% percentile of \$75.00\. However, 1 expense of $750.00 was made.

>`BUILDING SECRETARIES`
 
Anomalies were detected with `FIRST_DIGIT` values of 4, 8, and 9. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |    583.00
mean |    1257.96
std |     1112.13
min |      813.23
25% |      885.74
50% |      928.81
75% |      951.31
max |     4837.26

- 4
 - 53 expenses with 50% percentile of \$4,771.07\.  However, 1 expense of $4651.29 and 1 expense of $4,837.26 were made.
 
- 8
 - 262 expenses with 50% percentile of \$868.57\. However, 64 expenses averaging $813.23 were made.

- 9
 - 266 expenses with 50% percentile of \$939.77\. However, all expenses were made approximately within 2 standard deviations. No outliers were considered found.

>`ATHLETIC SUPPLIES & MATS`
 
Anomalies were detected with `FIRST_DIGIT` values of 5, 7, and 8. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |     531.00
mean |      306.24
std |      1134.14
min |        25.00
25% |        75.00
50% |        80.00
75% |       116.00
max |     11852.00

- 5
 - 89 expenses with 50% percentile of \$58.00\. However, 1 expense of $5,934.00 was made.
 
- 7
 - 62 expenses with 50% percentile of \$75.00\. However, 1 expense of $7,249.00 was made.
 
- 8
 - 197 expenses with 50% percentile of \$80.00\. However, 1 expense of $7,249.00 was made.
 
>`TUITION TO MASS SCHOOLS`
 
Anomalies were detected with `FIRST_DIGIT` values of 4, 5, and 6. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |     541.00
mean |     5346.03
std |      2498.54
min |       309.51
25% |      4244.10
50% |      5261.67
75% |      6224.68
max |     32928.00

- 4
 - 115 expenses with 50% percentile of \$4,527.20\. However, all expenses were made approximately within 2 standard deviations. No outliers were considered found.
 
- 5
 - 93 expenses with 50% percentile of \$5,571.18\. However, 1 expense of $565.88 was made.
 
- 6
 - 163 expenses with 50% percentile of \$6,224.68\. However, all expenses were made approximately within 2 standard deviations. No outliers were considered found.

>`SPED OUTSIDE TRANSPORTATION`
 
Anomalies were detected with `FIRST_DIGIT` values of 2 and 3. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |     525.00
mean |     2526.08
std |      1527.54
min |        60.00
25% |      1567.50
50% |      2340.00
75% |      3204.00
max |     10880.00

- 2
 - 157 expenses with 50% percentile of \$2,500.00\. However, 10 expenses averaging $250.33 were made.

- 3
 - 103 expenses with 50% percentile of \$3,318.00\. However, 3 expenses averaging $326.67 were made.

>`ASSISTANT PRINCIPALS`
 
Anomalies were detected with `FIRST_DIGIT` values of 2 and 4. Distribution of **all** expenditures were found as:
 
STATISTIC | VALUE
:--- | :---: 
count |     505.00
mean |     3771.52
std |      4426.15
min |      2008.13
25% |      2021.83
50% |      2042.96
75% |      4096.96
max |     63732.59

- 2 
 - 318 expenses with 50% percentile of \$$2,031.09\. However, 10 expenses averaging $2,260.28 were made.
 
- 4
 - 132 expenses with 50% percentile of \$4,096.96\. However, all expenses were made approximately within 2 standard deviations. No outliers were considered found.

### Anomalies found by DBSCAN Model

The amount of unique large departments numbered 18 in total. The number of clusters identified by the DBSCAN model amounted to 155 including the `noise` class of `-1`.

A Silhouette score of 0.53 resulted. As previously mentioned, the DBSCAN model was used as an additional anomaly detection with parameters to catch at least the outliers found by Benford's Law. The desire to capture much more outliers than detected through the Benford's Law analysis since DBSCAN clusters could not approximate `JOURNAL DESC` classes was prioritized over the performance of the DBSCAN model.


### Common anomalies between Benford's Law Analysis and DBSCAN Model

Comparison of the amount of outliers detected by the DBSCAN model and the Benford's Law analysis for large accounts follows:

ACCOUNT DESC | DBSCAN OUTLIER COUNT
:--- | :---: 
PERMANENT SALARIES |               721
PURCHASE OF SERVICES |             471
CLASSROOM TEACHERS |               376
REVOLVING ACCOUNT EXPENSES |       290
OFFICE SUPPLIES |                  213
BUILDING MAINTENANCE & REPAIR |    132
TUITION TO NON PUBLIC SCHOOLS |    128
INSTRUCTIONAL MATERIALS |          120
EDUCATIONAL INCENTIVE |             76
TUITION TO MASS SCHOOLS |           74
SPED OUTSIDE TRANSPORTATION |       69
MAINTENANCE OF BUILDINGS |          48
TAX TITLE |                         39
ADDITIONAL DIFFERENTIAL |           33
ATHLETIC SUPPLIES & MATS |          27
ASSISTANT PRINCIPALS |              22
BUILDING SECRETARIES |               2

ACCOUNT DESC | BENFORD'S LAW OUTLIER COUNT
:--- | :---: 
CLASSROOM TEACHERS |               245
ADDITIONAL DIFFERENTIAL |          245
BUILDING SECRETARIES |             138
OFFICE SUPPLIES |                  118
ASSISTANT PRINCIPALS |              97
TUITION TO MASS SCHOOLS |           71
SPED OUTSIDE TRANSPORTATION |       13
TUITION TO NON PUBLIC SCHOOLS |     13
ATHLETIC SUPPLIES & MATS |           3
TAX TITLE |                          1



## Future Evaluation
---

Future evaluation will entail further tuning of the DBSCAN to approximate the classes by `AMOUNT DESC`. Data for 2018 Fiscal Year was obtained as well, and would serve as testing data to the above procedures.
