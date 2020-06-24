# Paper

- __Title:__ Deontological Ethics By Monotonicity Shape Constraints
- __Authors:__ Serena Wang, Maya Gupta (_Google Research_)
- __Link:__ http://proceedings.mlr.press/v108/wang20e/wang20e.pdf
- __Platform:__ International Conference on Artificial Intelligence and Statistics (AISTATS)
- __Year:__ 2020

# Summary

### What?

* Non-linear ML models can violate social norms or [deontological ethical](https://en.wikipedia.org/wiki/Deontological_ethics) principles.
* Can incorporation of monotone behavior in ML models via shape constraints help overcome this?
* Can these deontological constraints be related to commonly known notions of statistical fairness (parity and equal opportunity)?
* The proposal intends to help accelerate progress towards a trustworthy and responsible AI.

### Monotonicity Fairness Constraints

* __Unfair Penalization (UP):__ There may be inputs that a responsible model may reward but should _never_ penalize.
* __Favor the less fortunate (FLF):__ There may be inputs that help us identify the less fortunate and _favor them_ if there are no other relevant differences.

#### How?

* UP and FLF are addressed by constraining the ML model to __only__ respond positively to relevant inputs _if all other inputs are fixed_. 
Such monotonicity shape restrictions are referred to as _ceterus paribus_.
* Focus is on ML models that produce a score which is used to determine some benefit, such as better credit rating or a scholarship. 

### Motivating Examples

1. __Resume Score (UP):__ Based on the candidate's years of experience, best fit model sometimes penalizes candidates for having <i>more</i> job experience. 
The model picks the age discrimination in the biased training samples.
    * Monotonicity shape constraints guarantee that model uses job experience as __positive__ evidence.
    
> Such unfair responses are very much a danger in sparse regions of a feature space with modern over-parameterized nonlinear models.
 
2. __Law School Admissions (UP):__ Standard 2-DNN (and, gradient boosted tree) violates merit-based social norms and the "best-qualified" ethical principle 
by penalizing people for higher GPA or rewarding for lower GPA. ("objectionable model")
    * Training a monotone calibrated linear model helps the cause, with similar test accuracy compared to DNN and GBT.
    
> Non-uniform distribution of the training data means that some regions of the feature space were sparse and model may have overfit.

3. __Crimes and Misdemeanors:__ 
    * (UP) Fines for misdemeanors would be fairer if they increase monotonically w.r.t. magnitude of illegality.
    * (FLF) Juvenile shouldn't penalized more heavily than an adult for the same crime, all else equal.
    * (FLF) Prefer to not give harsher penalties to first-time offenders than to repeat offenders.

4. __Pay:__
    * (UP) Unfair to get paid less for doing more of the same work.
    * (UP) Desirable to workers if the recommended pay were constrained to be increasing monotonically as a function of the number of hours worked, all else equal.
    * (UP) Larger tips for expensive meals and higher tip percentages for more upscale establishments, in accordance with American societal norms.
    
5. __Medical Triage:__
    * (FLF) More ethical or responsible to prioritize patients based on their risk or neediness.
    * (UP) Emergency room prioritization should treat patients that have waited longer first, if all other relevant characteristics are equal.

### Contributions

* __Need for Monotonicity:__ Demonstrate that violating monotonicity in ML models can result in significant ethical issues and risk of societal harm.
These violations easily occur in practice with nonlinear ML models.
* __Existing__ (lattice) shape constrained ML approach can ameliorate such problems.
* In addition to consequentialist view of ML fairness, the authors introduce __deontological__ aspect of fairness. They further provide analysis of the 
relation between proposed monotonicity constraints and statistical fairness notions.

### Enforcing Monotonicity Constraints

Historically, monotonicity shape constraints have been used to capture prior knowledge and regularize estimation problems to improve a model's 
generalization to new test samples. _Here_, authors show that these constraints can and __should__ be used to ensure that ML models behave consistently with
societal norms and [prima facie duties](http://people.wku.edu/jan.garrett/ethics/rossethc.htm#:~:text=A%20prima%20facie%20duty%20is,by%20another%20duty%20or%20duties.&text=An%20example%20of%20a%20prima,to%20keep%20a%20promise%20made.%22).

However, it's likely that embedding monotonicity constraints to impose ethical principles may hurt test accuracy if the training and test data are biased.
(drop was minimal for the presented experiments)

* [TensorFlow Lattice 2.0 package](https://www.tensorflow.org/lattice/overview) was used for training the monotone models - calibrated linear and lattice.

#### Relation to Statistical Fairness

Major _theoretical analysis_ has been presented in this sub-section: interaction between deontological monotonicity constraints and consequentialist 
statistical fairness goals which are based on aggregate outcomes.

1. __One-sided statistical parity (OSSP):__ Due to asymmetric goals like FLF, it requires that E[f(X, Z) | Z=j] <= E[f(X, Z) | Z=k] for any j<=k, where Z is the 
intended-monotone (protected) attribute.
Though monotonicity can't guarantee OSSP, it doees imply a bound on the OSSP violation - bounded by the maximum density ratio between the two groups (Z=j and Z=k).
    * __Note:__ Converse doesn't hold.
    * Monotonic projection of f cannot have worse statistical parity violations on average. (Always doing better with _monotone_ model)
    
2. __Bound for Binary Classifiers:__ Goal of statistical parity is equivalent to marginal independence. So, the bound on statistical parity becomes a bound
on the marginal probabilities. 

3. __Equal opportunity:__ Less discussion provided on this in the paper. Read Lemma 4 for specific details of the bound.

### Experimental Results

* Q. Do monotonicity constraints badly impact the test accuracy?
* Q. What's the corresponding change in statistical fairness violations?

#### Setup

* Calibrated linear model from [TensorFlow Lattice 2.0 package](https://www.tensorflow.org/lattice/overview) was used in all the experiments.
* Piece-wise calibration was done by dividing into 20 quantiles.
* Joint training via projected SGD was further tuned by picking LR through grid search on powers of 10.

#### [Credit Default (Taiwanese)](https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients) - UP

Sourced from the UCI repository, the target is to predict whether a user defaulted on a payment in a time window, or not.
* Unfair to many if they are penalized for paying their bills sooner, all else equal. Monotonicity constraints are included to abstain the model from 
penalizing early payments.
* __1st experiment:__ Only 2 features: marital status and April repayment status (constrained).
* __2nd experiment:__ All 24 features: monotonicity constraint on all 6 repayment features to avoid penalization.
* Train and test accuracy were reported by the authors.

Monotonicity constraints have tiny effect for 1st experiment, but slightly worse for the 2nd one.

> Since the train accuracy is not hurt, authors believe the lower test accuracy is simply due to the randomness of the sample.

#### [Funding Proposals](https://www.kaggle.com/c/kdd-cup-2014-predicting-excitement-at-donors-choose) - FLF

Sourced from KDD Cup 2014, the target feature is a binary label that represents outcome of the project proposed by teachers - "exciting" (1) and 0.
* Standard ML models won't favor the schools at higher poverty levels, or projects with greater impact.
* Again, __2__ experiments are conducted: first, poverty level and number of students impacted are considered. And, then all the features are included 
(same two constrained though). 
* Train and test AUC were reported by the authors, mostly because of the inherent balance (~5%).

In this case, monotonicity constraints to prefer poorer schools and greater student impact __don't hurt__ the test AUC.

* For comparison to statistical fairness, empirical values of OSSP and equal opportunity are shown below. 
Note that the monotonically constrained models do lower violations of both fairness goals. Improvement is smaller when model had all features than just 2 features.

### Conclusion

* Non-linear ML models can easily overfit noise or learn bias in a way that violates social norms or ethics about whether certain inputs should be
allowed to negatively affect a score or decision.
* This problem can be ameliorated by training with monotonicity constraints to reflect the desired principle.
* The constraints can improve or _bound_ (consequentialist) one-sided statistical fairness violations.
* The constaints are __fairly__ easy to explain, and reason about for laypeople.

`Monotonicity constraints are a necessary and useful tool for creating responsible AI, but certainly not sufficient or application to all situations.`

### Open Questions

* For the proposed method to be completely effective, all relevant features must be identified and constrained.
* Special care could be taken to ascertain whether the non-sensitive information is highly correlated with the sensitive information (thereby, causing indirect
discrimination).
* This method is not directly applicable to __unordered inputs__ such as addresses, photos, or voice signals.
