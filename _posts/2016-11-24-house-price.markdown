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
## 1. Correlation visualization with R packages(tabplots)_inspired by Laurae@Kaggle   
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

