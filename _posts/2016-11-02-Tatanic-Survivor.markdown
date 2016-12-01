---
layout: post
title:  "Tatanic Survivor"
date:   2016-11-02
tech:  Using R and Tableau
image: 'Tatanic.jpg'
ghsource: 'titanic'
ghpages: 'titanic/data'


---

<i>On April 15, 1912, the RMS Titanic sank resulting in the loss of 1502 out of 2224 passengers and crew.  </i>  

<i>The goal here is to complete the analysis of what sorts of people were likely to survive in this disaster. </i>  

<i>We can use the tools of machine learning to predict which passengers survived the tragedy.</i>    

<i>This project is based on two datasets of train and test. The train dataset is of 891 items with 12 variables, and we want to make prediction on "survived" of test dataset.  </i>

Following is an exhibition of my work.  
  
<a href="#1">1. Feature engineering, all kinds of modeling and predictions</a>  
<a href="#2">2. Visualization to show who are survivors</a>    


<a name="1"> </a>  
<h5>1. Feature engineering, different kinds of modeling and predictions </h5>    
About data cleanning and feature engineering, currently I am doing no better than an excellent <a href="https://www.kaggle.com/mrisdal/titanic/exploring-survival-on-the-titanic"> tutorial </a>.   
My exhibition focus on how to build different models and their prediction scores on Kaggle public leaderboard. Here I tried four types of models that are commonly used for classification:Lasso, randomForest, SVM and Xgboost. Among them, Lasso showed the  highest score.
<pre>  
<b># Lasso score =  0.80383</b>
set.seed(12)
fit.glmnet <- train(Survived~Pclass + Sex + Age + SibSp + Parch + Fare +
                            Embarked + Title + FamilySize + FamilyID, 
                    data=train, 
                    method="glmnet",
                    trControl=trainControl(method = "cv",number = 5))
print(fit.glmnet)
Prediction_l <- predict(fit.glmnet, test, OOB=TRUE)
submit <- data.frame(PassengerId = test$PassengerId, Survived = Prediction_l)
write.csv(submit, file = "Lasso.csv", row.names = FALSE)
</pre>  
<pre> 
<b># randomForest score = 0.77512 </b>

fit.rf <- train(Survived~Pclass + Sex + Age + SibSp + Parch + Fare +
                            Embarked + Title + FamilySize + FamilyID, 
                    data=train, 
                    method="rf",
                    trControl=trainControl(method = "cv",number = 5))
print(fit.rf)

Prediction_rf <- predict(fit.rf, test, OOB=TRUE)
submit <- data.frame(PassengerId = test$PassengerId, Survived = Prediction_rf)
write.csv(submit, file = "RandomForest.csv", row.names = FALSE)
</pre>  
<pre> 
<b># SVM Score = 0.69378</b>

fit.SVM <- train(Survived~Pclass + Sex + Age + SibSp + Parch + Fare +
                            Embarked + Title + FamilySize + FamilyID, 
                    data=train, 
                    method="svmRadial",
                    trControl=trainControl(method = "cv",number = 5))
print(fit.SVM)
Prediction_SVM <- predict(fit.SVM, test, OOB=TRUE)
submit <- data.frame(PassengerId = test$PassengerId, Survived = Prediction_SVM)
write.csv(submit, file = "SVM.csv", row.names = FALSE)
</pre>  
<pre> 
<b># Xgboost Score = 0.77033</b>

fit.XGB <- train(Survived~Pclass + Sex + Age + SibSp + Parch + Fare +
                         Embarked + Title + FamilySize + FamilyID, 
                 data=train, 
                 method="xgbTree",
                 trControl=trainControl(method = "cv",number = 5))
print(fit.XGB)
Prediction_XGB <- predict(fit.XGB, test, OOB=TRUE)
submit <- data.frame(PassengerId = test$PassengerId, Survived = Prediction_XGB)
write.csv(submit, file = "XGB.csv", row.names = FALSE)  
</pre>  
<a name="2"> </a>  
<h5>2. Visualization to show who are survivors </h5>  
To better explain those who have bigger chances to survive, I made these pictures.
<img src="\images\Sex.jpg">
<img src="\images\Age.jpg">
<img src="\images\Economic.jpg"><img src="\images\Family Size.jpg">

