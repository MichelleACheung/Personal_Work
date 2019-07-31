# DSI-7 Project 2 #

## Problem Statement ##

Through creating a regression model based on the Ames Housing Dataset, predict the price of a house at sale..

## Sources of Data ##

- Ames, Iowa Home Sales Data from 2006-2010
 - https://www.kaggle.com/c/dsi-us-7-project-2-regression-challenge


### Executive Summary ###

Linear regression model was used to predict home sale price for the city of Ames, Iowa. An iteratation of feature engineering was executed in order to build a predictive model. As the dataset was found to not be perfectly linear, where target data was found to not have a completely normal distribution, the mdoel created was overfit and decreased in accuracy as home sale price was expected to reach over approximatlely $300,000.

### Data Library ###

|Feature|Type|Dataset|Description|
|---|---|---|---|
|Id|int64|train|Home ID number| 
|PID|int64|train|Purchase ID|
|MS SubClass|int64|train|The building class|
|MS Zoning|object|train|Identifies the general zoning classification of the sale.|
|Lot Frontage|float64|train|Linear feet of street connected to property.|
|Lot Area|int64|train|Lot size in square feet.|
|Street|object|train|Type of road access to property.|
|Alley|object|train|Type of alley access to property.|
|Lot Shape|object|train|General shape of property.|
|Land Contour|object|train|Flatness of the property.|
|Utilities|object|train|Type of utilities available|
|Lot Config|object|train|Lot configuration.|
|Land Slope|object|train|Slope of property.|
|Neighborhood|object|train|Physical locations within Ames city limits.|
|Condition 1|object|train|Proximity to main road or railroad.|
|Condition 2|object|train|Proximity to main road or railroad (if a second is present).|
|Bldg Type|object|train|Type of dwelling.|
|House Style|object|train|Style of dwelling.|
|Overall Qual|int64|train|Overall material and finish quality.|
|Overall Cond|int64|train|Overall condition rating.|
|Year Built|int64|train|Original construction date.|
|Year Remod/Add|int64|train|Remodel date (same as construction date if no remodeling or additions).|
|Roof Style|object|train|Type of roof.|
|Roof Matl|object|train|Roof material.|
|Exterior 1st|object|train|Exterior covering on house.|
|Exterior 2nd|object|train|Exterior covering on house (if more than one material).|
|Mas Vnr Type|object|train|Masonry veneer type.|
|Mas Vnr Area|float64|train|Masonry veneer area in square feet.|
|Exter Qual|int64|object|Exterior material quality.|
|Exter Cond|int64|object|Present condition of the material on the exterior|
|Foundation|int64|object|Type of foundation.|
|Bsmt Qual|int64|object|Height of the basement.|
|Bsmt Cond|int64|object|General condition of the basement.|
|Bsmt Exposure|int64|object|Walkout or garden level basement walls.|
|BsmtFin Type 1|int64|object|Quality of basement finished area.|
|BsmtFin SF 1|int64|float64|Type 1 finished square feet.|
|BsmtFin Type 2|int64|object|Quality of second finished area (if present).|
|BsmtFin SF 2|float64|train|Type 2 finished square feet.|
|Bsmt Unf SF|float64|train|Unfinished square feet of basement area.|
|Total Bsmt SF|float64|train|Total square feet of basement area.|
|Heating|object|train|Type of heating.|
|Heating QC|object|train|Heating quality and condition.|
|Central Air|object|train|Central air conditioning|
|Electrical|object|train|Electrical system|
|1st Flr SF|int64|train|First Floor square feet|
|2nd Flr SF|int64|train|Second floor square feet|
|Low Qual Fin SF|int64|train|Low quality finished square feet (all floors)|
|Gr Liv Area|int64|train|Above grade (ground) living area square feet|
|Bsmt Full Bath|float64|train|Basement full bathrooms|
|Bsmt Half Bath|float64|train|Basement half bathrooms|
|Full Bath|int64|train|Full bathrooms above grade|
|Half Bath|int64|train|Half baths above grade|
|Bedroom AbvGr|int64|train|Number of bedrooms above basement level|
|Kitchen AbvGr|int64|train|Number of kitchens|
|Kitchen Qual|object|train|Kitchen quality|
|TotRms AbvGrd|int64|train|Total rooms above grade (does not include bathrooms)|
|Functional|object|train|Home functionality rating|
|Fireplaces|int64|train|Number of fireplaces|
|Fireplace Qu|object|train|Fireplace quality|
|Garage Type|object|train|Garage location|
|Garage Yr Blt|float64|train|Year garage was built|
|Garage Finish|object|train|Interior finish of the garage|
|Garage Cars|float64|train|Size of garage in car capacity|
|Garage Area|float64|train|Size of garage in square feet|
|Garage Qual|object|train|Garage quality|
|Garage Cond|object|train|Garage condition|
|Paved Drive|object|train| Paved driveway|
|Wood Deck SF|int64|train|Wood deck area in square feet|
|Open Porch SF|int64|train|Open porch area in square feet|
|Enclosed Porch|int64|train|Enclosed porch area in square feet|
|3Ssn Porch|int64|train|Three season porch area in square feet|
|Screen Porch|int64|train|Screen porch area in square feet|
|Pool Area|int64|train|Pool area in square feet|
|Pool QC|object|train|Pool quality|
|Fence|object|train|Fence quality|
|Misc Feature|object|train|Miscellaneous feature not covered in other categories|
|Misc Val|int64|train|$Value of miscellaneous feature|
|Mo Sold|int64|train|Month Sold|
|Yr Sold|int64|train|Year Sold|
|Sale Type|object|train|Type of sale|
|SalePrice|int64|train|Property's sale price in dollars.|

### Procedure ###

Home sale data from 2006 to 2010 was imported and cleaned. The target was identifided as "SalePrice." Nan cells were inspected for quantitative and qualitative features. Values of zero was replaced for NaN values in quantitative data. Similarly, "Unknown" was replaced for NaN values in qualitative data. The feature, "Pool QC" was dropped as number of observations was below 30. A histogram was plotted for the target, "SalePrice" and a skewed-right normal distribution was found.

Quantitative features were assessed and engineered separateley from qualitative features. The following was done for each.

For quantitative features, histograms were plotted for each feature. Interaction features that could strengthen predictors were identified and created. The feature "Year Built" was subtracted from "Yr Sold" to create "house_age." The features "1st Flr SF," "2nd Flr SF," "BsmtFin SF 1,"  and "BsmtFin SF 2" were added to create "total_fin_sq_ft." The features "Garage Area" and "Garage Cars" were multiplied to create "garage_size." Lastly, the features "Overall Qual" and "Overall Cond" was added to create "overall_grade."

Date features were dummied: "Year Built," "Year Remod/Add," "Garage Yr Blt," "Mo Sold," and "Yr Sold."

A correlation heatmap was then plotted for all quantitative features. No quantitative features were dropped, subsequently.

Qualitative features were separated by variance in median sale price between subcategories. For example, when looking at "LandSlope," minimum median sale price found among subcategories was subtracted from maximum median sale price found among subcategories. Using the standard deviation of the "SalePrice" distribution, if the variance in the feature median sale price was found to be less than 2 * standard deviations the feature was dropped. If the variance in the feature median sale price was greater than 2 * standard deviations, the feature was kept and dummied for the model.

### Conclusion ###

The linear regression model, withough regularization, resulted in a R^2 train score of 0.937556947016988 and an R^2 test score of -1.1731971079913591e+21 (overfit). With regularization, ridge train score of 0.9351073027711895 and ridge test score of 0.8741537280823408; Lasso train score of 0.8897174607263378 and lasso test score of 0.9035302861767349.

### Included in Project Submission ###
- Technical Report: Project_2_MCheung.ipynb
- README.md: Introduction to and overview of project
- Data files:
 - train.csv
- Presentation: Project 2 Presentation