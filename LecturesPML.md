question -> input data -> features -> algorithm -> parameters -> evaluation

http://radar.oreilly.com/2013/09/gaining-access-to-the-best-machine-learning-methods.html

**Prediction is about accuracy tradeoffs
-Interpretability versus accuracy
-Speed versus accuracy
-Simplicity versus accuracy
-Scalability versus accuracy

In Sample Error: The error rate you get on the same data set you used to build your predictor. Sometimes called resubstitution error.

Out of Sample Error: The error rate you get on a new data set. Sometimes called generalization error.

Key ideas

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

*** Prediction study design
-Define your error rate
-Split data into:
--Training, Testing, Validation (optional)
-On the training set pick features
--Use cross-validation
-On the training set pick prediction function
--Use cross-validation
-If no validation
--Apply 1x to test set
-If validation
--Apply to test set and refine
--Apply 1x to validation

Know the benchmarks
http://www.heritagehealthprize.com/c/hhp/leaderboard

Study Design
http://www2.research.att.com/~volinsky/papers/ASAStatComp.pdf

https://www.kaggle.com/

**Rules of thumb for prediction study design
-If you have a large sample size
--60% training
--20% test
--20% validation
-If you have a medium sample size
--60% training
--40% testing
-If you have a small sample size
--Do cross validation
--Report caveat of small sample size

**Some principles to remember
-Set the test/validation set aside and don't look at it
-In general randomly sample training and test
-Your data sets must reflect structure of the problem
--If predictions evolve with time split train/test in time chunks (calledbacktesting in finance)
-All subsets should reflect as much diversity as possible
--Random assignment does this
--You can also try to balance by features - but this is tricky

Backtesting is a term used in oceanography, meteorology and the financial industry to refer to testing a predictive model using existing historic data. Backtesting is a kind of retrodiction, and a special type of cross-validation applied to time series data.
https://en.wikipedia.org/wiki/Backtesting
