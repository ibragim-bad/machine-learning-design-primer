# Machine learning design primer

Some helpful notes for Machine Learning System Design Interview preparation, which I gathered from various resources to prepare for machine learning systems design interview.


## Motivation
> Learn how to design Machine Learning systems and prepare for an interview.


## The main resources 

[Facebook Field Guide to Machine Learning](https://research.facebook.com/blog/2018/05/the-facebook-field-guide-to-machine-learning-video-series/)

[CS 329S: Machine Learning Systems Design, Stanford, Winter 2022](https://stanford-cs329s.github.io/)

[ML Systems Design Interview Guide](http://patrickhalina.com/posts/ml-systems-design-interview-guide/)

[ML System Design interview example](https://youtu.be/VPg2Uu1MYgI)

[Yandex MLSD interview guide](https://youtu.be/krDfPEYVVvk)

## Overview:

1. [Framework for solving MLSD cases](#framework-for-solving-mlsd-cases)
2. [Detailed notes on some concepts](#detailed-notes-on-some-concepts)
3. [Cases](#cases.md)

## Contributing

Feel free to submit pull requests to help:

+ Fix errors
+ Improve sections
+ Add new sections and cases

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
+ Start with a simple solution as a baseline

After listening to the conditions of the case, ask the interviewer clarifying questions. Repeat the main points to make sure you understand everything correctly. During the interview, ask the questions and state your assumptions. 

**Understand requirements:**
+ Users/samples number
+ Peak numbers of requests
+ Batch or online predictions
+ Edge or server computations


Examples of questions:
1. Could you give an example of the successful use case of this system?


## Problem definition. 

Define proxy machine learning metric for the business goal. 
   1. Define the business goal. 
   2. Define ML task type. Classification/regression/other
   3. Split the task into subtasks. Example: maximize users engagement while minimizing the spread of extreme views and misinformation. 

## Data 

1. [ Data source and data type](#data-source)
    + Where the data comes from? Is it in the same format or should we transform and join it?
    + One sample of data. What is X(features) and what is Y(labels)?
1. [Labeling](#Labeling)
    + Are the labels known? Is there Natural labelling? Should we label some data?
2. [Sampling](#sampling) 
3. Data recency and Distribution drift. 


## Evaluation

1. [Offline evaluation](#evaluation) 
   1. Data split
        + Random split or should split by date, users, products to prevent data leakage?
   2. Metric.
        + Choose a metric, that is interpretable and sensitive to the task. Think what errors will be most harmful, FP or FN for classification, over or underpredicting for regression.
   1. Baseline.
       + Mention baseline Non-ml-solution. You will compare your machine learning models with this baseline.
1. [Online evaluation](#online-evaluation)
   1. online-offline gap
   2. Online comparing.
      1. A/B randomised test
      2. A/A test.


## Features

1. What type of data do we have? Can we encode it?
2. [Feature representation, data preprocessing.](#feature-representation)
3. Data augmentation.


## Model

1. Pick the model.
2. Pros and cons of the model.
3. Architecture overview at a glance.
    + Linear model
    + GBDT
    + Embeddings + KNN
    + Neural networks
1. What loss function you will use?

## Further actions

1. [Deployment](#deployment)
2. Experiments
3. [Monitoring & Continual Learning](#monitoring-and-continual-learning)

# Detailed notes on some concepts

## Deployment

1. Batch prediction vs Online prediction
2. Model compression
   + Low-rank factorisation. Mobile net optimizations.
   + Pruning 
   + Knowledge distillation
   + Quantization
   + Special inference formats. (pb, onnx, torchscript)
3. Edge / Cloud computing


## Monitoring and Continual Learning

1. Monitoring -> Continual Learning
    1. Detect distribution shift -> adapt with CL
    2. Check if more frequent will boost
2. Eval
    1. Offline - sanity check
    2. Online
        + Canary - two models old and new, slowly route traffic
        + A/B - test
        + Interleaved experiments - preds from 2 models are mixed
        + Shadow test - log prediction from new, then study
3. Internal test on coworkers. But it is a sanity check


## Data source

1. Data source
    + User-generated - user data
    + Sys generated - internal data
    + 3party data, collect from all, mention GDPR
1. Data formats
    1. Row or column-major
        1. Row - fast writes
        2. Column - fast column reads
    2. Txt/binary
2. Data models
    1. Relational
    2. Nosql - not only SQL
        1. Document
        2. Graph
    3. Structured/UnStructured 
3. Data storage engines and processing
4. ETL /ELT

## Sampling

**Sampling - sampling from all possible real-world data to create training data**
1. Non-Probability Sampling - selective bias - OK for init
    + Convenience - what is available 
    + Snowball - Example: collect friends of friends on the social network.
    + Judgement - expert decision.
    + Quota - Example: 30 from each age group
2. Probability Sampling
    + Simple random sampling
    + Stratified
    + Weighted 
3. Importance sampling (RL)
4. Reservoir sampling - sampling from the stream.
    1. K samples in a reservoir, N - length of the current stream
    2. For new element generate i = random(1, n), if i<k: replace in reservoir
    3. Correct probability for each sample in the reservoir.

Training sample selection. For example positive, negative sampling in metric learning.


## Labeling

1. Label types
    1. Hand label
       1. Measure of consent - [Fleiss' kappa](https://en.wikipedia.org/wiki/Fleiss%27_kappa)
       2. Data lineage - preserve the source of data, if its labelling worsen model
    2. Natural label e.g. - like/dislike, click etc
2. Handling the lack of hand labels
    1. Weak supervision - define labelling function(e.g. regex) and label data. Pros: cheap, Cons: noisy
    2. Semi-supervision - train model on small subset then label all data with the model. 
    3. Transfer learning. Train on one task/domain. Change final layer -> train on the final subset of data.
    4. Active learning/query learning - decide what samples to label next. 
3. Class imbalance
    1. Important to use the right metric based on error cost.
    2. Techniques
       1. Data-level - resample
           1. Under/oversample
           2. SMOTE - oversample with new points between existing 
       2. Algorithm-level
           1. Balanced loss
           2. Focal loss - hard examples > weight CE*(1-pt)^hamma

## Feature Representation
1. Overall.
   1. Handling missing values
   2. Feature crossing 
2. Numeric values.
   + demeaning
   + scaling - e.g. log for skew
   + remove outliers
   + bining
   + quantization
3. Categorical values.
   + One-hot
   + Hashing trick
   + Embedding
4. Text.
   + Embedding. Bert-like-models.
   + Fasttext average. (fast)
5. Complex.
   + Concatenation. Example: Product title + category + other features
   + Encode -> Attention. Example: User history




## Offline evaluation

1. Offline evaluation
   1. How to split the data?
      1. classic K-FOLD (tr+val) + ts
      2. If time-sensitive data. Data sorted by time:
         1. split by time  
         2. splitting by time + margin 
         3. prequential validation
      3. If user/product sensitive data.
         1. split by user/product to prevent data leakage
      4. If cold start problem.
         1. drop some data from history
         2. make some users with empty or min history 
   2. Specificity/Sparsity trade-off
   3. Choose metric 
      1. Interpretable. You can say what exactly metric is showing.
      2. Sensitive to the task. By metric, we can say in the context of the task what model is better.
   4. Calibration on a test.
   5. Slicing. Slice by some features to find model failures.
2. Baseline evaluation. 
    1. Random label
    2. Majority label 
    3. Simple heuristic (if contains swear word -> toxic message)
    4. Human label
    5. Existing solution
3. Evaluation methods 
    1. Perturbation test - Check the model on noisy samples.
    2. Invariance (Fairness)
    3. Directional expectations - sanity check
    4. Model calibration. Outputs from ML models are not necessarily probabilities. If you need the probe -> calibrate the model.
    5. Confidence measure
    6. Slice based metrics.
        1. Heuristic
        2. Error analysis
        3. Slice finder algos (FreaAI). Generate with beam search, then check.


## Feature engineering

1. Handling missing values
2. Scaling - e.g. log for skew
3. Discretization
4. Encoding. Hashing trick
5. Feature crossing, e.g. in recsys to add nonlinearly
6. Pos embeddings, e.g. pos encoding in bert

## Data Augmentation

1. Simple Label-preserving - synonyms 
2. Perturbation/Adversarial - add hard samples - noise
3. Data synth - add new samples (use diff model)


## Online evaluation 

1. Online-offline gap
2. Online
    1. A/B randomised test
    2. Minimise time to first online
    3. Isolate eng bug from ml issues
        1. A/A test before adding real model w new system
    4. Real-world FL
        1. Backtest + forward test
    5. More metrics to check the correctness
    6. Triangulate causes, so iterate atomic steps
    7. Have a backup plan
    8. Calibration average prediction = average ground-truth prediction 
        1. != on the train - underfitting, test - overfit, online - on/offline gap
