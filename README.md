## Capstone: Credit Fraud-Detection 
_Supervised learning on a dataset of credit card transactions.  Develop a model to detect fraudulent transactions using contemporary methods. Learn about fraud behaviours and customer purchasing insights._

<img src='https://github.com/particleman14/Capstone---Fraud-Detection/blob/main/Files/Screens/nilson%20cc%20fraud%20chart.PNG' width="240" height="200"> <img src='https://github.com/particleman14/Capstone---Fraud-Detection/blob/main/Files/Screens/nilson%20chart.PNG' width="240" height="200">

__Introduction:__  Credit Card fraud has been a growing problem in the digital age.  In 2020, COVID-19 accelerated this trend and we are seeing record numbers of credit card transactions.  Along with these trends, fraudulent transactions have been increasing in size and sophistication.  The cost of fraud includes the gross amount but also the associated servicing, which takes time and resources.  Having an efficient model to help determine fraud can greatly improve efficiency and customer satisfaction.  Companies can also use these models to gain insight into fraudulent behavior to further protect themselves and customers.  Typically, companies already have some combination of systems or models in place to detect fraud, but they should consider updating to contemporary models that have greater performance (especially in the last couple years).  Let’s try to build a model using basic transaction data.


__Capstone EDA:__  Take the existing dataset and explore it's features.  Try to figure out what the differences are between fraud and legitimate transactions. 
  This dataset contains 786,358 transactions with 12,417 transactions marked as fraudulent.The dataset contains very few null items and was relatively straightforward to clean and pre-process.  We explored distributions of different features and explored some correlations between fraud and non-fraud transactions.  The main feature about this dataset is the highly imbalanced nature of the fraud/no-fraud transactions (fraud cases % : 0.016).  We identified a few features to look out for and figured out which features would be relevant to building our model.      
 <img src='https://github.com/particleman14/Capstone---Fraud-Detection/blob/main/Files/Screens/original%20df%20columns.PNG' width="200" height="300"> <img src='https://github.com/particleman14/Capstone---Fraud-Detection/blob/main/Files/Screens/nilson%20cc%20fraud%20chart.PNG' width="200" height="200"> 
 
  
__Capstone Modeling:__  We compare a number of different models to see how effective they are.  We determined that the XGBoost model has the best overall performance.  This model is very popular for fraud and other highly imbalanced datasets.  Typically, resampling methods have been employed (SMOTE, undersampling, etc.) to rebalance the dataset and give more weight to the fraud rows.  However, XGBoost doesn't necessarily improve with normalized/standardized numeric data.  Instead, it's the features themselves that become most important.  Running this model, we get an AUC of around .74 even after tuning hyperparameters. 
The most important features were transactionAmount, currentBalance and availableMoney (the only 3 numeric features!)

A few articles on XGBoost... Over the last couple of years this model has become very popular for its performance.
https://www.sciencedirect.com/science/article/abs/pii/S0167865520302129
https://ijecce.org/administrator/components/com_jresearch/files/publications/IJECCE_4356_FINAL.pdf
https://ieeexplore.ieee.org/abstract/document/9214206


__Capstone Modeling-FeatureTools:__= The next step is to try and create features in order to improve the model.  Typically, you can use feature interactions, representations, or other techniques based on domain specific knowledge.  However, in this notebook we try to autogenerate a wide range of features using the FeatureTools module.  We are able to create relationships between various features of dataset and the module generates numerous new features.  
* Starting with 16 columns, we manage to create an additional 154 features from this tool.  After encoding the categoricals we end up with around 464 columns.  This is a very brute force approach but it did yield better results! (AUC .80)  
* This model also used entirely different features than the original XGBoost model (num_unique,count,mode).  We can compare the features created in this model to gain insight to where Fraud is occuring and see if we can further increase our predictive power.  


__Capstone Modeling-PyCaret:__  PyCaret is an end-to-end ML pocketknife type of module requiring little code and quick development.  We can setup various environments with differnet amounts of pre-processing,feature generation,model selection, blending, hyperparameter tuning, etc.   I found using this tool very handy, however my memory and computer were bogging down very hard.  Overall we achieved the best results from using this module.  
* First, we compared a range models for relative performance.  As expected, the Gradient Boosting Models were most effective (.80-.82 AUC).  
* Second, utilizing a blending feature in PyCaret, we combined the top 4 performing models and managed to create a blended model that achieved .84 AUC just slightly higher than the individual gradient boost models. However, the caveat with this model is we cannot view the feature importances so the results are a bit of a blackbox.  
* Next, we create the top 3 models individually (all gradient boosting!) and analyze the results and more importantly the features.  
* What's most interesting is that all 3 models used mostly differing features to comes up with their predictions.  The various features give us additional paths to explore and test for future optimization.  

__Takeaways:__ Overall the gradient boosting models have shown to be superior when it comes to binary classification of rare/infrequent events often with highly skewed distributions. We found that we can eke out a little bit of performance by blending these models together in PyCaret. The most effective way to improve our prediction power is by analyzing the features utilized by the models to help inform our next direction of analysis or inquiry. This in turn will help us create better features and further refine the model.  It's true what they say, model optimization is more often than not a form of feature engineering. Now we have a model that is fairly strong and ready to be deployed, while we also have a few new lines of inquiry(from our feature importances) to explore further improvements.


