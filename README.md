# Capstone---Fraud-Detection
Supervised learning on a dataset of card transactions.  Try to develop a model to detect fraudulent transactions. 



Capstone EDA = Take the existing dataset and explore it's features.  Try to figure out what the differences are between fraud and legitimate transactions. 
  This dataset contains very few null items and was relatively straightforward to clean and pre-process. The main feature about this dataset is the highly imbalanced nature of the fraud/no-fraud transactions (fraud cases % : 0.016).    
  
Capstone Modeling = We compare a number of different models to see how effective they are.  We arrive at the XGBoost model as its overall performance is best.  This model is very popular for fraud and other highly imbalanced datasets.  Typically resampling methods have been employed(SMOTE,undersampling, etc.) to rebalance the dataset and give more weight to the fraud rows.  However, XGboost doesn't necessarily improve with normalized/standardized numeric data.  Instead, it's the features themselves that become most important.  Running this model we get an AUC of around .74 even after tuning hyperparameters. The most important features being transactionAmount, currentBalance and availableMoney (the only 3 numeric features!)
https://www.sciencedirect.com/science/article/abs/pii/S0167865520302129
https://ieeexplore.ieee.org/abstract/document/9214206
https://ijecce.org/administrator/components/com_jresearch/files/publications/IJECCE_4356_FINAL.pdf


Capstone Modeling-FeatureTools = The next step is to try and create features in order to improve the model.  Typically you can use feature interactions, representations, or other techniques based on domain specific knowledge.  However, in this notebook we try to autogenerate a wide range of features using the FeatureTools module.  We are able to create relationships between various features of dataset and the module generates a bunch of features.  Starting with 16 columns, we manage to create an additional 154 features from this tool.  After encoding the categoricals we end up with around 464 columns.  This is a very brute force approach but it did yield better results! (AUC .80)  This model also used entirely different features than the original xgboost model.(num_unique,count,mode)  We can compare the features created in this model to gain insight to where Fraud is occuring and see if we can further increase our predictive power.  


Capstone Modeling-PyCaret = PyCaret is an end to end ML pocketknife type of module.  We can setup various environments with differnet amounts of pre-processing,feature generation,model selection, blending, hyperparameter tuning, etc.   I found that using this tool, the pre-processing and feature-pruning very handy and was able to compare all of the popular classifier models in a neat low-code interface.  
