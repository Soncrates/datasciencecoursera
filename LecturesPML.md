# Week 1 Practical Machine Learning

## Key ideas

question -> input data -> features -> algorithm -> parameters -> evaluation

http://radar.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html

## Prediction is about accuracy tradeoffs
- Interpretability versus accuracy
- Speed versus accuracy
- Simplicity versus accuracy
- Scalability versus accuracy

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

## Types of Errors
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
