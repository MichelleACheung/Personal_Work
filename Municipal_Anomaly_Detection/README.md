# Detecting Anomalies in Municipal Expenditures
---

## Directory Structure
---

```
.
+-- Final_Report.md
+-- README.md
+-- data
+-- notebooks

```

## Project Outline
---

Mangement of municipal finances yields large amounts of data annually. Detection of transaction anomoalies proves difficult with time and resources available given the amount of data.

Data analytics and machine learning can help process and evaluate the large amount of municipal financial transactions to find means of cost efficiency as well as any outliers that may be of fraudulent nature.

This project includes the in-depth analysis and machine learning of a municipal's financial accounts. Benford's Law was applied to explore the data and a DBSCAN clusterning algorithm was used as an additional anomaly detector.

## Directory Outline
---

+ **Final_Report.md**
> A technical summary of the project.

+ **README.md**
> Summary introduction to project.

+ **data** folder
> - An `XML` file is provided by the City of Revere, Massachusetts for the 2017 fiscal year (June 30, 2016 through July 1, 2017). Within this `XML` file, data includes all transactions seen by the City Auditor made in the reference fiscal year.
>>`FY17 DETAIL HISTORY TB AS OF PERIOD 13.xlsx`
> - An additional `XML` file is included which shows the amounts of outliers found per account with different values of DBSCAN epsilon values.
>>`dbscan_eps.xlsx`

+ **notebooks** folder
> - Jupyter notebook which includes exploration, analysis, and modeling of data for anomaly detection.
>> `2017FY_Data_and_Model.ipynb`
