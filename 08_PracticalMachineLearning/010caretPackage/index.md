---
title       : The caret package
subtitle    : 
author      : Jeffrey Leek
job         : Johns Hopkins Bloomberg School of Public Health
logo        : bloomberg_shield.png
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow   # 
url:
  lib: ../../librariesNew
  assets: ../../assets
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---





## The caret R package

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/caret.png height=400>

[http://caret.r-forge.r-project.org/](http://caret.r-forge.r-project.org/)

---

## Caret functionality

* Some preprocessing (cleaning)
  * preProcess
* Data splitting
  * createDataPartition
  * createResample
  * createTimeSlices
* Training/testing functions
  * train
  * predict
* Model comparison
  * confusionMatrix

---

## Machine learning algorithms in R

* Linear discriminant analysis
* Regression
* Naive Bayes
* Support vector machines
* Classification and regression trees
* Random forests
* Boosting
* etc. 

---

## Why caret? 

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/predicttable.png height=250>

http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf


--- 

## SPAM Example: Data splitting


```r
library(caret); library(kernlab); data(spam)
inTrain <- createDataPartition(y=spam$type,
                              p=0.75, list=FALSE)
training <- spam[inTrain,]
testing <- spam[-inTrain,]
dim(training)
```

```
[1] 3451   58
```



--- 

## SPAM Example: Fit a model


```r
set.seed(32343)
modelFit <- train(type ~.,data=training, method="glm")
modelFit
```

```
Generalized Linear Model 

3451 samples
  57 predictors
   2 classes: 'nonspam', 'spam' 

No pre-processing
Resampling: Bootstrapped (25 reps) 

Summary of sample sizes: 3451, 3451, 3451, 3451, 3451, 3451, ... 

Resampling results

  Accuracy  Kappa  Accuracy SD  Kappa SD
  0.9       0.8    0.007        0.01    

 
```



--- 

## SPAM Example: Final model


```r
modelFit <- train(type ~.,data=training, method="glm")
modelFit$finalModel
```

```

Call:  NULL

Coefficients:
      (Intercept)               make            address                all              num3d  
        -1.88e+00          -1.96e-01          -1.40e-01          -2.86e-02           4.54e+00  
              our               over             remove           internet              order  
         6.43e-01           1.00e+00           1.83e+00           4.06e-01           3.41e-01  
             mail            receive               will             people             report  
         9.91e-02          -5.66e-01          -1.56e-01          -1.97e-01           7.99e-02  
        addresses               free           business              email                you  
         2.60e+00           1.08e+00           7.90e-01           1.72e-01           6.29e-02  
           credit               your               font             num000              money  
         1.91e+00           2.60e-01           1.62e-01           2.03e+00           6.57e-01  
               hp                hpl             george             num650                lab  
        -1.86e+00          -6.87e-01          -6.25e+00           4.41e-01          -1.79e+00  
             labs             telnet             num857               data             num415  
        -5.07e-01          -6.57e+00           1.25e+00          -8.79e-01          -3.26e-01  
            num85         technology            num1999              parts                 pm  
        -2.85e+00           9.00e-01          -1.54e-01          -5.80e-01          -7.17e-01  
           direct                 cs            meeting           original            project  
         1.65e+00          -4.94e+01          -2.43e+00          -1.44e+00          -2.37e+00  
               re                edu              table         conference      charSemicolon  
        -6.74e-01          -1.50e+00          -3.09e+00          -2.69e+00          -1.40e+00  
 charRoundbracket  charSquarebracket    charExclamation         charDollar           charHash  
        -1.11e-01          -9.18e-01           9.84e-01           6.04e+00           3.06e+00  
       capitalAve        capitalLong       capitalTotal  
         7.58e-02           8.03e-03           9.53e-04  

Degrees of Freedom: 3450 Total (i.e. Null);  3393 Residual
Null Deviance:	    4630 
Residual Deviance: 1290 	AIC: 1410
```



--- 

## SPAM Example: Prediction


```r
predictions <- predict(modelFit,newdata=testing)
predictions
```

```
   [1] spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam    nonspam
  [12] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    spam   
  [23] spam    spam    nonspam spam    spam    spam    spam    nonspam nonspam spam    spam   
  [34] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
  [45] nonspam spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
  [56] spam    spam    spam    spam    spam    spam    spam    nonspam nonspam nonspam spam   
  [67] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
  [78] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    nonspam
  [89] nonspam nonspam nonspam spam    spam    spam    spam    spam    spam    nonspam spam   
 [100] nonspam nonspam spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [111] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [122] spam    spam    spam    nonspam nonspam spam    spam    spam    spam    spam    spam   
 [133] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [144] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [155] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [166] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [177] spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam    spam   
 [188] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    nonspam
 [199] spam    spam    spam    spam    spam    spam    spam    spam    spam    nonspam nonspam
 [210] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [221] spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam    spam   
 [232] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [243] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [254] spam    spam    spam    spam    nonspam nonspam spam    spam    spam    spam    spam   
 [265] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [276] spam    spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam   
 [287] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [298] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    spam   
 [309] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [320] spam    spam    spam    spam    spam    nonspam spam    nonspam spam    spam    spam   
 [331] spam    spam    nonspam spam    nonspam spam    spam    spam    spam    spam    spam   
 [342] spam    spam    spam    spam    nonspam spam    spam    spam    spam    spam    spam   
 [353] spam    spam    spam    spam    spam    spam    spam    spam    spam    nonspam spam   
 [364] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [375] spam    spam    spam    spam    nonspam nonspam spam    nonspam spam    spam    spam   
 [386] spam    spam    spam    spam    spam    spam    spam    nonspam spam    spam    spam   
 [397] spam    spam    nonspam spam    spam    spam    spam    spam    spam    spam    spam   
 [408] spam    spam    spam    nonspam nonspam spam    spam    spam    nonspam spam    spam   
 [419] spam    nonspam spam    spam    nonspam spam    spam    nonspam spam    nonspam spam   
 [430] spam    nonspam spam    spam    spam    spam    spam    spam    spam    nonspam spam   
 [441] spam    spam    spam    spam    spam    spam    spam    spam    spam    spam    spam   
 [452] spam    spam    spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [463] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [474] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [485] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [496] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [507] nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [518] nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam
 [529] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [540] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [551] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [562] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [573] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [584] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [595] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [606] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [617] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [628] nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [639] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [650] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [661] nonspam nonspam nonspam spam    nonspam nonspam spam    nonspam nonspam nonspam nonspam
 [672] nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [683] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam
 [694] nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam
 [705] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [716] nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam
 [727] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [738] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [749] nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam
 [760] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [771] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [782] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [793] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [804] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [815] nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [826] nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam spam    nonspam nonspam
 [837] nonspam nonspam nonspam nonspam nonspam spam    spam    spam    nonspam nonspam nonspam
 [848] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [859] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [870] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [881] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [892] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [903] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [914] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [925] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [936] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [947] nonspam nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam
 [958] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [969] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [980] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
 [991] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1002] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1013] nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1024] spam    nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1035] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1046] nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam
[1057] nonspam nonspam nonspam nonspam nonspam spam    nonspam nonspam spam    nonspam nonspam
[1068] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam   
[1079] nonspam nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam
[1090] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam spam    spam    nonspam
[1101] spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1112] nonspam nonspam nonspam spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1123] spam    spam    nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1134] nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam nonspam
[1145] nonspam nonspam nonspam nonspam nonspam nonspam
Levels: nonspam spam
```


--- 

## SPAM Example: Confusion Matrix


```r
confusionMatrix(predictions,testing$type)
```

```
Confusion Matrix and Statistics

          Reference
Prediction nonspam spam
   nonspam     662   49
   spam         35  404
                                       
               Accuracy : 0.927        
                 95% CI : (0.91, 0.941)
    No Information Rate : 0.606        
    P-Value [Acc > NIR] : <2e-16       
                                       
                  Kappa : 0.846        
 Mcnemar's Test P-Value : 0.156        
                                       
            Sensitivity : 0.950        
            Specificity : 0.892        
         Pos Pred Value : 0.931        
         Neg Pred Value : 0.920        
             Prevalence : 0.606        
         Detection Rate : 0.576        
   Detection Prevalence : 0.618        
      Balanced Accuracy : 0.921        
                                       
       'Positive' Class : nonspam      
                                       
```


---

## Further information

* Caret tutorials:
  * [http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf](http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf)
  * [http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf](http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf)
* A paper introducing the caret package
  * [http://www.jstatsoft.org/v28/i05/paper](http://www.jstatsoft.org/v28/i05/paper)
