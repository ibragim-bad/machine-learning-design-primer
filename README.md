# machine-learning-design-cases

Repo for machine learning system design cases solving. I create this repo for gathering links and some notes on solving ML system design interview cases. 


## Overview:

1. Framework for solving MLSD cases
2. Concepts that will help in different cases (e.g. imbalanced data, data drift, embeddings, etc).
3. List of cases with notes on solving (e.g. recsys for e-commerce, automoderation, face identification)

The main resources:

### Framework by Facebook

https://research.facebook.com/blog/2018/05/the-facebook-field-guide-to-machine-learning-video-series/


### Overall
1. Problem definition
2. Data
3. Evaluation
4. Features
5. Model
6. Experimentation (Online Learning)

Overall tips
+ ask question 
+ think about pros and cons of solution
+ trade-offs 
+ start with simple solution

First, start with questions. Try to repeat the case to check that you understand it correcctly.
Ask questions.
   1. What is succces?
   2. Understand Bussiness goal. What are we trying to do? Increase revenue, retention, engagement.

### Problem definition. 
Define Proxy machine learning metric for Business goal. Is it classfication, regression taks, clustering? ML metric is proxy, so think about overall business goal, when improving baseline. (Users won't be happy if our new model will decrease UX speed)
   1. Define ML task type - Framing ML Problem - clf.regre
   2. Define requirements.
      1. speed, memory
      2. Online or batch
      3. Cloud or edge
   3. Decoupling objective - multiple goals. e.g. feed -  maximize users engagement while minimizing the spread of extreme views and misinformation. Or in delivery time prediction = cooking time + delivery time.


### Data. 

1. Data source and data type
   1. What we have as data. How many?
   2. What is one sample of data. X and Y
   3. How we can find the labels? Natural labeling?
2. Sampling - sampling from all possible real-world data to create training data
   1. Non-probabilty/Probability
3. Data recency and Distribution drift.
   1. How fast is data refreshing 


### Evaluation



--------

   2. What we have as data. 
   3. What is one sample of data. X and Y
   4. How we can find the labels? Natural labeling?
   5.  Data recency and real-time training
   6.  Data modality. Numerical, categorical, text, date. Start with the most informative data type
   7.  Training/prediction consistency
   8.  Records and sampling
   9.  What data we can store static, is there some dynamic features from data? Aggregations
   10. Distribution drift. What is the speed new data building.



4. Data source
    1. User generated - user data
    2. Sys generated - internal data
    3. 3party data, collect from all, mention GDPR
5. Data formats
    1. Row or column-major
        1. Row - fast writes
        2. Column - fast column reads
    2. Txt/binary
6. Data models
    1. Relational
    2. Nosql - not only sql
        1. Doc
        2. Graph
    3. Structured/UnStructured 
7. Data storage engines and processing
8. ETL /ELT


Sampling - sampling from all possible real-world data to create training data
1. Non-Probability Sampling - selective bias - OK for init
    1. Convenience - what is available 
    2. Snowball - collect friend of friends ->
    3. Judgement - expert decide
    4. Quota - e.g. 30 from each age group
2. Simple random sampling /Stratified  /Weighted 
3. Importance sampling (RL)
4. Reservoir sampling - from stream
    1. K samples in res, N - len of current stream
    2. For new elem generate random (1, n), if <k: replace in k
    3. Correct probe for each sample in res



Labeling
1. Types
    1. Hand label
    2. Natural label e.g. - like/dislike, click etc
2. Hand label
    1. Measure of consent - flies kappa
    2. Data lineage - preserve source of data, if its labelling worsen model
3. Handling the lack of hand labels
    1. Weak supervision - define labeling function(e.g. regex) and label data. May be noisy
    2. Semi-supervision - train then label w model
    3. Transfer learning
    4. Active learning/query learning - decide samples to label
4. Class imbalance
    1. Using the right metric, based on error cost
    2. Data-level - resample
        1. Under/oversample
        2. Smote - oversample w new points btw existing 
    3. Algorithm-level
        1. Balanced loss
        2. Focal loss - hard examples > weight CE*(1-pt)^hamma

   
### Evaluation

1. Offline evaluation
   1. How to split data?
      1. classic K-FOLD (tr+val) + ts
      2. If time sensitive data:
         1. split by time  
         2. splitting by time + margin
         3. preq splitting 
      3. If user/product sensitive data
         1. split by user/product 
      4. If cold start
         1. drop some data from history
         2. make some users with empty or min history 
   2. ?Spec/Spars trade-off
   3. Choose metric 
      1. interpretible. You can say what exatly metric is showing.
      2. Sensitive to task. By metric we can say in the context of task what model is better.
   4. ?Calibration on test.
   5. Slicing. Slice by some features to find model failures.
2. Online evaluation. 


1. Baseline evaluation
    1. Random / Majority / Simple heuristic / Human/ existing sol
2. Evaluation methods 
    1. Perturbation test - noisy samples
    2. Invariance (Fairness)
    3. Directional expectations - sanity check
    4. Model calibration - if proba 0.8, then 80% of preds - this class
    5. Confidence measure
    6. Slice based. How find slice?
        1. Heuristic
        2. Error analysis
        3. Slice finder algos. Generate with beam search than check



### Features

1. What type of data we have? And what we can do with these data.
2. Encoding. Embeddings. Other stuff.
3. Is there a feedback loop in features? 

Feature Engineering

Data Augmentation
1. Simple Label-preserving - synonyms 
2. Perturbation/Adverserial - add hard samples - noise
3. Data synth - add new samples (use diff model)

Feature engineering
1. Handling missing values
2. Scaling - e.g. log for skew
3. Discretization
4. Encoding. Hashing trick
5. Feature crossing, e.g. recsys to add nonlinear
6. Pos embeddings, e.g. pos enc in bert





### Model 
1. Pick. 
   1. Start with baseline.
   2. Compare models with baseline 
2. Tune.
   1. 

### Online learning 

1. Online-offline gap
2. Online
    1. A/B randomised test
    2. Minimise time to first online
    3. Isolate eng bug from ml issues
        1. A/A test before adding real model w new system
    4. Real world FL
        1. Back test + forward test
    5. More metrics to check correctness
    6. Triangulate causes, so iterate atomic steps
    7. Have backup plan
    8. Calibration average prediction = average gt prediction 
        1. Misc on train - under, test - overfit, online - on/offline gap
    9. 



## Optimization

Deployment
1. Batch prediction vs Online prediction
    1. Batch prediction - async - predict, than return from DB. High throughput
    2. Online - sync - get request - predict. Low latency
2. Model compression
    1. Low-rank factorisation. CNN - mobile net
    2. Knowledge distillation
    3. Pruning 
    4. Quantization
3. Edge / Cloud computing

Monitoring & Continual Learning

1. Monitoring -> Continual Learning
    1. Detect distribution shift -> adapt with CL
    2. Check if more frerquent will boost
2. Eval
    1. Offline - sanity check
    2. Online
        1. Canary - two models old and new, slowly route traffic
        2. A/B - test
        3. Interleaved experiments - preds from 2 models are mixed
        4. Shadow test - log prediction from new, then study
3. Internal test on coworkers. But it is a sanity check



--- 


Ideas
two versions

Slim  - 10 min reading 
Large - all data stuff

Memory = Data + Features (emb cache) + model 
Speed = (data processing + featurizing + model inference) + other services



concepts
metric learning + dssm 
embeddings + semantic space
Feedback loop
Bias + fairness 
Overfitting + shortcut learning 
Natural labels 
