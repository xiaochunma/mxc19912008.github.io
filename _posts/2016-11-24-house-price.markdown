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
<iframe> <div class='tableauPlaceholder' id='viz1480174884856' style='position: relative'><noscript><a href='#'><img alt='Does newly built or  remodeled houses sell better? ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;T2&#47;T2JTQ76C8&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='path' value='shared&#47;T2JTQ76C8' /> <param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;T2&#47;T2JTQ76C8&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1480174884856');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script></iframe>
