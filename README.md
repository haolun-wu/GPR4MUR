# GPR4DUR
⚙️ This is the implementation of our paper, ["**Density-based User Representation using Gaussian Process Regression for Multi-interest Personalized Retrieval**"](https://openreview.net/pdf?id=Px1hQM72iX), accepted to **NeurIPS 2024**. 

## Abstract
Accurate modeling of the diverse and dynamic interests of users remains a significant challenge in the design of personalized recommender systems. Existing user modeling methods, like single-point and multi-point representations, have limitations w.r.t. accuracy, diversity, and adaptability. To overcome these deficiencies, we introduce density-based user representations (DURs), a novel method that leverages Gaussian process regression (GPR) for effective multi-interest recommendation and retrieval. Our approach, GPR4DUR, exploits DURs to capture user interest variability without manual tuning, incorporates uncertainty-awareness, and scales well to large numbers of users. Experiments using real-world offline datasets confirm the adaptability and efficiency of GPR4DUR, while online experiments with simulated users demonstrate its ability to address the exploration-exploitation trade-off by effectively utilizing model uncertainty.

## Architecture:
```data```: save the data. Here we use MovieLens 1M as an example. The preprocessing for other datasets is the same.

```func```: 
 - **train_sur.py**: train a SUR or MUR model.
 - **test_sur.py**: evaluate a trained SUR or MUR model.
 - **tune_gpr.py**: train our GPR model.
 - **test_gpr.py**: evaluate trained GPR model.

```helper```: 
- **data_iterator.py**: extract the following information from the data and make it as an iterator, e.g., user_id, item_id, user_item_rating, cate_id, item_cate_rating, history_item, history_mask, history_rating.
 - **eval_new.py**: the implementation of the four metrics we are using for multi-interest retrieval: Interest Coverage (IC), Interest Relevance (IR), Exposure Deviation (ED), and Tail Exposure Improvement (TEI). 
 - **evaluation.py**: Traditional evaluation metrics, such as Recall and nDCG. We use the Recall for early stopping when training SUR/MUR.
 - **prediction.py**: prediction functions for SUR, MUR, and GPR
 - **preparation.py**: prepare data format.

```model```:
A list of baseline model implementations.

```preprocess```:
Preprocess the data and make the format suitable for our task (similar to the data preprocessing for sequential recommendation)


```synthetic```:
The implementation of the online simulation experiment, contains Data Preparation, GPR Fit and Prediction, User History and Observation Update, etc.

**run_exp.py**: The main running file.


## Run the code:
 - To train a SUR or MUR model:
```
blaze run run_exp.py -- --p=train-sur --dataset=ml1m --model_type=YDNN
```

 - To train our GPR model:
```
blaze run run_exp.py -- --p=tune-gpr --dataset=ml1m --model_type=YDNN --gpr_method=ucb --kernel_type=exp --o_noise=10.0 --gamma=1.0
```

 - To test a SUR or MUR model:
```
blaze run run_exp.py -- --p=test-sur --dataset=ml1m --model_type=YDNN
```

 - To test a GPR model:
```
blaze run run_exp.py -- --p=test-gpr --dataset=ml1m --model_type=YDNN --gpr_method=ucb --kernel_type=exp --o_noise=10.0 --gamma=1.0
```

