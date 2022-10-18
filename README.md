# Phase2 Project
## Goal of the Project
The goal of Phase 2 project is to model data using regression techniques and come up with some business
recommendataions. The data consists of house sales data of Kings County during 2014 and 2015. After looking into data, I was intrigued to find out how the age of house and features such as waterfront and view impact the price of house. I therefore looked at the effect of these variables and certain other parameters such as condition, bedrooms etc to see if these make an improvement in my model outcome. Alternatively, one could state that does including these varaibles in regression model minimizes the difference in predicted sale value and actual values, and thus provide insight into as to what features will help sellers to raise their house value.

## Data Set
The dataset contains inforamtion about the houses that were sold in year 2014 and 2015 in Kings County.
* The IMDB is an SQL database and had different tables connected by movie_id, and/or person id. These tables have lot of information like average ratings, directors, actors, etc etc. 

## Data Cleaning
  * Replaced  basement sq_footage values that were "?" and  NAN with 0 
  * Replaced NAN for waterfront with NO
  * Replaced NAN for view with UNKNOWN
  * Define "age" variable for house and also a "grade_val" which is numeric only value for grade value of house

## Modeling
### Features Selection
* Looked at the correlation heat-map to find out the most correlated feature with the house price Based on the heatmap, if two valriables had correlation above 0,7, I dropped one of them. Below I summarize the findings from heatmap.
* The most correalated feature with price is "sqft_living". This will be my most correlated feature and will be the variable for the baseline model.
* "sqft_living15" and "sqft_lot15" are strongly correlated with "sqft_living" and "sqft_lot" respectively. So I will drop "sqft_living15" and  "sqft_lot15 from the list of my variables that go into model
* yr_built and age are perfectly correlared as is obvious. I will use only "age"
* sqft_above and sqft_living are correlated, so will drop "sqft_above"
* "grad_val" is as they are strongly correlated with sqft_living and many other features. Therefore I dropped both grade_val and grade (since grade_val is essentially grade)
* bathrooms seem to be strongly correlated with sqft_living and so I dropped bathrooms as well
### Regression
* Looked at the individual distributions of variables, and also their scatter plot w.r.t. house prices. Almost all of the distributions deviate from normality and so using linear regression is not very justified. However we can still look at the linear regression and see what the results are. I therofore Built a baseline model using most correlated feature "sqft_living". 
* In the next step, I performed Log transformation on numerical variables and hot encoders for categorical variables.
* Another Baseline model with Log variables was built using most_correlated_feature="sqft_living_log"
* Added categorical features in the second model and then rest of the variables in successive steps

## Regression Results
Linear Model (R-squared Score):
   * Baseline Model (most correlated feature): Both train and test scores were around 0.50
   * Final Model: Both train and test scores were around 0.60

Log-transformed Model (R-squared Score)
   * Baseline Model (most correlated feature): Both train and test scores were around 0.45
   * Final Model: Both train and test scores were around 0.54
 
Even though R-scores are higher for non-log transformed data, we can clearly see in the qq plots below that linear Regression assumptions are violated and as such we can not rely on that model. Model with Log-tranformation has a much better qq plot distribution making us feel more comfortable about it. Looking at residual plots again confirms this as one can see homoscadesity in log-transformed data whereas non-log transformed data deviated from it.

![Linear Data](https://github.com/deepssharma/dsc-phase-2-project-v2-3/blob/main/figs/qqplot_linear.png "Linear Data") ![Log-Transformed data](https://github.com/deepssharma/dsc-phase-2-project-v2-3/blob/main/figs/qqplot_logdata.png "Log transformed data")

From Regression modeling we can say that price is most directly related to the living square footage of the house, and is the most important factor in price. But having additional features like waterfront, basement, number of bedrooms etc adds value to the house.

## Future Work
One can try to see if adding interaction terms will improve the model or not! Also, having more data as always will help figuring out things more quantitavely such as improvement from rennovations etc
