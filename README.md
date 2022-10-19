# Phase2 Project
## Goal of the Project
The goal of Phase 2 project is to model data using regression techniques and come up with some business
recommendataions. The data consists of house sales data of Kings County during 2014 and 2015. After looking into data, I was intrigued to find out how the age of house and features such as waterfront and view impact the price of house. I therefore looked at the effect of these variables and certain other parameters such as condition, bedrooms etc to see if these make an improvement in my model outcome. Alternatively, one could state that does including these varaibles in regression model minimizes the difference in predicted sale value and actual values, and thus provide insight into as to what features will help sellers to raise their house value.
Specifically I an define the following problem for a real estate agency(or a homeowner who wants to sell his house)interested in investing into houses

**What kind of houses the agency should invest in to maximize profits**

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
* "grad_val" is strongly correlated with sqft_living and many other features. Initially I dropped both grade_val and grade (since grade_val is essentially grade), however given that grade is such an important feature, I included it in regression and compared qq plots and residuals. They looked essentially the same as without including it, but model gave higher R2. So I include it in final model.
* bathrooms seem to be strongly correlated with sqft_living and so I dropped bathrooms as well
### Regression
* Looked at the individual distributions of variables, and also their scatter plot w.r.t. house prices. Almost all of the distributions deviate from normality and so using linear regression is not very justified. However we can still look at the linear regression and see what the results are. I therofore Built a baseline model using most correlated feature "sqft_living". 
* In the next step, I performed Log transformation on numerical variables and hot encoders for categorical variables.
* Another Baseline model with Log variables was built using most_correlated_feature="sqft_living_log"
* Added categorical features in the second model and then rest of the variables in successive steps

## Regression Results
Linear Model (R-squared Score):
   * Baseline Model (most correlated feature): Both train and test scores were around 0.50
   * Final Model: Both train and test scores were around 0.60 (0.66 if I include grade_val)

Log-transformed Model (R-squared Score)
   * Baseline Model (most correlated feature): Both train and test scores were around 0.45
   * Final Model: Both train and test scores were around 0.54 (0.62 if I include grade_val)
 
Even though R-scores are higher for non-log transformed data, we can clearly see in the qq plots below that linear Regression assumptions are violated and as such we can not rely on that model. Model with Log-tranformation has a much better qq plot distribution making us feel more comfortable about it. Looking at residual plots again confirms this as one can see homoscadesity in log-transformed data whereas non-log transformed data deviated from it.

![Linear Data](https://github.com/deepssharma/dsc-phase-2-project-v2-3/blob/main/figs/qqplot_linear.png "Linear Data") ![Log-Transformed data](https://github.com/deepssharma/dsc-phase-2-project-v2-3/blob/main/figs/qqplot_logdata.png "Log transformed data")


From Regression modeling we can say that price is most directly related to the living square footage of the house, and is the most important factor in price. But having additional features like waterfront, inceasing the grade, etc adds value to the house.

## Regression Co-efficients
* For the log transformed model we got a R2 of ~0.63, which means that our data can explain ~ 63% variances as seen in data. The p-values for all the features were very very small (<<0.0001) which means all the features have some effects on price.
* the co-efficient for sqft_living_log is ~0.48, meaning that given all other variables are constant, an increase of 1% in sqft_living area will result a 0.5% increase in price
* the co-efficient for grade_val_log is ~1.6, meaning that under the assumption all other variables are constant, an improvement in grade by 1% will result an increase of 1.6% in the house price.
* waterfront is ~0.39, since waterfront is a binary variable, this means that having a waterfront will increase the house price by 48% ((exp^0.39-1)*100).

## Final Recommendations
We see that living square footage is very strongly correlated to house price, and also that houses which have higher grade quality, meaning the construction standards get sold for higher prices. But looking at the distributions of grade values, we see that most of the houses that get sold are grades 7 and 8. Looking at the distribution of square footage vs grades, we can see that there are many houses in grades 5 and 6 that have same square footage as the houses in grades 7 and 8. I would therefore recommend that renovating houses in grades 5 and 6 with similar square footage as grades 7 and 8 would be profitable for real estate firm looking to invest.

![Business Recommendation ](https://github.com/deepssharma/dsc-phase-2-project-v2-3/blob/main/figs/business_recommendation.png)

## Future Work
One can try to see if adding interaction terms will improve the model or not! Also, having more data as always will help figuring out things more quantitavely such as improvement from rennovations etc
