# Week 1 Practical Machine Learning

# Lesson (1) Prediction Motivation

## About this course

* This course covers the basic ideas behind machine learning/prediction
  * Study design - training vs. test sets
  * Conceptual issues - out of sample error, ROC curves
  * Practical implementation - the caret package
* What this course depends on
  * The Data Scientist's Toolbox
  * R Programming
* What would be useful
  * Exploratory analysis
  * Reporting Data and Reproducible Research
  * Regression models

---

## Who predicts?

* Local governments -> pension payments
* Google -> whether you will click on an ad
* Amazon -> what movies you will watch
* Insurance companies -> what your risk of death is
* Johns Hopkins -> who will succeed in their programs

---


## Why predict? Glory!

[http://www.zimbio.com/photos/Chris+Volinsky](http://www.zimbio.com/photos/Chris+Volinsky)

---

## Why predict? Riches!

[http://www.heritagehealthprize.com/c/hhp](http://www.heritagehealthprize.com/c/hhp)

---

## Why predict? For sport!

[http://www.kaggle.com/](http://www.kaggle.com/)

---

## Why predict? To save lives!

[http://www.oncotypedx.com/en-US/Home](http://www.oncotypedx.com/en-US/Home)

---

## A useful (if a bit advanced) book

[The elements of statistical learning](http://statweb.stanford.edu/~tibs/ElemStatLearn/)

---

## A useful package

[http://caret.r-forge.r-project.org/](http://caret.r-forge.r-project.org/)

---

## Machine learning (more advanced material)

[https://www.coursera.org/course/ml](https://www.coursera.org/course/ml)


---

## Even more resources

* [List of machine learning resources on Quora](http://www.quora.com/Machine-Learning/What-are-some-good-resources-for-learning-about-machine-learning-Why)
* [List of machine learning resources from Science](http://www.sciencemag.org/site/feature/data/compsci/machine_learning.xhtml)
* [Advanced notes from MIT open courseware](http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-867-machine-learning-fall-2006/lecture-notes/)
* [Advanced notes from CMU](http://www.stat.cmu.edu/~cshalizi/350/)
* [Kaggle - machine learning competitions](http://www.kaggle.com/)

## Lesson (2) What is Prediction ?


## The central dogma of prediction

<img class="center" src=../../assets/img/08_PredictionAndMachineLearning/centraldogma.png height=450>

---

## What can go wrong

<img class="center" src=../../assets/img/08_PredictionAndMachineLearning/googleflu.png height=450>


http://www.sciencemag.org/content/343/6176/1203.full.pdf

---

## Components of a predictor

</br>

<center> question -> input data -> features -> algorithm -> parameters -> evaluation  </center>


---

## SPAM Example

</br>

<center> <redtext>question</redtext> -> input data -> features -> algorithm -> parameters -> evaluation  </center>

</br>

__Start with a general question__

Can I automatically detect emails that are SPAM that are not?

__Make it concrete__

Can I use quantitative characteristics of the emails to classify them as SPAM/HAM?


---

## SPAM Example

</br>

<center> question -> <redtext>input data </redtext> -> features -> algorithm -> parameters -> evaluation  </center>

<img class=center src=../../assets/img/spamR.png height='400' />

[http://rss.acs.unt.edu/Rdoc/library/kernlab/html/spam.html](http://rss.acs.unt.edu/Rdoc/library/kernlab/html/spam.html)



---

## SPAM Example

</br>

<center> question -> input data  -> <redtext>features</redtext> -> algorithm -> parameters -> evaluation  </center>

</br>



<b>
Dear Jeff, 

Can you send me your address so I can send you the invitation? 

Thanks,

Ben
</b>




---

## SPAM Example

</br>

<center> question -> input data  -> <redtext>features</redtext> -> algorithm -> parameters -> evaluation  </center>

</br>

<b> 

Dear Jeff, 

Can <rt>you</rt> send me your address so I can send <rt>you</rt> the invitation? 

Thanks,

Ben
</b>

</br>

Frequency of you $= 2/17 = 0.118$

---

## SPAM Example

</br>

<center> question -> input data  -> <redtext>features</redtext> -> algorithm -> parameters -> evaluation  </center>


```{r loadData}
library(kernlab)
data(spam)
head(spam)

```


---

## SPAM Example


<center> question -> input data  -> features -> <redtext>algorithm</redtext> -> parameters -> evaluation  </center>

```{r,dependson="loadData",fig.height=3.5,fig.width=3.5}
plot(density(spam$your[spam$type=="nonspam"]),
     col="blue",main="",xlab="Frequency of 'your'")
lines(density(spam$your[spam$type=="spam"]),col="red")
```

---

## SPAM Example


<center> question -> input data  -> features -> <redtext>algorithm</redtext> -> parameters -> evaluation  </center>

</br></br>

__Our algorithm__

* Find a value $C$. 
* __frequency of 'your' $>$ C__ predict "spam"

---

## SPAM Example


<center> question -> input data  -> features -> algorithm -> <redtext>parameters</redtext> -> evaluation  </center>

```{r,dependson="loadData",fig.height=3.5,fig.width=3.5}
plot(density(spam$your[spam$type=="nonspam"]),
     col="blue",main="",xlab="Frequency of 'your'")
lines(density(spam$your[spam$type=="spam"]),col="red")
abline(v=0.5,col="black")
```

---

## SPAM Example


<center> question -> input data  -> features -> algorithm -> parameters -> <redtext>evaluation</redtext></center>

```{r,dependson="loadData",fig.height=3.5,fig.width=3.5}
prediction <- ifelse(spam$your > 0.5,"spam","nonspam")
table(prediction,spam$type)/length(spam$type)
```

Accuracy$ \approx 0.459 + 0.292 = 0.751$

# Lesson (3) Relative Importance


## Relative order of importance

</br>
</br>
</br>

<center> question > data > features > algorithms </center>


---

## An important point

<q>The combination of some data and an aching desire for an answer does not ensure that a reasonable answer can be extracted from a given body of data.</q>

<center> John Tukey </center>




---

## Garbage in = Garbage out

<center> question -> <rt>input data</rt> -> features -> algorithm -> parameters -> evaluation  </center>

1. May be easy (movie ratings -> new movie ratings)
2. May be harder (gene expression data -> disease)
3. Depends on what is a "good prediction".
4. Often [more data > better models](http://www.youtube.com/watch?v=yvDCzhbjYWs)
5. The most important step!


---

## Features matter!

<center> question -> input data -> <rt>features</rt> -> algorithm -> parameters -> evaluation  </center>

__Properties of good features__

* Lead to data compression
* Retain relevant information
* Are created based on expert application knowledge

__Common mistakes__

* Trying to automate feature selection
* Not paying attention to data-specific quirks
* Throwing away information unnecessarily

---

## May be automated with care

<center> question -> input data -> <rt>features</rt> -> algorithm -> parameters -> evaluation  </center>

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/autofeatures.jpeg height=300>

[http://arxiv.org/pdf/1112.6209v5.pdf](http://arxiv.org/pdf/1112.6209v5.pdf)

---

## Algorithms matter less than you'd think

<center> question -> input data -> features -> <rt>algorithm</rt> -> parameters -> evaluation  </center>

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/illusiontable.png height=400>

[http://arxiv.org/pdf/math/0606441.pdf](http://arxiv.org/pdf/math/0606441.pdf)

---

## Issues to consider

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/mlconsiderations.jpg height=400>

[http://strata.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html](http://strata.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html)

---

## Prediction is about accuracy tradeoffs


* Interpretability versus accuracy
* Speed versus accuracy
* Simplicity versus accuracy
* Scalability versus accuracy

---

## Interpretability matters

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/interpretable.png height=150>

</br></br></br>

[http://www.cs.cornell.edu/~chenhao/pub/mldg-0815.pdf](http://www.cs.cornell.edu/~chenhao/pub/mldg-0815.pdf)

---

## Scalability matters

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/netflixno.png height=250>
</br></br></br>

[http://www.techdirt.com/blog/innovation/articles/20120409/03412518422/](http://www.techdirt.com/blog/innovation/articles/20120409/03412518422/)

[http://techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html](http://techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html)


# Lesson Out of Sample Error (4)

## In sample versus out of sample

__In Sample Error__: The error rate you get on the same
data set you used to build your predictor. Sometimes
called resubstitution error.

__Out of Sample Error__: The error rate you get on a new
data set. Sometimes called generalization error. 

__Key ideas__

1. Out of sample error is what you care about
2. In sample error $<$ out of sample error
3. The reason is overfitting
  * Matching your algorithm to the data you have

## In sample versus out of sample errors

## What's going on? 

<center><rt> Overfitting </rt></center>

* Data have two parts
  * Signal
  * Noise
* The goal of a predictor is to find signal
* You can always design a perfect in-sample predictor
* You capture both signal + noise when you do that
* Predictor won't perform as well on new samples

[http://en.wikipedia.org/wiki/Overfitting](http://en.wikipedia.org/wiki/Overfitting)

# Lesson (5) Prediction study design 

## Prediction study design

1. Define your error rate
2. Split data into:
  * Training, Testing, Validation (optional)
3. On the training set pick features
  * Use cross-validation
4. On the training set pick prediction function
  * Use cross-validation
6. If no validation 
  * Apply 1x to test set
7. If validation
  * Apply to test set and refine
  * Apply 1x to validation 

## Know the benchmarks

[http://www.heritagehealthprize.com/c/hhp/leaderboard](http://www.heritagehealthprize.com/c/hhp/leaderboard)

## Study design

[http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf](http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf)

## Used by the professionals

[http://www.kaggle.com/](http://www.kaggle.com/)

## Avoid small sample sizes

* Suppose you are predicting a binary outcome 
  * Diseased/healthy
  * Click on ad/not click on ad 
* One classifier is flipping a coin
* Probability of perfect classification is approximately:
  * $\left(\frac{1}{2}\right)^{sample \; size}$
  * $n = 1$ flipping coin 50% chance of 100% accuracy
  * $n = 2$ flipping coin 25% chance of 100% accuracy
  * $n = 10$ flipping coin 0.10% chance of 100% accuracy

## Rules of thumb for prediction study design

* If you have a large sample size
  * 60% training
  * 20% test
  * 20% validation
* If you have a medium sample size
  * 60% training
  * 40% testing
* If you have a small sample size
  * Do cross validation
  * Report caveat of small sample size

## Some principles to remember

* Set the test/validation set aside and _don't look at it_
* In general _randomly_ sample training and test
* Your data sets must reflect structure of the problem
  * If predictions evolve with time split train/test in time chunks (called[backtesting](http://en.wikipedia.org/wiki/Backtesting) in finance)
* All subsets should reflect as much diversity as possible
  * Random assignment does this
  * You can also try to balance by features - but this is tricky

# Lesson (6) Types of Errors 
## Basic terms

In general, __Positive__ = identified and __negative__ = rejected. Therefore:
__True positive__ = correctly identified
__False positive__ = incorrectly identified
__True negative__ = correctly rejected
__False negative__ = incorrectly rejected
_Medical testing example_:
__True positive__ = Sick people correctly diagnosed as sick
__False positive__= Healthy people incorrectly identified as sick
__True negative__ = Healthy people correctly identified as healthy
__False negative__ = Sick people incorrectly identified as healthy.

[http://en.wikipedia.org/wiki/Sensitivity_and_specificity](http://en.wikipedia.org/wiki/Sensitivity_and_specificity)

---

## Key quantities

[http://en.wikipedia.org/wiki/Sensitivity_and_specificity](http://en.wikipedia.org/wiki/Sensitivity_and_specificity)

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## Key quantities as fractions

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## Screening tests

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## General population

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## General population as fractions

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## At risk subpopulation

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## At risk subpopulation as fraction

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## Key public health issue 

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## Key public health issue 

---

## For continuous data

__Mean squared error (MSE)__:

$$\frac{1}{n} \sum_{i=1}^n (Prediction_i - Truth_i)^2$$

__Root mean squared error (RMSE)__:

$$\sqrt{\frac{1}{n} \sum_{i=1}^n(Prediction_i - Truth_i)^2}$$

---

## Common error measures

1. Mean squared error (or root mean squared error)
  * Continuous data, sensitive to outliers
2. Median absolute deviation 
  * Continuous data, often more robust
3. Sensitivity (recall)
  * If you want few missed positives
4. Specificity
  * If you want few negatives called positives
5. Accuracy
  * Weights false positives/negatives equally
6. Concordance
  * One example is [kappa](http://en.wikipedia.org/wiki/Cohen%27s_kappa)
7. Predictive value of a positive (precision)
  * When you are screeing and prevelance is low

# Lesson (7) Receiver Operating Characteristic

## Why a curve?
  
  * In binary classification you are predicting one of two categories
    * Alive/dead
    * Click on ad/don't click
  * But your predictions are often quantitative
    * Probability of being alive
    * Prediction on a scale from 1 to 10
  * The _cutoff_  you choose gives different results
  
  ---
  
  ## ROC curves
  
  [http://en.wikipedia.org/wiki/Receiver_operating_characteristic](http://en.wikipedia.org/wiki/Receiver_operating_characteristic)
  
  ---
  
  ## An example
  
  [http://en.wikipedia.org/wiki/Receiver_operating_characteristic](http://en.wikipedia.org/wiki/Receiver_operating_characteristic)
  
  ---
  
  ## Area under the curve
  
  * AUC = 0.5: random guessing
  * AUC = 1: perfect classifer
  * In general AUC of above 0.8 considered "good"
  
  [http://en.wikipedia.org/wiki/Receiver_operating_characteristic](http://en.wikipedia.org/wiki/Receiver_operating_characteristic)
  
  ---
  
  ## What is good?
  
  [http://en.wikipedia.org/wiki/Receiver_operating_characteristic](http://en.wikipedia.org/wiki/Receiver_operating_characteristic)
  
  # Lesson (8) Cross Validation

## Study design

[http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf](http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf)

---

## Key idea

1. Accuracy on the training set (resubstitution accuracy) is optimistic
2. A better estimate comes from an independent set (test set accuracy)
3. But we can't use the test set when building the model or it becomes part of the training set
4. So we estimate the test set accuracy with the training set. 

---

## Cross-validation

_Approach_:
1. Use the training set
2. Split it into training/test sets 
3. Build a model on the training set
4. Evaluate on the test set
5. Repeat and average the estimated errors
_Used for_:
1. Picking variables to include in a model
2. Picking the type of prediction function to use
3. Picking the parameters in the prediction function
4. Comparing different predictors

---

## Random subsampling

---

## K-fold

---

## Leave one out

---

## Considerations

* For time series data data must be used in "chunks"
* For k-fold cross validation
  * Larger k = less bias, more variance
  * Smaller k = more bias, less variance
* Random sampling must be done _without replacement_
* Random sampling with replacement is the _bootstrap_
  * Underestimates of the error
  * Can be corrected, but it is complicated ([0.632 Bootstrap](http://www.jstor.org/discover/10.2307/2965703?uid=2&uid=4&sid=21103054448997))
* If you cross-validate to pick predictors estimate you must estimate errors on independent data. 

# Lesson (9) What Data 

## A succcessful predictor

[fivethirtyeight.com](fivethirtyeight.com)

---

## Polling data

[http://www.gallup.com/](http://www.gallup.com/)

---

## Weighting the data

[http://www.fivethirtyeight.com/2010/06/pollster-ratings-v40-methodology.html](http://www.fivethirtyeight.com/2010/06/pollster-ratings-v40-methodology.html)

---

## Key idea

<center>To predict X use data related to X</center>

---

## Key idea

<center>To predict player performance use data about player performance</center>

---

## Key idea

<center>To predict movie preferences use data about movie preferences</center>

---

## Key idea

<center>To predict hospitalizations use data about hospitalizations</center>

---

## Not a hard rule

<center>To predict flu outbreaks use Google searches</center>

[http://www.google.org/flutrends/](http://www.google.org/flutrends/)

---

## Looser connection = harder prediction

---

## Data properties matter

---

## Unrelated data is the most common mistake

[http://www.nejm.org/doi/full/10.1056/NEJMon1211064](http://www.nejm.org/doi/full/10.1056/NEJMon1211064)

# Lesson (10) Caret Package 

## The caret R package

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

http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf

--- 

## SPAM Example: Data splitting

```{r loadPackage}
library(caret); library(kernlab); data(spam)
inTrain <- createDataPartition(y=spam$type,
                              p=0.75, list=FALSE)
training <- spam[inTrain,]
testing <- spam[-inTrain,]
dim(training)
```

--- 

## SPAM Example: Fit a model

```{r training, dependson="loadPackage",cache=TRUE}
set.seed(32343)
modelFit <- train(type ~.,data=training, method="glm")
modelFit
```

--- 

## SPAM Example: Final model

```{r finalModel, dependson="training",cache=TRUE}
modelFit <- train(type ~.,data=training, method="glm")
modelFit$finalModel
```

--- 

## SPAM Example: Prediction

```{r predictions, dependson="training",cache=TRUE}
predictions <- predict(modelFit,newdata=testing)
predictions
```
--- 

## SPAM Example: Confusion Matrix

```{r confusion, dependson="predictions",cache=TRUE}
confusionMatrix(predictions,testing$type)
```

---

## Further information

* Caret tutorials:
  * [http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf](http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf)
  * [http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf](http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf)
* A paper introducing the caret package
  * [http://www.jstatsoft.org/v28/i05/paper](http://www.jstatsoft.org/v28/i05/paper)
  * 
  
# Lesson (11) Data Slicing 

## SPAM Example: Data splitting

```{r loadPackage}
library(caret); library(kernlab); data(spam)
inTrain <- createDataPartition(y=spam$type,
                              p=0.75, list=FALSE)
training <- spam[inTrain,]
testing <- spam[-inTrain,]
dim(training)
```

---

## SPAM Example: K-fold

```{r kfold,dependson="loadPackage"}
set.seed(32323)
folds <- createFolds(y=spam$type,k=10,
                             list=TRUE,returnTrain=TRUE)
sapply(folds,length)
folds[[1]][1:10]
```

---

## SPAM Example: Return test

```{r kfoldtest,dependson="loadPackage"}
set.seed(32323)
folds <- createFolds(y=spam$type,k=10,
                             list=TRUE,returnTrain=FALSE)
sapply(folds,length)
folds[[1]][1:10]
```

---

## SPAM Example: Resampling

```{r resample,dependson="loadPackage"}
set.seed(32323)
folds <- createResample(y=spam$type,times=10,
                             list=TRUE)
sapply(folds,length)
folds[[1]][1:10]
```

---

## SPAM Example: Time Slices

```{r time,dependson="loadPackage"}
set.seed(32323)
tme <- 1:1000
folds <- createTimeSlices(y=tme,initialWindow=20,
                          horizon=10)
names(folds)
folds$train[[1]]
folds$test[[1]]
```

---

## Further information

* Caret tutorials:
  * [http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf](http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf)
  * [http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf](http://cran.r-project.org/web/packages/caret/vignettes/caret.pdf)
* A paper introducing the caret package
  * [http://www.jstatsoft.org/v28/i05/paper](http://www.jstatsoft.org/v28/i05/paper)
  * 
  
# Lesson (12) Training Options 


## SPAM Example

```{r loadPackage,cache=TRUE}
library(caret); library(kernlab); data(spam)
inTrain <- createDataPartition(y=spam$type,
                              p=0.75, list=FALSE)
training <- spam[inTrain,]
testing <- spam[-inTrain,]
modelFit <- train(type ~.,data=training, method="glm")
```

---

## Train options

```{r ,dependson="loadPackage"}
args(train.default)
```
---

## Metric options

__Continous outcomes__:
  * _RMSE_ = Root mean squared error
  * _RSquared_ = $R^2$ from regression models

__Categorical outcomes__:
  * _Accuracy_ = Fraction correct
  * _Kappa_ = A measure of [concordance](http://en.wikipedia.org/wiki/Cohen%27s_kappa)
  
--- 

## trainControl

```{r , dependson="loadPackage",cache=TRUE}
args(trainControl)
```

--- 

## trainControl resampling

* _method_
  * _boot_ = bootstrapping
  * _boot632_ = bootstrapping with adjustment
  * _cv_ = cross validation
  * _repeatedcv_ = repeated cross validation
  * _LOOCV_ = leave one out cross validation
* _number_
  * For boot/cross validation
  * Number of subsamples to take
* _repeats_
  * Number of times to repeate subsampling
  * If big this can _slow things down_

---

## Setting the seed

* It is often useful to set an overall seed
* You can also set a seed for each resample
* Seeding each resample is useful for parallel fits

--- 
## seed example

```{r , dependson="seedExample",cache=TRUE}
set.seed(1235)
modelFit2 <- train(type ~.,data=training, method="glm")
modelFit2
```
--- 

## seed example

```{r , dependson="seedExample",cache=TRUE}
set.seed(1235)
modelFit3 <- train(type ~.,data=training, method="glm")
modelFit3
```
--- 

## Further resources

* [Caret tutorial](http://www.edii.uclm.es/~useR-2013/Tutorials/kuhn/user_caret_2up.pdf)
* [Model training and tuning](http://caret.r-forge.r-project.org/training.html)

# Lesson (13) PlottingPredictors

## Example: predicting wages

Image Credit [http://www.cahs-media.org/the-high-cost-of-low-wages](http://www.cahs-media.org/the-high-cost-of-low-wages)

Data from: [ISLR package](http://cran.r-project.org/web/packages/ISLR) from the book: [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)  

---

## Example: Wage data

```{r loadData,cache=TRUE}
library(ISLR); library(ggplot2); library(caret); library(gridExtra);
data(Wage)
summary(Wage)
```

---

## Get training/test sets

```{r trainingTest,dependson="loadData",cache=TRUE}
inTrain <- createDataPartition(y=Wage$wage,
                              p=0.7, list=FALSE)
training <- Wage[inTrain,]
testing <- Wage[-inTrain,]
dim(training); dim(testing)
```


---

## Feature plot (*caret* package)

```{r ,dependson="trainingTest",fig.height=4,fig.width=4}
featurePlot(x=training[,c("age","education","jobclass")],
            y = training$wage,
            plot="pairs")
```


---

## Qplot (*ggplot2* package)


```{r ,dependson="trainingTest",fig.height=4,fig.width=6}
qplot(age,wage,data=training)
```


---

## Qplot with color (*ggplot2* package)


```{r ,dependson="trainingTest",fig.height=4,fig.width=6}
qplot(age,wage,colour=jobclass,data=training)
```
---

## Add regression smoothers (*ggplot2* package)


```{r ,dependson="trainingTest",fig.height=4,fig.width=6}
qq <- qplot(age,wage,colour=education,data=training)
qq +  geom_smooth(method='lm',formula=y~x)
```

---

## cut2, making factors (*Hmisc* package)

```{r cut2,dependson="trainingTest",fig.height=4,fig.width=6,cache=TRUE}
cutWage <- cut2(training$wage,g=3)
table(cutWage)
```
---

## Boxplots with cut2


```{r ,dependson="cut2plot",fig.height=4,fig.width=6,cache=TRUE}
p1 <- qplot(cutWage,age, data=training,fill=cutWage,
      geom=c("boxplot"))
p1
```

---

## Boxplots with points overlayed

```{r ,dependson="cut2plot",fig.height=4,fig.width=9}
p2 <- qplot(cutWage,age, data=training,fill=cutWage,
      geom=c("boxplot","jitter"))
grid.arrange(p1,p2,ncol=2)
```
---

## Tables

```{r ,dependson="cut2",fig.height=4,fig.width=9}
t1 <- table(cutWage,training$jobclass)
t1
prop.table(t1,1)
```
---

## Density plots

```{r ,dependson="trainingTest",fig.height=4,fig.width=6}
qplot(wage,colour=education,data=training,geom="density")
```
---

## Notes and further reading

* Make your plots only in the training set 
  * Don't use the test set for exploration!
* Things you should be looking for
  * Imbalance in outcomes/predictors
  * Outliers 
  * Groups of points not explained by a predictor
  * Skewed variables 
* [ggplot2 tutorial](http://rstudio-pubs-static.s3.amazonaws.com/2176_75884214fc524dc0bc2a140573da38bb.html)
* [caret visualizations](http://caret.r-forge.r-project.org/visualizations.html)

# Lesson (14) Basic Preprocessing 

## Why preprocess?

```{r loadPackage,cache=TRUE,fig.height=3.5,fig.width=3.5}
library(caret); library(kernlab); data(spam)
inTrain <- createDataPartition(y=spam$type,
                              p=0.75, list=FALSE)
training <- spam[inTrain,]
testing <- spam[-inTrain,]
hist(training$capitalAve,main="",xlab="ave. capital run length")
```

---

## Why preprocess?

```{r ,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
mean(training$capitalAve)
sd(training$capitalAve)
```

---

## Standardizing

```{r ,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
trainCapAve <- training$capitalAve
trainCapAveS <- (trainCapAve  - mean(trainCapAve))/sd(trainCapAve) 
mean(trainCapAveS)
sd(trainCapAveS)
```

---

## Standardizing - test set

```{r ,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
testCapAve <- testing$capitalAve
testCapAveS <- (testCapAve  - mean(trainCapAve))/sd(trainCapAve) 
mean(testCapAveS)
sd(testCapAveS)
```


---

## Standardizing - _preProcess_ function

```{r preprocess,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
preObj <- preProcess(training[,-58],method=c("center","scale"))
trainCapAveS <- predict(preObj,training[,-58])$capitalAve
mean(trainCapAveS)
sd(trainCapAveS)
```


---

## Standardizing - _preProcess_ function

```{r ,dependson="preprocess",cache=TRUE,fig.height=3.5,fig.width=3.5}
testCapAveS <- predict(preObj,testing[,-58])$capitalAve
mean(testCapAveS)
sd(testCapAveS)
```

---

## Standardizing - _preProcess_ argument

```{r training, dependson="loadPackage",cache=TRUE}
set.seed(32343)
modelFit <- train(type ~.,data=training,
                  preProcess=c("center","scale"),method="glm")
modelFit
```


---

## Standardizing - Box-Cox transforms

```{r ,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=7}
preObj <- preProcess(training[,-58],method=c("BoxCox"))
trainCapAveS <- predict(preObj,training[,-58])$capitalAve
par(mfrow=c(1,2)); hist(trainCapAveS); qqnorm(trainCapAveS)
```


---

## Standardizing - Imputing data

```{r knn,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=7}
set.seed(13343)

# Make some values NA
training$capAve <- training$capitalAve
selectNA <- rbinom(dim(training)[1],size=1,prob=0.05)==1
training$capAve[selectNA] <- NA

# Impute and standardize
preObj <- preProcess(training[,-58],method="knnImpute")
capAve <- predict(preObj,training[,-58])$capAve

# Standardize true values
capAveTruth <- training$capitalAve
capAveTruth <- (capAveTruth-mean(capAveTruth))/sd(capAveTruth)
```


---

## Standardizing - Imputing data

```{r ,dependson="knn",cache=TRUE,fig.height=3.5,fig.width=7}
quantile(capAve - capAveTruth)
quantile((capAve - capAveTruth)[selectNA])
quantile((capAve - capAveTruth)[!selectNA])
```

---

## Notes and further reading

* Training and test must be processed in the same way
* Test transformations will likely be imperfect
  * Especially if the test/training sets collected at different times
* Careful when transforming factor variables!
* [preprocessing with caret](http://caret.r-forge.r-project.org/preprocess.html)

#  Lesson (15) Covariate Creation 


## Two levels of covariate creation

**Level 1: From raw data to covariate**

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/covCreation1.png height=200>

**Level 2: Transforming tidy covariates** 

```{r spamData,fig.height=4,fig.width=4}
library(kernlab);data(spam)
spam$capitalAveSq <- spam$capitalAve^2
```


---

## Level 1, Raw data -> covariates

* Depends heavily on application
* The balancing act is summarization vs. information loss
* Examples:
  * Text files: frequency of words, frequency of phrases ([Google ngrams](https://books.google.com/ngrams)), frequency of capital letters.
  * Images: Edges, corners, blobs, ridges ([computer vision feature detection](http://en.wikipedia.org/wiki/Feature_detection_(computer_vision)))
  * Webpages: Number and type of images, position of elements, colors, videos ([A/B Testing](http://en.wikipedia.org/wiki/A/B_testing))
  * People: Height, weight, hair color, sex, country of origin. 
* The more knowledge of the system you have the better the job you will do. 
* When in doubt, err on the side of more features
* Can be automated, but use caution!


---

## Level 2, Tidy covariates -> new covariates

* More necessary for some methods (regression, svms) than for others (classification trees).
* Should be done _only on the training set_
* The best approach is through exploratory analysis (plotting/tables)
* New covariates should be added to data frames



---

## Load example data


```{r loadData,cache=TRUE}
library(ISLR); library(caret); data(Wage);
inTrain <- createDataPartition(y=Wage$wage,
                              p=0.7, list=FALSE)
training <- Wage[inTrain,]; testing <- Wage[-inTrain,]
```


---

## Common covariates to add, dummy variables

__Basic idea - convert factor variables to [indicator variables](http://bit.ly/19ZhWB6)__

```{r dummyVar,dependson="loadData"}
table(training$jobclass)
dummies <- dummyVars(wage ~ jobclass,data=training)
head(predict(dummies,newdata=training))
```



---

## Removing zero covariates

```{r ,dependson="dummyVar"}
nsv <- nearZeroVar(training,saveMetrics=TRUE)
nsv
```



---

## Spline basis

```{r splines,dependson="dummyVar",cache=TRUE}
library(splines)
bsBasis <- bs(training$age,df=3) 
bsBasis
```

_See also_: ns(),poly()

---

## Fitting curves with splines

```{r ,dependson="splines",fig.height=4,fig.width=4}
lm1 <- lm(wage ~ bsBasis,data=training)
plot(training$age,training$wage,pch=19,cex=0.5)
points(training$age,predict(lm1,newdata=training),col="red",pch=19,cex=0.5)
```


---

## Splines on the test set

```{r ,dependson="splines",fig.height=4,fig.width=4}
predict(bsBasis,age=testing$age)
```


---

## Notes and further reading

* Level 1 feature creation (raw data to covariates)
  * Science is key. Google "feature extraction for [data type]"
  * Err on overcreation of features
  * In some applications (images, voices) automated feature creation is possible/necessary
    * http://www.cs.nyu.edu/~yann/talks/lecun-ranzato-icml2013.pdf
* Level 2 feature creation (covariates to new covariates)
  * The function _preProcess_ in _caret_ will handle some preprocessing.
  * Create new covariates if you think they will improve fit
  * Use exploratory analysis on the training set for creating them
  * Be careful about overfitting!
* [preprocessing with caret](http://caret.r-forge.r-project.org/preprocess.html)
* If you want to fit spline models, use the _gam_ method in the _caret_ package which allows smoothing of multiple variables.
* More on feature creation/data tidying in the Obtaining Data course from the Data Science course track. 

# Lesson (16) Preprocessing PCA


## Correlated predictors

```{r loadPackage,cache=TRUE,fig.height=3.5,fig.width=3.5}
library(caret); library(kernlab); data(spam)
inTrain <- createDataPartition(y=spam$type,
                              p=0.75, list=FALSE)
training <- spam[inTrain,]
testing <- spam[-inTrain,]

M <- abs(cor(training[,-58]))
diag(M) <- 0
which(M > 0.8,arr.ind=T)
```

---

## Correlated predictors

```{r,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
names(spam)[c(34,32)]
plot(spam[,34],spam[,32])
```


---

## Basic PCA idea

* We might not need every predictor
* A weighted combination of predictors might be better
* We should pick this combination to capture the "most information" possible
* Benefits
  * Reduced number of predictors
  * Reduced noise (due to averaging)


---

## We could rotate the plot

$$ X = 0.71 \times {\rm num 415} + 0.71 \times {\rm num857}$$

$$ Y = 0.71 \times {\rm num 415} - 0.71 \times {\rm num857}$$

```{r,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
X <- 0.71*training$num415 + 0.71*training$num857
Y <- 0.71*training$num415 - 0.71*training$num857
plot(X,Y)
```

---

## Related problems

You have multivariate variables $X_1,\ldots,X_n$ so $X_1 = (X_{11},\ldots,X_{1m})$

* Find a new set of multivariate variables that are uncorrelated and explain as much variance as possible.
* If you put all the variables together in one matrix, find the best matrix created with fewer variables (lower rank) that explains the original data.


The first goal is <font color="#330066">statistical</font> and the second goal is <font color="#993300">data compression</font>.

---

## Related solutions - PCA/SVD

__SVD__

If $X$ is a matrix with each variable in a column and each observation in a row then the SVD is a "matrix decomposition"

$$ X = UDV^T$$

where the columns of $U$ are orthogonal (left singular vectors), the columns of $V$ are orthogonal (right singluar vectors) and $D$ is a diagonal matrix (singular values). 

__PCA__

The principal components are equal to the right singular values if you first scale (subtract the mean, divide by the standard deviation) the variables.

---

## Principal components in R - prcomp

```{r prcomp,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
smallSpam <- spam[,c(34,32)]
prComp <- prcomp(smallSpam)
plot(prComp$x[,1],prComp$x[,2])
```

---

## Principal components in R - prcomp

```{r ,dependson="prcomp",cache=TRUE,fig.height=3.5,fig.width=3.5}
prComp$rotation
```


---

## PCA on SPAM data

```{r spamPC,dependson="loadPackage",cache=TRUE,fig.height=3.5,fig.width=3.5}
typeColor <- ((spam$type=="spam")*1 + 1)
prComp <- prcomp(log10(spam[,-58]+1))
plot(prComp$x[,1],prComp$x[,2],col=typeColor,xlab="PC1",ylab="PC2")
```


---

## PCA with caret

```{r ,dependson="spamPC",cache=TRUE,fig.height=3.5,fig.width=3.5}
preProc <- preProcess(log10(spam[,-58]+1),method="pca",pcaComp=2)
spamPC <- predict(preProc,log10(spam[,-58]+1))
plot(spamPC[,1],spamPC[,2],col=typeColor)
```


---

## Preprocessing with PCA

```{r pcaCaret,dependson="spamPC",cache=TRUE,fig.height=3.5,fig.width=3.5}
preProc <- preProcess(log10(training[,-58]+1),method="pca",pcaComp=2)
trainPC <- predict(preProc,log10(training[,-58]+1))
modelFit <- train(training$type ~ .,method="glm",data=trainPC)
```

---

## Preprocessing with PCA

```{r ,dependson="pcaCaret",cache=TRUE,fig.height=3.5,fig.width=3.5}
testPC <- predict(preProc,log10(testing[,-58]+1))
confusionMatrix(testing$type,predict(modelFit,testPC))
```

---

## Alternative (sets # of PCs)

```{r ,dependson="pcaCaret",cache=TRUE,fig.height=3.5,fig.width=3.5}
modelFit <- train(training$type ~ .,method="glm",preProcess="pca",data=training)
confusionMatrix(testing$type,predict(modelFit,testing))
```

---

## Final thoughts on PCs

* Most useful for linear-type models
* Can make it harder to interpret predictors
* Watch out for outliers! 
  * Transform first (with logs/Box Cox)
  * Plot predictors to identify problems
* For more info see 
  * Exploratory Data Analysis
  * [Elements of Statistical Learning](http://statweb.stanford.edu/~tibs/ElemStatLearn/)
  
# Lesson (17) Predicting with Regression 


## Key ideas

* Fit a simple regression model
* Plug in new covariates and multiply by the coefficients
* Useful when the linear model is (nearly) correct

__Pros__:
* Easy to implement
* Easy to interpret

__Cons__:
* Often poor performance in nonlinear settings


---

## Example: Old faithful eruptions

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/yellowstone.png height=400>

Image Credit/Copyright Wally Pacholka [http://www.astropics.com/](http://www.astropics.com/)

---

## Example: Old faithful eruptions

```{r faith}
library(caret);data(faithful); set.seed(333)
inTrain <- createDataPartition(y=faithful$waiting,
                              p=0.5, list=FALSE)
trainFaith <- faithful[inTrain,]; testFaith <- faithful[-inTrain,]
head(trainFaith)
```

---

## Eruption duration versus waiting time

```{r dependson="faith",fig.height=4,fig.width=4}
plot(trainFaith$waiting,trainFaith$eruptions,pch=19,col="blue",xlab="Waiting",ylab="Duration")
```

---

## Fit a linear model 

$$ ED_i = b_0 + b_1 WT_i + e_i $$

```{r faithlm,dependson="faith",fig.height=4,fig.width=4}
lm1 <- lm(eruptions ~ waiting,data=trainFaith)
summary(lm1)
```


---
## Model fit

```{r dependson="faithlm",fig.height=4,fig.width=4}
plot(trainFaith$waiting,trainFaith$eruptions,pch=19,col="blue",xlab="Waiting",ylab="Duration")
lines(trainFaith$waiting,lm1$fitted,lwd=3)
```

---

## Predict a new value

$$\hat{ED} = \hat{b}_0 + \hat{b}_1 WT$$

```{r ,dependson="faithlm",fig.height=4,fig.width=4}
coef(lm1)[1] + coef(lm1)[2]*80
newdata <- data.frame(waiting=80)
predict(lm1,newdata)
```

---

## Plot predictions - training and test

```{r ,dependson="faithlm",fig.height=4,fig.width=8}
par(mfrow=c(1,2))
plot(trainFaith$waiting,trainFaith$eruptions,pch=19,col="blue",xlab="Waiting",ylab="Duration")
lines(trainFaith$waiting,predict(lm1),lwd=3)
plot(testFaith$waiting,testFaith$eruptions,pch=19,col="blue",xlab="Waiting",ylab="Duration")
lines(testFaith$waiting,predict(lm1,newdata=testFaith),lwd=3)
```

---

## Get training set/test set errors

```{r ,dependson="faithlm",fig.height=4,fig.width=4}
# Calculate RMSE on training
sqrt(sum((lm1$fitted-trainFaith$eruptions)^2))

# Calculate RMSE on test
sqrt(sum((predict(lm1,newdata=testFaith)-testFaith$eruptions)^2))
```

---

## Prediction intervals

```{r ,dependson="faithlm",fig.height=4,fig.width=4}
pred1 <- predict(lm1,newdata=testFaith,interval="prediction")
ord <- order(testFaith$waiting)
plot(testFaith$waiting,testFaith$eruptions,pch=19,col="blue")
matlines(testFaith$waiting[ord],pred1[ord,],type="l",,col=c(1,2,2),lty = c(1,1,1), lwd=3)
```


---

## Same process with caret

```{r caretfaith,dependson="faith",fig.height=4,fig.width=4}
modFit <- train(eruptions ~ waiting,data=trainFaith,method="lm")
summary(modFit$finalModel)
```


---

## Notes and further reading

* Regression models with multiple covariates can be included
* Often useful in combination with other models 
* [Elements of statistical learning](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)
* [Modern applied statistics with S](http://www.amazon.com/Modern-Applied-Statistics-W-N-Venables/dp/0387954570)
* [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)

# Lesson (18) Predicting with Regression 


## Example: predicting wages

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/wages.jpg height=350>

Image Credit [http://www.cahs-media.org/the-high-cost-of-low-wages](http://www.cahs-media.org/the-high-cost-of-low-wages)

Data from: [ISLR package](http://cran.r-project.org/web/packages/ISLR) from the book: [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)



---

## Example: Wage data

```{r loadData,cache=TRUE}
library(ISLR); library(ggplot2); library(caret);
data(Wage); Wage <- subset(Wage,select=-c(logwage))
summary(Wage)
```



---

## Get training/test sets

```{r trainingTest,dependson="loadData",cache=TRUE}
inTrain <- createDataPartition(y=Wage$wage,
                              p=0.7, list=FALSE)
training <- Wage[inTrain,]; testing <- Wage[-inTrain,]
dim(training); dim(testing)
```



---

## Feature plot

```{r ,dependson="trainingTest",fig.height=4,fig.width=4}
featurePlot(x=training[,c("age","education","jobclass")],
            y = training$wage,
            plot="pairs")
```


---

## Plot age versus wage


```{r ,dependson="trainingTest",fig.height=4,fig.width=6}
qplot(age,wage,data=training)
```


---

## Plot age versus wage colour by jobclass


```{r ,dependson="trainingTest",fig.height=4,fig.width=6}
qplot(age,wage,colour=jobclass,data=training)
```


---

## Plot age versus wage colour by education


```{r ,dependson="trainingTest",fig.height=4,fig.width=6}
qplot(age,wage,colour=education,data=training)
```

---

## Fit a linear model 

$$ ED_i = b_0 + b_1 age + b_2 I(Jobclass_i="Information") + \sum_{k=1}^4 \gamma_k I(education_i= level k) $$

```{r modelFit,dependson="trainingTest", cache=TRUE,fig.height=4,fig.width=4}
modFit<- train(wage ~ age + jobclass + education,
               method = "lm",data=training)
finMod <- modFit$finalModel
print(modFit)
```

Education levels: 1 = HS Grad, 2 = Some College, 3 = College Grad, 4 = Advanced Degree

---

## Diagnostics

```{r,dependson="modelFit",fig.height=5,fig.width=5}
plot(finMod,1,pch=19,cex=0.5,col="#00000010")
```


---

## Color by variables not used in the model 

```{r,dependson="modelFit",fig.height=4,fig.width=6}
qplot(finMod$fitted,finMod$residuals,colour=race,data=training)
```

---

## Plot by index

```{r,dependson="modelFit",fig.height=5,fig.width=5}
plot(finMod$residuals,pch=19)
```


---

## Predicted versus truth in test set

```{r predictions, dependson="modelFit",fig.height=4,fig.width=6}
pred <- predict(modFit, testing)
qplot(wage,pred,colour=year,data=testing)
```

---

## If you want to use all covariates

```{r allCov,dependson="trainingTest",fig.height=4,fig.width=4,warning=FALSE}
modFitAll<- train(wage ~ .,data=training,method="lm")
pred <- predict(modFitAll, testing)
qplot(wage,pred,data=testing)
```


---

## Notes and further reading

* Often useful in combination with other models 
* [Elements of statistical learning](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)
* [Modern applied statistics with S](http://www.amazon.com/Modern-Applied-Statistics-W-N-Venables/dp/0387954570)
* [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)
* 

# Lesson (19) Predicting with Trees 


## Key ideas

* Iteratively split variables into groups
* Evaluate "homogeneity" within each group
* Split again if necessary

__Pros__:

* Easy to interpret
* Better performance in nonlinear settings

__Cons__:

* Without pruning/cross-validation can lead to overfitting
* Harder to estimate uncertainty
* Results may be variable


---

## Example Tree

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/obamaTree.png height=450>

[http://graphics8.nytimes.com/images/2008/04/16/us/0416-nat-subOBAMA.jpg](http://graphics8.nytimes.com/images/2008/04/16/us/0416-nat-subOBAMA.jpg)

---

## Basic algorithm

1. Start with all variables in one group
2. Find the variable/split that best separates the outcomes
3. Divide the data into two groups ("leaves") on that split ("node")
4. Within each split, find the best variable/split that separates the outcomes
5. Continue until the groups are too small or sufficiently "pure"


---

## Measures of impurity

$$\hat{p}_{mk} = \frac{1}{N_m}\sum_{x_i\; in \; Leaf \; m}\mathbb{1}(y_i = k)$$

__Misclassification Error__: 
$$ 1 - \hat{p}_{m k(m)}; k(m) = {\rm most; common; k}$$ 
* 0 = perfect purity
* 0.5 = no purity

__Gini index__:
$$ \sum_{k \neq k'} \hat{p}_{mk} \times \hat{p}_{mk'} = \sum_{k=1}^K \hat{p}_{mk}(1-\hat{p}_{mk}) = 1 - \sum_{k=1}^K p_{mk}^2$$

* 0 = perfect purity
* 0.5 = no purity

http://en.wikipedia.org/wiki/Decision_tree_learning

---

## Measures of impurity

__Deviance/information gain__:

$$ -\sum_{k=1}^K \hat{p}_{mk} \log_2\hat{p}_{mk} $$
* 0 = perfect purity
* 1 = no purity

http://en.wikipedia.org/wiki/Decision_tree_learning


--- &twocol w1:50% w2:50%
## Measures of impurity

*** =left

```{r leftplot,fig.height=3,fig.width=4,echo=FALSE,fig.align="center"}
par(mar=c(0,0,0,0)); set.seed(1234); x = rep(1:4,each=4); y = rep(1:4,4)
plot(x,y,xaxt="n",yaxt="n",cex=3,col=c(rep("blue",15),rep("red",1)),pch=19)
```

* __Misclassification:__ $1/16 = 0.06$
* __Gini:__ $1 - [(1/16)^2 + (15/16)^2] = 0.12$
* __Information:__$-[1/16 \times log2(1/16) + 15/16 \times log2(15/16)] = 0.34$

*** =right

```{r,dependson="leftplot",fig.height=3,fig.width=4,echo=FALSE,fig.align="center"}
par(mar=c(0,0,0,0)); 
plot(x,y,xaxt="n",yaxt="n",cex=3,col=c(rep("blue",8),rep("red",8)),pch=19)
```

* __Misclassification:__ $8/16 = 0.5$
* __Gini:__ $1 - [(8/16)^2 + (8/16)^2] = 0.5$
* __Information:__$-[1/16 \times log2(1/16) + 15/16 \times log2(15/16)] = 1$




---

## Example: Iris Data

```{r iris, cache=TRUE}
data(iris); library(ggplot2)
names(iris)
table(iris$Species)
```


---

## Create training and test sets

```{r trainingTest, dependson="iris",cache=TRUE}
inTrain <- createDataPartition(y=iris$Species,
                              p=0.7, list=FALSE)
training <- iris[inTrain,]
testing <- iris[-inTrain,]
dim(training); dim(testing)
```


---

## Iris petal widths/sepal width

```{r, dependson="trainingTest",fig.height=4,fig.width=6}
qplot(Petal.Width,Sepal.Width,colour=Species,data=training)
```


---

## Iris petal widths/sepal width

```{r createTree, dependson="trainingTest", cache=TRUE}
library(caret)
modFit <- train(Species ~ .,method="rpart",data=training)
print(modFit$finalModel)
```

---

## Plot tree

```{r, dependson="createTree", fig.height=4.5, fig.width=4.5}
plot(modFit$finalModel, uniform=TRUE, 
      main="Classification Tree")
text(modFit$finalModel, use.n=TRUE, all=TRUE, cex=.8)
```


---

## Prettier plots

```{r, dependson="createTree", fig.height=4.5, fig.width=4.5}
library(rattle)
fancyRpartPlot(modFit$finalModel)
```

---

## Predicting new values

```{r newdata, dependson="createTree", fig.height=4.5, fig.width=4.5, cache=TRUE}
predict(modFit,newdata=testing)
```

---

## Notes and further resources

* Classification trees are non-linear models
  * They use interactions between variables
  * Data transformations may be less important (monotone transformations)
  * Trees can also be used for regression problems (continuous outcome)
* Note that there are multiple tree building options
in R both in the caret package - [party](http://cran.r-project.org/web/packages/party/index.html), [rpart](http://cran.r-project.org/web/packages/rpart/index.html) and out of the caret package - [tree](http://cran.r-project.org/web/packages/tree/index.html)
* [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)
* [Elements of Statistical Learning](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)
* [Classification and regression trees](http://www.amazon.com/Classification-Regression-Trees-Leo-Breiman/dp/0412048418)


# Lesson (20) Bagging 


## Bootstrap aggregating (bagging)

__Basic idea__: 

1. Resample cases and recalculate predictions
2. Average or majority vote

__Notes__:

* Similar bias 
* Reduced variance
* More useful for non-linear functions


---

## Ozone data

```{r ozoneData, cache=TRUE}
library(ElemStatLearn); data(ozone,package="ElemStatLearn")
ozone <- ozone[order(ozone$ozone),]
head(ozone)
```
[http://en.wikipedia.org/wiki/Bootstrap_aggregating](http://en.wikipedia.org/wiki/Bootstrap_aggregating)


---

## Bagged loess

```{r baggedOzone, dependson="ozoneData",cache=TRUE}
ll <- matrix(NA,nrow=10,ncol=155)
for(i in 1:10){
  ss <- sample(1:dim(ozone)[1],replace=T)
  ozone0 <- ozone[ss,]; ozone0 <- ozone0[order(ozone0$ozone),]
  loess0 <- loess(temperature ~ ozone,data=ozone0,span=0.2)
  ll[i,] <- predict(loess0,newdata=data.frame(ozone=1:155))
}
```

---

## Bagged loess

```{r, dependson="baggedOzone",fig.height=4.5,fig.width=4.5}
plot(ozone$ozone,ozone$temperature,pch=19,cex=0.5)
for(i in 1:10){lines(1:155,ll[i,],col="grey",lwd=2)}
lines(1:155,apply(ll,2,mean),col="red",lwd=2)
```


---

## Bagging in caret

* Some models perform bagging for you, in `train` function consider `method` options 
  * `bagEarth` 
  * `treebag`
  * `bagFDA`
* Alternatively you can bag any model you choose using the `bag` function

---

## More bagging in caret

```{r bag1}
predictors = data.frame(ozone=ozone$ozone)
temperature = ozone$temperature
treebag <- bag(predictors, temperature, B = 10,
                bagControl = bagControl(fit = ctreeBag$fit,
                                        predict = ctreeBag$pred,
                                        aggregate = ctreeBag$aggregate))
```

http://www.inside-r.org/packages/cran/caret/docs/nbBag


---

## Example of custom bagging (continued)

```{r,dependson="bag1",fig.height=4,fig.width=4}
plot(ozone$ozone,temperature,col='lightgrey',pch=19)
points(ozone$ozone,predict(treebag$fits[[1]]$fit,predictors),pch=19,col="red")
points(ozone$ozone,predict(treebag,predictors),pch=19,col="blue")
```


---

## Parts of bagging

```{r}
ctreeBag$fit
```

---

## Parts of bagging

```{r}
ctreeBag$pred
```


---

## Parts of bagging

```{r}
ctreeBag$aggregate
```


---

## Notes and further resources

__Notes__:

* Bagging is most useful for nonlinear models
* Often used with trees - an extension is random forests
* Several models use bagging in caret's _train_ function

__Further resources__:

* [Bagging](http://en.wikipedia.org/wiki/Bootstrap_aggregating)
* [Bagging and boosting](http://stat.ethz.ch/education/semesters/FS_2008/CompStat/sk-ch8.pdf)
* [Elements of Statistical Learning](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)

# Lesson (21) Random Forest 


## Random forests

1. Bootstrap samples
2. At each split, bootstrap variables
3. Grow multiple trees and vote

__Pros__:

1. Accuracy

__Cons__:

1. Speed
2. Interpretability
3. Overfitting


---

## Random forests

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/forests.png height=400>

[http://www.robots.ox.ac.uk/~az/lectures/ml/lect5.pdf](http://www.robots.ox.ac.uk/~az/lectures/ml/lect5.pdf)


---

## Iris data

```{r iris, cache=TRUE}
data(iris); library(ggplot2)
inTrain <- createDataPartition(y=iris$Species,
                              p=0.7, list=FALSE)
training <- iris[inTrain,]
testing <- iris[-inTrain,]
```


---

## Random forests

```{r forestIris, dependson="irisData",fig.height=4,fig.width=4,cache=TRUE}
library(caret)
modFit <- train(Species~ .,data=training,method="rf",prox=TRUE)
modFit
```

---

## Getting a single tree

```{r , dependson="forestIris",fig.height=4,fig.width=4}
getTree(modFit$finalModel,k=2)
```

---

## Class "centers"

```{r centers, dependson="forestIris",fig.height=4,fig.width=4}
irisP <- classCenter(training[,c(3,4)], training$Species, modFit$finalModel$prox)
irisP <- as.data.frame(irisP); irisP$Species <- rownames(irisP)
p <- qplot(Petal.Width, Petal.Length, col=Species,data=training)
p + geom_point(aes(x=Petal.Width,y=Petal.Length,col=Species),size=5,shape=4,data=irisP)
```

---

## Predicting new values

```{r predForest, dependson="centers",fig.height=4,fig.width=4,cache=TRUE}
pred <- predict(modFit,testing); testing$predRight <- pred==testing$Species
table(pred,testing$Species)
```

---

## Predicting new values

```{r, dependson="predForest",fig.height=4,fig.width=4}
qplot(Petal.Width,Petal.Length,colour=predRight,data=testing,main="newdata Predictions")
```

---

## Notes and further resources

__Notes__:

* Random forests are usually one of the two top
performing algorithms along with boosting in prediction contests.
* Random forests are difficult to interpret but often very accurate. 
* Care should be taken to avoid overfitting (see [rfcv](http://cran.r-project.org/web/packages/randomForest/randomForest.pdf) funtion)


__Further resources__:

* [Random forests](http://www.stat.berkeley.edu/~breiman/RandomForests/cc_home.htm)
* [Random forest Wikipedia](http://en.wikipedia.org/wiki/Random_forest)
* [Elements of Statistical Learning](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)

# Lesson (22) Boosting 


## Basic idea

1. Take lots of (possibly) weak predictors
2. Weight them and add them up
3. Get a stronger predictor


---

## Basic idea behind boosting

1. Start with a set of classifiers $h_1,\ldots,h_k$
  * Examples: All possible trees, all possible regression models, all possible cutoffs.
2. Create a classifier that combines classification functions:
$f(x) = \rm{sgn}\left(\sum_{t=1}^T \alpha_t h_t(x)\right)$.
  * Goal is to minimize error (on training set)
  * Iterative, select one $h$ at each step
  * Calculate weights based on errors
  * Upweight missed classifications and select next $h$
  
[Adaboost on Wikipedia](http://en.wikipedia.org/wiki/AdaBoost)

[http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf](http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf)

---

## Simple example

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/ada1.png height=450>

[http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf](http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf)

---

## Round 1: adaboost

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/adar1.png height=450>

[http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf](http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf)

---

## Round 2 & 3

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/ada2.png height=450>

[http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf](http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf)


---

## Completed classifier

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/ada3.png height=450>

[http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf](http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf)

---

## Boosting in R 

* Boosting can be used with any subset of classifiers
* One large subclass is [gradient boosting](http://en.wikipedia.org/wiki/Gradient_boosting)
* R has multiple boosting libraries. Differences include the choice of basic classification functions and combination rules.
  * [gbm](http://cran.r-project.org/web/packages/gbm/index.html) - boosting with trees.
  * [mboost](http://cran.r-project.org/web/packages/mboost/index.html) - model based boosting
  * [ada](http://cran.r-project.org/web/packages/ada/index.html) - statistical boosting based on [additive logistic regression](http://projecteuclid.org/DPubS?service=UI&version=1.0&verb=Display&handle=euclid.aos/1016218223)
  * [gamBoost](http://cran.r-project.org/web/packages/GAMBoost/index.html) for boosting generalized additive models
* Most of these are available in the caret package 



---

## Wage example

```{r wage, cache=TRUE}
library(ISLR); data(Wage); library(ggplot2); library(caret);
Wage <- subset(Wage,select=-c(logwage))
inTrain <- createDataPartition(y=Wage$wage,
                              p=0.7, list=FALSE)
training <- Wage[inTrain,]; testing <- Wage[-inTrain,]
```


---

## Fit the model

```{r, dependson="wage", cache=TRUE}
modFit <- train(wage ~ ., method="gbm",data=training,verbose=FALSE)
print(modFit)
```

---

## Plot the results

```{r, dependson="wage", fig.height=4,fig.width=4}
qplot(predict(modFit,testing),wage,data=testing)
```



---

## Notes and further reading

* A couple of nice tutorials for boosting
  * Freund and Shapire - [http://www.cc.gatech.edu/~thad/6601-gradAI-fall2013/boosting.pdf](http://www.cc.gatech.edu/~thad/6601-gradAI-fall2013/boosting.pdf)
  * Ron Meir- [http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf](http://webee.technion.ac.il/people/rmeir/BoostingTutorial.pdf)
* Boosting, random forests, and model ensembling are the most common tools that win Kaggle and other prediction contests. 
  * [http://www.netflixprize.com/assets/GrandPrize2009_BPC_BigChaos.pdf](http://www.netflixprize.com/assets/GrandPrize2009_BPC_BigChaos.pdf)
  * [https://kaggle2.blob.core.windows.net/wiki-files/327/09ccf652-8c1c-4a3d-b979-ce2369c985e4/Willem%20Mestrom%20-%20Milestone%201%20Description%20V2%202.pdf](https://kaggle2.blob.core.windows.net/wiki-files/327/09ccf652-8c1c-4a3d-b979-ce2369c985e4/Willem%20Mestrom%20-%20Milestone%201%20Description%20V2%202.pdf)
  

# Lesson (23) Model Based Prediction 


## Basic idea

1. Assume the data follow a probabilistic model
2. Use Bayes' theorem to identify optimal classifiers

__Pros:__

* Can take advantage of structure of the data
* May be computationally convenient
* Are reasonably accurate on real problems

__Cons:__

* Make additional assumptions about the data
* When the model is incorrect you may get reduced accuracy

---

## Model based approach


1. Our goal is to build parametric model for conditional distribution $P(Y = k | X = x)$

2. A typical approach is to apply [Bayes theorem](http://en.wikipedia.org/wiki/Bayes'_theorem):
$$ Pr(Y = k | X=x) = \frac{Pr(X=x|Y=k)Pr(Y=k)}{\sum_{\ell=1}^K Pr(X=x |Y = \ell) Pr(Y=\ell)}$$
$$Pr(Y = k | X=x) = \frac{f_k(x) \pi_k}{\sum_{\ell = 1}^K f_{\ell}(x) \pi_{\ell}}$$

3. Typically prior probabilities $\pi_k$ are set in advance.

4. A common choice for $f_k(x) = \frac{1}{\sigma_k \sqrt{2 \pi}}e^{-\frac{(x-\mu_k)^2}{\sigma_k^2}}$, a Gaussian distribution

5. Estimate the parameters ($\mu_k$,$\sigma_k^2$) from the data.

6. Classify to the class with the highest value of $P(Y = k | X = x)$

---

## Classifying using the model

A range of models use this approach

* Linear discriminant analysis assumes $f_k(x)$ is multivariate Gaussian with same covariances
* Quadratic discrimant analysis assumes $f_k(x)$ is multivariate Gaussian with different covariances
* [Model based prediction](http://www.stat.washington.edu/mclust/) assumes more complicated versions for the covariance matrix 
* Naive Bayes assumes independence between features for model building

http://statweb.stanford.edu/~tibs/ElemStatLearn/


---

## Why linear discriminant analysis?

$$log \frac{Pr(Y = k | X=x)}{Pr(Y = j | X=x)}$$
$$ = log \frac{f_k(x)}{f_j(x)} + log \frac{\pi_k}{\pi_j}$$
$$ = log \frac{\pi_k}{\pi_j} - \frac{1}{2}(\mu_k^T \Sigma^{-1}\mu_k - \mu_j^T \Sigma^{-1}\mu_j)$$
$$ + x^T \Sigma^{-1} (\mu_k - \mu_j)$$

http://statweb.stanford.edu/~tibs/ElemStatLearn/


---

## Decision boundaries

<img class="center" src="../../assets/img/ldaboundary.png" height=500>

---

## Discriminant function

$$\delta_k(x) = x^T \Sigma^{-1} \mu_k - \frac{1}{2}\mu_k \Sigma^{-1}\mu_k + log(\mu_k)$$


* Decide on class based on $\hat{Y}(x) = argmax_k \delta_k(x)$
* We usually estimate parameters with maximum likelihood


---

## Naive Bayes

Suppose we have many predictors, we would want to model: $P(Y = k | X_1,\ldots,X_m)$

We could use Bayes Theorem to get:

$$P(Y = k | X_1,\ldots,X_m) = \frac{\pi_k P(X_1,\ldots,X_m| Y=k)}{\sum_{\ell = 1}^K P(X_1,\ldots,X_m | Y=k) \pi_{\ell}}$$
$$ \propto \pi_k P(X_1,\ldots,X_m| Y=k)$$

This can be written:

$$P(X_1,\ldots,X_m, Y=k) = \pi_k P(X_1 | Y = k)P(X_2,\ldots,X_m | X_1,Y=k)$$
$$ = \pi_k P(X_1 | Y = k) P(X_2 | X_1, Y=k) P(X_3,\ldots,X_m | X_1,X_2, Y=k)$$
$$ = \pi_k P(X_1 | Y = k) P(X_2 | X_1, Y=k)\ldots P(X_m|X_1\ldots,X_{m-1},Y=k)$$

We could make an assumption to write this:

$$ \approx \pi_k P(X_1 | Y = k) P(X_2 | Y = k)\ldots P(X_m |,Y=k)$$

---

## Example: Iris Data

```{r iris, cache=TRUE}
data(iris); library(ggplot2)
names(iris)
table(iris$Species)
```


---

## Create training and test sets

```{r trainingTest, dependson="iris",cache=TRUE}
inTrain <- createDataPartition(y=iris$Species,
                              p=0.7, list=FALSE)
training <- iris[inTrain,]
testing <- iris[-inTrain,]
dim(training); dim(testing)
```

---

## Build predictions

```{r fit,dependson="trainingTest"}
modlda = train(Species ~ .,data=training,method="lda")
modnb = train(Species ~ ., data=training,method="nb")
plda = predict(modlda,testing); pnb = predict(modnb,testing)
table(plda,pnb)
```


---

## Comparison of results

```{r,dependson="fit",fig.height=4,fig.width=4}
equalPredictions = (plda==pnb)
qplot(Petal.Width,Sepal.Width,colour=equalPredictions,data=testing)
```

---

## Notes and further reading

* [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)
* [Elements of Statistical Learning](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)
* [Model based clustering](http://www.stat.washington.edu/raftery/Research/PDF/fraley2002.pdf)
* [Linear Discriminant Analysis](http://en.wikipedia.org/wiki/Linear_discriminant_analysis)
* [Quadratic Discriminant Analysis](http://en.wikipedia.org/wiki/Quadratic_classifier)

# Lesson (24) Regularized  Regression



## Basic idea

1. Fit a regression model
2. Penalize (or shrink) large coefficients

__Pros:__

* Can help with the bias/variance tradeoff
* Can help with model selection

__Cons:__

* May be computationally demanding on large data sets
* Does not perform as well as random forests and boosting



---

## A motivating example

$$Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \epsilon$$

where $X_1$ and $X_2$ are nearly perfectly correlated (co-linear). You can approximate this model by:

$$Y = \beta_0 + (\beta_1 + \beta_2)X_1 + \epsilon$$

The result is:

* You will get a good estimate of $Y$
* The estimate (of $Y$) will be biased 
* We may reduce variance in the estimate

---

## Prostate cancer 

```{r prostate}
library(ElemStatLearn); data(prostate)
str(prostate)
```



---

## Subset selection

<img class="center" src="../../assets/img/prostate.png" height="450">

[Code here](http://www.cbcb.umd.edu/~hcorrada/PracticalML/src/selection.R)


---

## Most common pattern

<img class="center" src="../../assets/img/trainingandtest.png" height="450">

http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/

---

## Model selection approach: split samples

* No method better when data/computation time permits it

* Approach
  1. Divide data into training/test/validation
  2. Treat validation as test data, train all competing models on the train data and pick the best one on validation. 
  3. To appropriately assess performance on new data apply to test set
  4. You may re-split and reperform steps 1-3

* Two common problems
  * Limited data
  * Computational complexity
  
http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/
http://www.cbcb.umd.edu/~hcorrada/PracticalML/


---

## Decomposing expected prediction error

Assume $Y_i = f(X_i) + \epsilon_i$

$EPE(\lambda) = E\left[\{Y - \hat{f}_{\lambda}(X)\}^2\right]$

Suppose $\hat{f}_{\lambda}$ is the estimate from the training data and look at a new data point $X = x^*$

$$E\left[\{Y - \hat{f}_{\lambda}(x^*)\}^2\right] = \sigma^2 + \{E[\hat{f}_{\lambda}(x^*)] - f(x^*)\}^2 + var[\hat{f}_\lambda(x_0)]$$

<center> = Irreducible error + Bias$^2$ + Variance </center>

http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/
http://www.cbcb.umd.edu/~hcorrada/PracticalML/

---

## Another issue for high-dimensional data

```{r ,dependson="prostate"}
small = prostate[1:5,]
lm(lpsa ~ .,data =small)
```

http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/
http://www.cbcb.umd.edu/~hcorrada/PracticalML/

---

## Hard thresholding

* Model $Y = f(X) + \epsilon$

* Set $\hat{f}_{\lambda}(x) = x'\beta$

* Constrain only $\lambda$ coefficients to be nonzero. 

* Selection problem is after chosing $\lambda$ figure out which $p - \lambda$ coefficients to make nonzero

http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/
http://www.cbcb.umd.edu/~hcorrada/PracticalML/

---

## Regularization for regression

If the $\beta_j$'s are unconstrained:
* They can explode
* And hence are susceptible to very high variance

To control variance, we might regularize/shrink the coefficients. 

$$ PRSS(\beta) = \sum_{j=1}^n (Y_j - \sum_{i=1}^m \beta_{1i} X_{ij})^2 + P(\lambda; \beta)$$

where $PRSS$ is a penalized form of the sum of squares. Things that are commonly looked for

* Penalty reduces complexity
* Penalty reduces variance
* Penalty respects structure of the problem

---

## Ridge regression

Solve:

$$ \sum_{i=1}^N \left(y_i - \beta_0 + \sum_{j=1}^p x_{ij}\beta_j \right)^2 + \lambda \sum_{j=1}^p \beta_j^2$$

equivalent to solving

$\sum_{i=1}^N \left(y_i - \beta_0 + \sum_{j=1}^p x_{ij}\beta_j \right)^2$ subject to $\sum_{j=1}^p \beta_j^2 \leq s$ where $s$ is inversely proportional to $\lambda$ 


Inclusion of $\lambda$ makes the problem non-singular even if $X^TX$ is not invertible.

http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/
http://www.cbcb.umd.edu/~hcorrada/PracticalML/


---

## Ridge coefficient paths

<img class="center" src="../../assets/img/ridgepath.png" height="450">

http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/
http://www.cbcb.umd.edu/~hcorrada/PracticalML/

---

## Tuning parameter $\lambda$

* $\lambda$ controls the size of the coefficients
* $\lambda$ controls the amount of {\bf regularization}
* As $\lambda \rightarrow 0$ we obtain the least square solution
* As $\lambda \rightarrow \infty$ we have $\hat{\beta}_{\lambda=\infty}^{ridge} = 0$


---

## Lasso 

$\sum_{i=1}^N \left(y_i - \beta_0 + \sum_{j=1}^p x_{ij}\beta_j \right)^2$ subject to $\sum_{j=1}^p |\beta_j| \leq s$ 

also has a lagrangian form 

$$ \sum_{i=1}^N \left(y_i - \beta_0 + \sum_{j=1}^p x_{ij}\beta_j \right)^2 + \lambda \sum_{j=1}^p |\beta_j|$$

For orthonormal design matrices (not the norm!) this has a closed form solution

$$\hat{\beta}_j = sign(\hat{\beta}_j^0)(|\hat{\beta}_j^0 - \gamma)^{+}$$
 
but not in general. 

http://www.biostat.jhsph.edu/~ririzarr/Teaching/649/
http://www.cbcb.umd.edu/~hcorrada/PracticalML/


---

## Notes and further reading


* [Hector Corrada Bravo's Practical Machine Learning lecture notes](http://www.cbcb.umd.edu/~hcorrada/PracticalML/)
* [Hector's penalized regression reading list](http://www.cbcb.umd.edu/~hcorrada/AMSC689.html#readings)
* [Elements of Statistical Learning](http://statweb.stanford.edu/~tibs/ElemStatLearn/)
* In `caret` methods are:
  * `ridge`
  * `lasso`
  * `relaxo`

# Lesson (25) Combining Predictors 


## Key ideas

* You can combine classifiers by averaging/voting
* Combining classifiers improves accuracy
* Combining classifiers reduces interpretability
* Boosting, bagging, and random forests are variants on this theme

---

## Netflix prize

BellKor = Combination of 107 predictors 

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/netflix.png height=450>

[http://www.netflixprize.com//leaderboard](http://www.netflixprize.com//leaderboard)

---

## Heritage health prize - Progress Prize 1

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/makers.png height=200>
[Market Makers](https://kaggle2.blob.core.windows.net/wiki-files/327/e4cd1d25-eca9-49ca-9593-b254a773fe03/Market%20Makers%20-%20Milestone%201%20Description%20V2%201.pdf)

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/mestrom.png height=200>

[Mestrom](https://kaggle2.blob.core.windows.net/wiki-files/327/09ccf652-8c1c-4a3d-b979-ce2369c985e4/Willem%20Mestrom%20-%20Milestone%201%20Description%20V2%202.pdf)


---

## Basic intuition - majority vote

Suppose we have 5 completely independent classifiers

If accuracy is 70% for each:
  * $10\times(0.7)^3(0.3)^2 + 5\times(0.7)^4(0.3)^2 + (0.7)^5$
  * 83.7% majority vote accuracy

With 101 independent classifiers
  * 99.9% majority vote accuracy
  

---

## Approaches for combining classifiers

1. Bagging, boosting, random forests
  * Usually combine similar classifiers
2. Combining different classifiers
  * Model stacking
  * Model ensembling

---

## Example with Wage data

__Create training, test and validation sets__

```{r wage}
library(ISLR); data(Wage); library(ggplot2); library(caret);
Wage <- subset(Wage,select=-c(logwage))

# Create a building data set and validation set
inBuild <- createDataPartition(y=Wage$wage,
                              p=0.7, list=FALSE)
validation <- Wage[-inBuild,]; buildData <- Wage[inBuild,]

inTrain <- createDataPartition(y=buildData$wage,
                              p=0.7, list=FALSE)
training <- buildData[inTrain,]; testing <- buildData[-inTrain,]
```


---

## Wage data sets

__Create training, test and validation sets__

```{r, dependson="wage"}
dim(training)
dim(testing)
dim(validation)
```


---

## Build two different models

```{r modFit,dependson="wage"}
mod1 <- train(wage ~.,method="glm",data=training)
mod2 <- train(wage ~.,method="rf",
              data=training, 
              trControl = trainControl(method="cv"),number=3)
```


---

## Predict on the testing set 

```{r predict,dependson="modFit",fig.height=4,fig.width=6}
pred1 <- predict(mod1,testing); pred2 <- predict(mod2,testing)
qplot(pred1,pred2,colour=wage,data=testing)
```


---

## Fit a model that combines predictors

```{r combine,dependson="predict"}
predDF <- data.frame(pred1,pred2,wage=testing$wage)
combModFit <- train(wage ~.,method="gam",data=predDF)
combPred <- predict(combModFit,predDF)
```


---

## Testing errors

```{r ,dependson="combine"}
sqrt(sum((pred1-testing$wage)^2))
sqrt(sum((pred2-testing$wage)^2))
sqrt(sum((combPred-testing$wage)^2))
```


---

## Predict on validation data set

```{r validation,dependson="combine"}
pred1V <- predict(mod1,validation); pred2V <- predict(mod2,validation)
predVDF <- data.frame(pred1=pred1V,pred2=pred2V)
combPredV <- predict(combModFit,predVDF)
```


---

## Evaluate on validation

```{r ,dependson="validation"}
sqrt(sum((pred1V-validation$wage)^2))
sqrt(sum((pred2V-validation$wage)^2))
sqrt(sum((combPredV-validation$wage)^2))
```



---

## Notes and further resources

* Even simple blending can be useful
* Typical model for binary/multiclass data
  * Build an odd number of models
  * Predict with each model
  * Predict the class by majority vote
* This can get dramatically more complicated
  * Simple blending in caret: [caretEnsemble](https://github.com/zachmayer/caretEnsemble) (use at your own risk!)
  * Wikipedia [ensemlbe learning](http://en.wikipedia.org/wiki/Ensemble_learning)

---

## Recall - scalability matters

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/netflixno.png height=250>
</br></br></br>

[http://www.techdirt.com/blog/innovation/articles/20120409/03412518422/](http://www.techdirt.com/blog/innovation/articles/20120409/03412518422/)

[http://techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html](http://techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html)

# Lesson (26) Unsupervised Redictions  


## Key ideas

* Sometimes you don't know the labels for prediction
* To build a predictor
  * Create clusters
  * Name clusters
  * Build predictor for clusters
* In a new data set
  * Predict clusters


---

## Iris example ignoring species labels

```{r iris}
data(iris); library(ggplot2)
inTrain <- createDataPartition(y=iris$Species,
                              p=0.7, list=FALSE)
training <- iris[inTrain,]
testing <- iris[-inTrain,]
dim(training); dim(testing)
```


---

## Cluster with k-means

```{r kmeans,dependson="iris",fig.height=4,fig.width=6}
kMeans1 <- kmeans(subset(training,select=-c(Species)),centers=3)
training$clusters <- as.factor(kMeans1$cluster)
qplot(Petal.Width,Petal.Length,colour=clusters,data=training)
```


---

## Compare to real labels

```{r ,dependson="kmeans"}
table(kMeans1$cluster,training$Species)
```



---

## Build predictor

```{r modelFit,dependson="kmeans"}
modFit <- train(clusters ~.,data=subset(training,select=-c(Species)),method="rpart")
table(predict(modFit,training),training$Species)
```

---

## Apply on test

```{r ,dependson="modFit"}
testClusterPred <- predict(modFit,testing) 
table(testClusterPred ,testing$Species)
```


---

## Notes and further reading

* The cl_predict function in the clue package provides similar functionality
* Beware over-interpretation of clusters!
* This is one basic approach to [recommendation engines](http://en.wikipedia.org/wiki/Recommender_system)
* [Elements of statistical learning](http://www-stat.stanford.edu/~tibs/ElemStatLearn/)
* [Introduction to statistical learning](http://www-bcf.usc.edu/~gareth/ISL/)

# Lesson (27) Forecasting 


## Time series data

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/GOOG.png height=450>

[https://www.google.com/finance](https://www.google.com/finance)

---

## What is different?

* Data are dependent over time
* Specific pattern types
  * Trends - long term increase or decrease
  * Seasonal patterns - patterns related to time of week, month, year, etc.
  * Cycles - patterns that rise and fall periodically
* Subsampling into training/test is more complicated
* Similar issues arise in spatial data 
  * Dependency between nearby observations
  * Location specific effects
* Typically goal is to predict one or more observations into the future. 
* All standard predictions can be used (with caution!)

---

## Beware spurious correlations!


<img class=center src=../../assets/img/08_PredictionAndMachineLearning/spurious.jpg height=450>

[http://www.google.com/trends/correlate](http://www.google.com/trends/correlate)

[http://www.newscientist.com/blogs/onepercent/2011/05/google-correlate-passes-our-we.html](http://www.newscientist.com/blogs/onepercent/2011/05/google-correlate-passes-our-we.html)

---

## Also common in geographic analyses

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/heatmap.png height=450>

[http://xkcd.com/1138/](http://xkcd.com/1138/)


---

## Beware extrapolation!

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/extrapolation.jpg height=450>

[http://www.nature.com/nature/journal/v431/n7008/full/431525a.html](http://www.nature.com/nature/journal/v431/n7008/full/431525a.html)

---

## Google data


```{r loadGOOG}
library(quantmod)
from.dat <- as.Date("01/01/08", format="%m/%d/%y")
to.dat <- as.Date("12/31/13", format="%m/%d/%y")
getSymbols("GOOG", src="google", from = from.dat, to = to.dat)
head(GOOG)
```

---

## Summarize monthly and store as time series

```{r tseries,dependson="loadGOOG",fig.height=4,fig.width=4}
mGoog <- to.monthly(GOOG)
googOpen <- Op(mGoog)
ts1 <- ts(googOpen,frequency=12)
plot(ts1,xlab="Years+1", ylab="GOOG")
```


---

## Example time series decomposition

* __Trend__  - Consistently increasing pattern over time 
* __Seasonal__ -  When there is a pattern over a fixed period of time that recurs.
* __Cyclic__ -  When data rises and falls over non fixed periods

[https://www.otexts.org/fpp/6/1](https://www.otexts.org/fpp/6/1)


---

## Decompose a time series into parts

```{r ,dependson="tseries",fig.height=4.5,fig.width=4.5}
plot(decompose(ts1),xlab="Years+1")
```

---

## Training and test sets

```{r trainingTest,dependson="tseries",fig.height=4.5,fig.width=4.5}
ts1Train <- window(ts1,start=1,end=5)
ts1Test <- window(ts1,start=5,end=(7-0.01))
ts1Train
```

---

## Simple moving average

$$ Y_{t}=\frac{1}{2*k+1}\sum_{j=-k}^k {y_{t+j}}$$

```{r ,dependson="trainingTest",fig.height=4.5,fig.width=4.5}
plot(ts1Train)
lines(ma(ts1Train,order=3),col="red")
```



---

## Exponential smoothing

__Example - simple exponential smoothing__
$$\hat{y}_{t+1} = \alpha y_t + (1-\alpha)\hat{y}_{t-1}$$

<img class=center src=../../assets/img/08_PredictionAndMachineLearning/expsmooth.png height=300>

[https://www.otexts.org/fpp/7/6](https://www.otexts.org/fpp/7/6)

---

## Exponential smoothing

```{r ets,dependson="trainingTest",fig.height=4.5,fig.width=4.5}
ets1 <- ets(ts1Train,model="MMM")
fcast <- forecast(ets1)
plot(fcast); lines(ts1Test,col="red")
```


---

## Get the accuracy

```{r ,dependson="ets",fig.height=4.5,fig.width=4.5}
accuracy(fcast,ts1Test)
```

---

## Notes and further resources

* [Forecasting and timeseries prediction](http://en.wikipedia.org/wiki/Forecasting) is an entire field
* Rob Hyndman's [Forecasting: principles and practice](https://www.otexts.org/fpp/) is a good place to start
* Cautions
  * Be wary of spurious correlations
  * Be careful how far you predict (extrapolation)
  * Be wary of dependencies over time
* See [quantmod](http://cran.r-project.org/web/packages/quantmod/quantmod.pdf) or [quandl](http://www.quandl.com/help/packages/r) packages for finance-related problems.





