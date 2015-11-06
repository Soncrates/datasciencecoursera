# Week 1 Practical Machine Learning

## Lesson 

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

## May be automated with care

<center> question -> input data -> <rt>features</rt> -> algorithm -> parameters -> evaluation  </center>

[http://arxiv.org/pdf/1112.6209v5.pdf](http://arxiv.org/pdf/1112.6209v5.pdf)

## Algorithms matter less than you'd think

<center> question -> input data -> features -> <rt>algorithm</rt> -> parameters -> evaluation  </center>

[http://arxiv.org/pdf/math/0606441.pdf](http://arxiv.org/pdf/math/0606441.pdf)

## Issues to consider

[http://strata.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html](http://strata.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html)

## Prediction is about accuracy tradeoffs

* Interpretability versus accuracy
* Speed versus accuracy
* Simplicity versus accuracy
* Scalability versus accuracy

## Interpretability matters

[http://www.cs.cornell.edu/~chenhao/pub/mldg-0815.pdf](http://www.cs.cornell.edu/~chenhao/pub/mldg-0815.pdf)

## Scalability matters

# Lesson

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

# Lesson

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

# Lesson

http://radar.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html

*In Sample Error: The error rate you get on the same data set you used to build your predictor. Sometimes called resubstitution error.*

Out of Sample Error: The error rate you get on a new data set. Sometimes called generalization error.

## Key ideas

Out of sample error is what you care about
In sample error < out of sample error
The reason is overfitting
Matching your algorithm to the data you have

Data have two parts
Signal
Noise
The goal of a predictor is to find signal
You can always design a perfect in-sample predictor
You capture both signal + noise when you do that
Predictor won't perform as well on new samples

https://en.wikipedia.org/wiki/Overfitting

### Prediction study design
- Define your error rate
- Split data into:
  - Training, Testing, Validation (optional)
- On the training set pick features
  - Use cross-validation
- On the training set pick prediction function
  - Use cross-validation
- If no validation
  - Apply 1x to test set
- If validation
  - Apply to test set and refine
  - Apply 1x to validation

### Know the benchmarks
http://www.heritagehealthprize.com/c/hhp/leaderboard

### Study Design
http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf

https://www.kaggle.com/

## Rules of thumb for prediction study design
- If you have a large sample size
  - 60% training
  - 20% test
  - 20% validation
- If you have a medium sample size
  - 60% training
  - 40% testing
- If you have a small sample size
  - Do cross validation
  - Report caveat of small sample size

## Some principles to remember
- Set the test/validation set aside and don't look at it
- In general randomly sample training and test
- Your data sets must reflect structure of the problem
  - If predictions evolve with time split train/test in time chunks (calledbacktesting in finance)
- All subsets should reflect as much diversity as possible
  - Random assignment does this
  - You can also try to balance by features - but this is tricky

Backtesting is a term used in oceanography, meteorology and the financial industry to refer to testing a predictive model using existing historic data. Backtesting is a kind of retrodiction, and a special type of cross-validation applied to time series data.
https://en.wikipedia.org/wiki/Backtesting

## Lesson: Types of Errors
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


<img class=center src=../../assets/img/keyquantities.png height=500>


[http://en.wikipedia.org/wiki/Sensitivity_and_specificity](http://en.wikipedia.org/wiki/Sensitivity_and_specificity)

http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## Key quantities as fractions


<img class=center src=../../assets/img/keyquantfrac.png height=500>
http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/


---

## Screening tests


<img class=center src=../../assets/img/predpos1.png height=500>
http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## General population


<img class=center src=../../assets/img/predpos2.png height=500>
http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## General population as fractions


<img class=center src=../../assets/img/predpos3.png height=500>
http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## At risk subpopulation

<img class=center src=../../assets/img/predpos4.png height=500>
http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## At risk subpopulation as fraction

<img class=center src=../../assets/img/predpos5.png height=500>
http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/


---

## Key public health issue 

<img class=center src=../../assets/img/mammograms.png height=500>
http://www.biostat.jhsph.edu/~iruczins/teaching/140.615/

---

## Key public health issue 

<img class=center src=../../assets/img/prostatescreen.png height=500>



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
5. Predictive value of a positive (precision)
  * When you are screeing and prevelance is low

---

- In general, Positive = identified and negative = rejected. Therefore:
  - True positive = correctly identified
  - False positive = incorrectly identified
  - True negative = correctly rejected
  - False negative = incorrectly rejected
- Medical testing example:
  - True positive = Sick people correctly diagnosed as sick
  - False positive= Healthy people incorrectly identified as sick
  - True negative = Healthy people correctly identified as healthy
  - False negative = Sick people incorrectly identified as healthy.

### Key Quantities
![bayes](https://camo.githubusercontent.com/f3156300abccf94ee861480c58b664675d421044/687474703a2f2f64617461736369656e63657370656369616c697a6174696f6e2e6769746875622e696f2f636f75727365732f6173736574732f696d672f6b65797175616e7469746965732e706e67)

http://en.wikipedia.org/wiki/Sensitivity_and_specificity

### Key Quantities as Fractions
![bayesAsFractions](https://camo.githubusercontent.com/cee0e1db1482884ad9ceb8d364fa85d7a2de8bd0/687474703a2f2f64617461736369656e63657370656369616c697a6174696f6e2e6769746875622e696f2f636f75727365732f6173736574732f696d672f6b65797175616e74667261632e706e67)

#### Example
- Assume that some disease has 0.1% prevalence in the population.
- Assume we have a test kit for that disease.
  - 99% Sensitivity
  - 99% Specificity
- What is the probablility of a person having the disease given a positive test result if we randomly select that person from :
  - the general population
  - a high risk sub-population with 10% disease prevalence?

### General population as fractions
![bayesAsFractionsGenPop](https://camo.githubusercontent.com/b32484d8d9b814e205ee45de64ce8c5479c3886b/687474703a2f2f64617461736369656e63657370656369616c697a6174696f6e2e6769746875622e696f2f636f75727365732f6173736574732f696d672f70726564706f73332e706e67)

### At risk subpopulation as fraction
![bayesAsFractionsHighRisk](https://camo.githubusercontent.com/6269842963a050e75da9dd2f14999df624ef1fcb/687474703a2f2f64617461736369656e63657370656369616c697a6174696f6e2e6769746875622e696f2f636f75727365732f6173736574732f696d672f70726564706f73352e706e67)

## For continuous data

### Mean squared error (MSE):

1/n(∑i=1)(for i = 1 through N)  (Prediction(sub i)−Truth(sub i))^2

### Root mean squared error (RMSE):

sqrt(1/n(∑i=1)(for i = 1 through N)  (Predictioni−Truthi)^2)

## Common error measures
- Mean squared error (or root mean squared error)
  - Continuous data, sensitive to outliers
- Median absolute deviation
  - Continuous data, often more robust
- Sensitivity (recall)
  - If you want few missed positives
- Specificity
  - If you want few negatives called positives
- Accuracy
  - Weights false positives/negatives equally
- Concordance
  - One example is kappa
- Predictive value of a positive (precision)
  - When you are screeing and prevelance is low


## Receiver Operating Characteristic

### Why a curve? 

* In binary classification you are predicting one of two categories
  * Alive/dead
  * Click on ad/don't click
* But your predictions are often quantitative
  * Probability of being alive
  * Prediction on a scale from 1 to 10
* The _cutoff_  you choose gives different results

### ROC curves
[http://en.wikipedia.org/wiki/Receiver_operating_characteristic](http://en.wikipedia.org/wiki/Receiver_operating_characteristic)

### Area under the curve
* AUC = 0.5: random guessing
* AUC = 1: perfect classifer
* In general AUC of above 0.8 considered "good"

## Cross VAlidation

[http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf](http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf)

## Key idea

1. Accuracy on the training set (resubstitution accuracy) is optimistic
2. A better estimate comes from an independent set (test set accuracy)
3. But we can't use the test set when building the model or it becomes part of the training set
4. So we estimate the test set accuracy with the training set. 

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

## Random subsampling

## K-fold

## Leave one out

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
