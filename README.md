# Machine learning design manual

Some helpful notes for Machine Learning System Design Interview preparation, which I gathered from various resources to prepare for machine learning systems design interview.


## Motivation
> Learn how to design Machine Learning systems and prepare for an interview.


## The main resources 

[Facebook Field Guide to Machine Learning](https://research.facebook.com/blog/2018/05/the-facebook-field-guide-to-machine-learning-video-series/)

[CS 329S: Machine Learning Systems Design, Stanford, Winter 2022](https://stanford-cs329s.github.io/)


## Overview:

1. [Framework for solving MLSD cases](#framework-for-solving-mlsd-cases)
2. [Detailed notes on some concepts](#detailed-notes-on-some-concepts)

# Framework for solving MLSD cases


1. [Problem definition](#problem-definition)
2. [Data](#data)
3. [Evaluation](#evaluation)
4. [Features](#features)
5. [Model](#model)
6. [Further actions](#further-actions)


## Overall tips
+ Ask questions 
+ Tell pros and cons of different solutions
+ Start with simple solution as baseline

After listening the conditions of the case, ask the interviewer clarifying questions. Repeat the main points to make sure you understand everything correctly. During the interview, ask the questions by stating the assumptions. 
Examples of questions:
1. Could you give an example of succcesful use case of this system?


## Problem definition. 

Define proxy machine learning metric for the business goal. 
*ML metric is a proxy metric, so think about overall business goal, when improving baseline. (For example, users won't be happy if our new model will decrease UX speed)*
   1. Define the business goal. 
   2. Define ML task type. Classification/regression/other
   3. Split the task into subtasks. Exxample: maximize users engagement while minimizing the spread of extreme views and misinformation. 
   4. Define requirements on speed, memory.
      1. Online or batch predicting
      2. Cloud or edge computations

## Data 

1. Data source and data type
   1. Where the data comes from? Is it in the same format or should we transform and join it?
   2. One sample of data. What is X(features) and what is Y(labels)?
   3. Are the labels known? Is there Natural labeling? Should we label some data.
2. Sampling. Sampling strategy to get subset from all possible real-world data to create training and test data.
3. Data recency and Distribution drift. 


## Evaluation

1. Offline evaluation. 
   1. Data split
      1. Random split or should split by date, users, products to prevent data leakage?
      2. *Could you drop the information from some samples - Cold start*
   2. Choose interpretable and sensitive to task Metric. Think what errors will be most harmful, FP or FN for classificaction, over or underpredicting for regression.
   3. Mention baseline Non-ml-solution. You will compare your machine learning models with this baseline.
2. Online evaluation.
   1. Online-offline gap
   2. Online comparing.
      1. A/B randomised test
      2. A/A test.


## Features

1. What type of data we have? Can we encode it?
2. Feature engineering, data preprocessing.
3. Data augmentation.


## Model

1. Pick the model.
2. Pros and cons of the model.
3. What is the loss function?


## Further actions

1. Deployment
2. Experiments
3. Monitoring & Continual Learning




# Detailed notes on some concepts

## Deployment

1. Batch prediction vs Online prediction
2. Model compression
    1. Low-rank factorisation. Mobile net optimizations.
    2. Knowledge distillation
    3. Pruning 
    4. Quantization
    5. Special inference formats. (pb, onnx, torchscript)
3. Edge / Cloud computing


## Monitoring & Continual Learning

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


## Data

1. Data source
    1. User generated - user data
    2. Sys generated - internal data
    3. 3party data, collect from all, mention GDPR
2. Data formats
    1. Row or column-major
        1. Row - fast writes
        2. Column - fast column reads
    2. Txt/binary
3. Data models
    1. Relational
    2. Nosql - not only sql
        1. Doc
        2. Graph
    3. Structured/UnStructured 
4. Data storage engines and processing
5. ETL /ELT

## Sampling

Sampling - sampling from all possible real-world data to create training data
1. Non-Probability Sampling - selective bias - OK for init
    1. Convenience - what is available 
    2. Snowball - Example: collect friend of friends on social network.
    3. Judgement - expert decision.
    4. Quota - Example: 30 from each age group
2. Probability Sampling
   1. Simple random sampling
   2. Stratified
   3. Weighted 
3. Importance sampling (RL)
4. Reservoir sampling - sampling from stream.
    1. K samples in res, N - len of current stream
    2. For new elem generate random (1, n), if <k: replace in k
    3. Correct probability for each sample in reservoir.



## Labeling

1. Label types
    1. Hand label
       1. Measure of consent - flies kappa
       2. Data lineage - preserve source of data, if its labelling worsen model
    2. Natural label e.g. - like/dislike, click etc
2. Handling the lack of hand labels
    1. Weak supervision - define labeling function(e.g. regex) and label data. Pros: cheap, Cons: noisy
    2. Semi-supervision - train model on small subset then label all data with the model. 
    3. Transfer learning. Train on one task/domain. Change final layer -> train on final subset of data.
    4. Active learning/query learning - decide what samples to label next. 
3. Class imbalance
    1. Important to use the right metric based on error cost.
    2. Tehniques
       1. Data-level - resample
           1. Under/oversample
           2. SMOTE - oversample with new points between existing 
       2. Algorithm-level
           1. Balanced loss
           2. Focal loss - hard examples > weight CE*(1-pt)^hamma


## Evaluation

1. Offline evaluation
   1. How to split the data?
      1. classic K-FOLD (tr+val) + ts
      2. If time sensitive data. Data sorted by time:
         1. split by time  
         2. splitting by time + margin 
         3. preq splitting 
      3. If user/product sensitive data.
         1. split by user/product to prevent data leakage
      4. If cold start problem.
         1. drop some data from history
         2. make some users with empty or min history 
   2. Specifity/Sparsity trade-off
   3. Choose metric 
      1. Interpretible. You can say what exatly metric is showing.
      2. Sensitive to task. By metric we can say in the context of task what model is better.
   4. Calibration on test.
   5. Slicing. Slice by some features to find model failures.
2. Online evaluation. 
3. Baseline evaluation. 
    1. Random label
    2. Majority label 
    3. Simple heuristic (if contains swear word -> toxic message)
    4. Human label
    5. Existing solution
4. Evaluation methods 
    1. Perturbation test - Check the model on noisy samples.
    2. Invariance (Fairness)
    3. Directional expectations - sanity check
    4. Model calibration. Outputs from ML models are not necessarily probabilities. If you need the proba -> calibrate the model.
    5. Confidence measure
    6. Slice based metrics. How find slice?
        1. Heuristic
        2. Error analysis
        3. Slice finder algos. Generate with beam search than check


## Feature engineering

1. Handling missing values
2. Scaling - e.g. log for skew
3. Discretization
4. Encoding. Hashing trick
5. Feature crossing, e.g. recsys to add nonlinear
6. Pos embeddings, e.g. pos enc in bert

## Data Augmentation

1. Simple Label-preserving - synonyms 
2. Perturbation/Adverserial - add hard samples - noise
3. Data synth - add new samples (use diff model)


## Online learning 

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


