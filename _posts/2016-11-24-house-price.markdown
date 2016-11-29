---
layout: post
title:  "House price"
h3:   "Description"
date:   2016-11-24
tech: R Python Tableau Excel
image: 'house-prices-up.jpg'
ghsource: 'house-prices-advanced-regression-techniques'
ghpages: 'house-prices-advanced-regression-techniques/data'
image_1: '1.npg'

---
This is a house price prediction project on Kaggle.  
The dataset was compiled by Dean De Cock for use in data science education.  
This project is based on two datasets of train and test. The train dataset is of 1460 items with 80 variables and one especially important one of saleprice, and we want to make prediction on saleprice of test dataset.  
Following is a short exhibition of my work on visualization.  
<h5>1. Correlation visualization with R packages(tabplots)_inspired by Laurae@Kaggle</h5>  
This part aims to find strong-related variables amoung 80 variables to Saleprice which further help us do feature selection and engineering.  This was drew with R package(tabplots), and the following set of pictures show the number and range of values for each variable as well as the covariance among the variables(both numeric and characters), sepecially with log_saleprice, which was on the right of every row.
    <img src="\images\1.png">
    <img src="\images\2.png">
    <img src="\images\3.png">
    <img src="\images\4.png">
    <img src="\images\5.png">
    <img src="\images\6.png">
    <img src="\images\7.png">
    <img src="\images\8.png">
    <img src="\images\9.png">
    <img src="\images\10.png">
    <img src="\images\11.png">
    <img src="\images\12.png">
    <img src="\images\13.png">
    <img src="\images\14.png">
    <img src="\images\15.png">
    <img src="\images\16.png">
    Of all variables, OverallQual, YearBuilt, YearRemodAdd, MasvnrArea, BsmtFinSF1, TotalBsmtSF, BsmtFullBath, (X)1stFlrSF, GrLiveArea, FullBath, HalfBath, TotRmsAbvGrd, TotRmsAbvGrd, FirePlaces, GarageYrBlt, GarageCars, GarageArea, WoodDeskSF, OpenPorchSF, MSZonging, LotShape, MasVnrType, BsmtExposure, HeatingQC, KitchenQual, FireplaceQu, GarageFinsh, SaleType and SaleCondition <b>show good shape resemblance or anti-similarity.</b>  
    This means these variables are of <b>good correlations with Saleprice.</b>  
<h5>2. Correlation visualization with R packages(corrplot, ggplot2)</h5> 
This section is to find strong-related numeric variables amoung each other to help us assure feature selection and better relate highly related variables for feature engineering.  
<img src="\images\cor-10-1.png">
Of all numeric variables, OverallQual, YearBuilt, YearRemodAdd, MasvnrArea, BsmtFinSF1, TotalBsmtSF, 1stFlrSF, GrLiveArea, FullBath, TotRmsAbvGrd, FirePlaces, GarageYrBlt, GarageCars, GarageArea, WoodDeskSF and OpenPorchSF show strong co-relationship with saleprice, which is in accordance with our conclusion above.   
Besides, because it is easy to judge the relationship of any two variables in this visualized correlation matrix, we can dig deeper to do feature engineering or something else interesting:)  
<h5>3. Feature engineering, selection, modeling and prediction </h5>   
Firstly, we drop outliers in the train data in case of imprecise prediction. To find outliers, we can make scatter plot with each numeric variable and saleprice, and find those extremely irregular ones. For example, in the GrLivArea variable, there are two obvious outliers when GrLivArea>4500, thus we can rule them out by setting GrLivArea<=4500, or drop out those that are bigger than 4500.   
<img src="\images\GrLivArea_outliers.png">
Then, we need to combine train and test together using 'rbind' to uniformize factor levels in these two dataframe(be sure to give NAs to test$SalePrice), which will help us a lot when making predictions. By the way, it is also a good time to fill the missing values(except those in SalePrice of test). Here we can use 'mice' or 'rpart'. However, before this we need to decide the meanning of missing values, for example, missing values in "FireplaceQu" means no fireplace means no quality when we check those in "Fireplace", while "LotFrontage" misses its values maybe because of misses of records since a house should has its lot frontage. Below is how I deal with missing values with 'mice' and 'rpart' packages.
```R
namenacol <-c('LotFrontage', 'MasVnrType', 'MasVnrArea', 'MSZoning', 'Utilities' , 'BsmtFullBath', 'BsmtHalfBath'   , 'Functional', 'Exterior1st', 'Exterior2nd' , 'BsmtFinSF1', 'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'Electrical', 'KitchenQual', 'GarageCars', 'GarageArea', 'SaleType') 
full_m <- full[namenacol]

require(mice)
imp.full <- mice(full_m, m=1, method='cart', printFlag=FALSE)
full_imp <- complete(imp.full)
```
