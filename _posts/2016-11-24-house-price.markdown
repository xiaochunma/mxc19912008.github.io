---
layout: post
title:  "House price"
h3:   "Description"
date:   2016-11-24
tech: R Python Tableau Excel
image: 'house-prices-up.jpg'
ghsource: 'house-prices-advanced-regression-techniques'
ghpages: 'test'

---
This is a house price prediction project on Kaggle.  
The dataset was compiled by Dean De Cock for use in data science education.  
This project is based on two datasets of train and test. The train dataset is of 1460 items with 80 variables and one especially important one of saleprice, and we want to make prediction on saleprice of test dataset.  
Following is a short exhibition of my work.  
  
<a href="#1">1. Correlation visualization with R packages(tabplots)</a>  
<a href="#2">2. Correlation visualization with R packages(corrplot, ggplot2)</a>   
<a href="#3">3. Feature engineering, selection, modeling and prediction</a>   
<a href="http://mxc19912008.github.io/house-price"> 4. Interesting interactive visualization using Tableau </a>  

  
    
      
        
<a name="1"> </a>  
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
      
        
<a name="2"> </a>  
<h5>2. Correlation visualization with R packages(corrplot, ggplot2)</h5> 
This section is to find strong-related numeric variables amoung each other to help us assure feature selection and better relate highly related variables for feature engineering.  
<img src="\images\cor-10-1.png">
Of all numeric variables, OverallQual, YearBuilt, YearRemodAdd, MasvnrArea, BsmtFinSF1, TotalBsmtSF, 1stFlrSF, GrLiveArea, FullBath, TotRmsAbvGrd, FirePlaces, GarageYrBlt, GarageCars, GarageArea, WoodDeskSF and OpenPorchSF show strong co-relationship with saleprice, which is in accordance with our conclusion above.   
Besides, because it is easy to judge the relationship of any two variables in this visualized correlation matrix, we can dig deeper to do feature engineering or something else interesting:)  
  
    
      
<a name="3"> </a>  
<h5>3. Feature engineering, selection, modeling and prediction </h5> 
<b> Ruling out outliers</b>  
Firstly, we drop outliers in the train data in case of imprecise prediction. To find outliers, we can make scatter plot with each numeric variable and saleprice, and find those extremely irregular ones. For example, in the GrLivArea variable, there are two obvious outliers when GrLivArea>4500, thus we can rule them out by setting GrLivArea<=4500, or drop out those that are bigger than 4500.   
<img src="\images\GrLivArea_outliers.png">  
<b> Dealing with missing values </b>  
Then, we need to combine train and test together using 'rbind' to uniformize factor levels in these two dataframe(be sure to give NAs to test$SalePrice), which will help us a lot when making predictions. By the way, it is also a good time to fill the missing values(except those in SalePrice of test). Here we can use 'mice' or 'rpart'. However, before this we need to decide the meanning of missing values, for example, missing values in "FireplaceQu" means no fireplace means no quality when we check those in "Fireplace", while "LotFrontage" misses its values maybe because of misses of records since a house should has its lot frontage. Below is how I deal with missing values with 'mice' and 'rpart' packages.  
This gives an example of 'mice' imputation  
<pre>
# Selecting the variables that need to be 'miced' 
namenacol <-c('LotFrontage', 'MasVnrType', 'MasVnrArea', 'MSZoning', 'Utilities' , 'BsmtFullBath', 'BsmtHalfBath'   , 'Functional', 'Exterior1st', 'Exterior2nd' , 'BsmtFinSF1', 'BsmtFinSF2', 'BsmtUnfSF', 'TotalBsmtSF', 'Electrical', 'KitchenQual', 'GarageCars', 'GarageArea', 'SaleType')   
full_m <- full[namenacol]  
# Do mice  
require(mice)  
imp.full <- mice(full_m, m=1, method='cart', printFlag=FALSE)  
full_imp <- complete(imp.full)  
</pre>
<img src="\images\Lot area.png">   
The imputed data (in red) appear to have a similar relationship to LotArea as the actual data (in blue), which means a good fit.  
This gives an example of 'rpart' imputation  
<pre>
area.rpart <- rpart(GarageArea ~ .,
                    data = full[!is.na(full$GarageArea),col.pred],
                    method = "anova",
                    na.action=na.omit)

full$GarageArea[is.na(full$GarageArea)] <- round(predict(area.rpart, full[is.na(full$GarageArea),col.pred]))
</pre>
<b> Feature engineering and selection </b>  
Let's do some feature engineering.  
<pre># Add up area of 1st floor and 2nd floor to get total floor area.  
full$FloorArea <- full$X1stFlrSF+full$X2ndFlrSF  
# Add up area of 1st floor, 2nd floor, low quality finshed area and above ground living area to get total living area.  
full$AllLivArea <- full$X1stFlrSF+full$X2ndFlrSF+full$LowQualFinSF+full$GrLivArea  
# Add up numbers of full bathrooms and 0.5* half bathrooms to get total bathroom.   
full$totalbath <- full$BsmtFullBath+0.5*full$BsmtHalfBath+full$FullBath+0.5*full$HalfBath  
# Add up all the porch area to get total porch area.  
full$totalPorchArea <- full$OpenPorchSF+full$EnclosedPorch+full$X3SsnPorch+full$ScreenPorch  </pre>  
Next, we want to give 'Ex', 'Gd', 'TA','Fa','Po' values for better modeling. This is easier with the map in Python, thus we use write.csv function to store current data for python.
<pre>qual_score = {None: 0, "Po": 1, "Fa": 2, "TA": 3, "Gd": 4, "Ex": 5}  
full["ExterQual"] = full["ExterQual"].map(qual_score).astype(int)...</pre>  
Now it's time to select features:  
we can use features we selected using visualization, or we can also use 'Boruta' to do it.  
<pre>boruta.train <- Boruta(SalePrice~., data = train_1, doTrace = 2)</pre>  
And the results are similar.    
  
<b> Model building </b>     
After we finished feature engineering and selection, we get rid of Id, which is of no use, and then the train and test data finally are to be divided.  
For randomForest: we firstly scale the numeric variables and then dummized the character(factor) variables.  
<pre> # get rid of Id
full_noid <- full[,-1]  
 # get the names of numeric variables (There is an easier way using sapply function)
numnames <-  c('MSSubClass','LotArea','OverallQual','OverallCond','YearBuilt','YearRemodAdd','X1stFlrSF','X2ndFlrSF','LowQualFinSF','GrLivArea','FullBath','HalfBath','BedroomAbvGr','KitchenAbvGr','TotRmsAbvGrd','Fireplaces','WoodDeckSF','OpenPorchSF','EnclosedPorch','X3SsnPorch','ScreenPorch','PoolArea','MiscVal','MoSold','YrSold','FloorArea','AllLivArea','OverallRate','totalPorchArea','LotFrontage','MasVnrArea','BsmtFinSF1','BsmtFinSF2','BsmtUnfSF','TotalBsmtSF','BsmtFullBath','BsmtHalfBath','GarageCars','GarageArea','subtractYearBuilt','subtractYearRemodAdd','subtractYrSold','totalbath')  
full_s <- full_noid[numnames]  
# Scale all the numeric data except saleprice  
full_ss <- apply(full_s, 2, function(x) scale(x))  
# Dummized all the character variables  
dummy_full <- model.matrix(~MSSubClass+LotFrontage+MasVnrArea+BsmtFinSF1+BsmtFinSF2+BsmtUnfSF+TotalBsmtSF+BsmtFullBath+BsmtHalfBath+GarageCars+GarageArea+subtractYearBuilt+subtractYearRemodAdd+subtractYrSold+totalbath+LotArea+OverallQual+OverallCond+YearBuilt+YearRemodAdd+X1stFlrSF+X2ndFlrSF+LowQualFinSF+GrLivArea+FullBath+HalfBath+BedroomAbvGr+KitchenAbvGr+TotRmsAbvGrd+Fireplaces+WoodDeckSF+OpenPorchSF+EnclosedPorch+X3SsnPorch+ScreenPorch+PoolArea+MiscVal+MoSold+YrSold+SalePrice+FloorArea+AllLivArea+OverallRate+totalPorchArea+MSZoning+Street+Alley+LotShape+LandContour+Utilities+LotConfig+LandSlope+Neighborhood+Condition1+Condition2+BldgType+HouseStyle+RoofStyle+RoofMatl+Exterior1st+Exterior2nd+MasVnrType+ExterQual+ExterCond+Foundation+BsmtQual+BsmtCond+BsmtExposure+BsmtFinType1+BsmtFinType2+Heating+HeatingQC+CentralAir+Electrical+KitchenQual+Functional+FireplaceQu+GarageType+GarageYrBlt+GarageFinish+GarageQual+GarageCond+PavedDrive+PoolQC+Fence+MiscFeature+SaleType+SaleCondition+SeasonSold-1,full_noid)
</pre>  
Let's do <b>randomForest</b>  
<pre>
require(randomForest)  
rf <- randomForest(SalePrice~.,train_1,do.trace=TRUE)  
prf <- predict(rf,test_1)</pre>  
For <b>Xgboost and Lasso</b>, we use caret package to do 'scale' and 'dummy' for us.  
<pre>
# take log for SalePrice to make it more normal
train$SalePrice <- log(label_df$SalePrice)
library(caret)
library(plyr)
library(xgboost)
library(Metrics)
library(glmnet)
# Create custom summary function in proper format for caret
custom_summary <- function(data, lev = NULL, model = NULL){
        out = rmsle(data[, "obs"], data[, "pred"])
        names(out) = c("rmse")
        out
}

# Create control object
control <- trainControl(method = "cv",  # Use cross validation
                       number = 5,     # 5-folds
                       summaryFunction = custom_summary                      
)
set.seed(12)
# Lasso
fit.glmnet <- train(SalePrice~., 
                    data=train, 
                    method="glmnet", 
                    metric="RMSE", 
                    preProc=c("center", "scale"), 
                    #tuneGrid = expand.grid(.alpha=c(0.008,0.01,0.03),.lambda=c(65500,65600,65700)),
                    trControl=control,
                    maximize = FALSE)
print(fit.glmnet)
pl<- predict(fit.glmnet, test)
epl <- exp(pl)

grid = expand.grid(nrounds=c(100, 200, 400, 800), # Test 4 values for boosting rounds
                   max_depth= c(4, 6),           # Test 2 values for tree depth
                   eta=c(0.1, 0.05, 0.025),      # Test 3 values for learning rate
                   gamma= c(0.1), 
                   colsample_bytree = c(1), 
                   min_child_weight = c(1),
                   subsample =0.5 )
set.seed(12)
xgb_tree_model =  train(SalePrice~., data=train, method="xgbTree", trControl=control,  tuneGrid=grid, metric="rmse", maximize = FALSE) 
px<- predict(xgb_tree_model, test)
epx <- exp(px)
# take average of these three models
ept <- (epx+epl+prf)/3
</pre>  
I got a score of 0.11975 on public leaderboard. There is a lot to be improved, for example, use more models.
<img src="\images\score.jpg">
