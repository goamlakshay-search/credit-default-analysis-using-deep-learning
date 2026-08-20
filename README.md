# credit-default-analysis-using-deep-learning
# data set used: UCI Default of credit card clients dataset (30,000 Taiwanese)
abstract: This project presents a machine learning approach to credit analysis using the provided credit ecotrix dataset. The objective was to develop a model capable of predicting credit default. The methodology involved several key steps: data loading and initial exploration, robust preprocessing including one-hot encoding for categorical features and standardization for numerical features, and splitting the data into training and testing sets. A deep learning model, specifically a Keras sequential model with a hidden layer and sigmoid activation for binary classification, was constructed. The model was trained over 300 epochs with binary_crossentropy loss and the adam optimizer. Evaluation metrics, including training and validation loss and accuracy, demonstrated the model's strong performance, converging to very low loss and high accuracy (approximately 99.85%) on both seen and unseen data. The results indicate that the developed model is highly effective in identifying credit risk, offering a robust tool for credit assessment.

# extended version
well this model provides great output but still we uses other methods like tree based XGBoost to make it more intutive and further used SHAP model explanatory framework to better understand the underlying setup that we are dealing with
# using XGBoost to this dataset
using XGBoost our model is not performing well and The model achieves 81.5% accuracy, but this is misleading due to class imbalance. It performs very well on non‑defaults (precision 0.84, recall 0.94), but poorly on defaults (precision 0.63, recall 0.37). Since defaults are critical in risk management, I’d rebalance the dataset, adjust class weights, or tune thresholds to improve recall for defaults while maintaining overall performance, and even after adjusting the threshold and rebalancing dataset we had a further improvement in the recall for the defaulter class (class 1), often with a corresponding change in precision and potentially overall accuracy. The main problems are class imbalance, poor recall for defaults, misleading reliance on accuracy, and limited gains from threshold tuning. The model needs rebalancing, better feature engineering, and alternative evaluation metrics to be truly effective.
provided that previous alterations made to this models were inefficient and lacks predictability of determining defaulters, so to analyze dive deeper I introduced SHAP To understand how each feature contributes to the model's predictions, we will use SHAP (SHapley Additive exPlanations). SHAP values help us interpret the output of machine learning models by explaining the prediction of an instance by computing the contribution of each feature to the prediction.

# extended XGBoost version(featuring)
after analyzing SHAP we
Feature Engineering: Aggregating Bill/Payment Series and Creating Ratios
To capture more comprehensive information from the monthly bill and payment statements, we will create new features by:

Aggregating BILL_AMT and PAY_AMT: Calculating the sum and mean of these amounts over the six-month period.
Creating Utilization Ratios: Dividing aggregated bill amounts by the LIMIT_BAL to understand how much of the credit limit is being used.
Creating Payment Ratios: Dividing aggregated payment amounts by aggregated bill amounts to assess repayment behavior relative to outstanding bills.
Correlation Analysis and Feature Selection
Highly correlated features can introduce redundancy and multicollinearity, potentially affecting model stability and interpretability. We will compute the correlation matrix and remove one of the features from any pair that has a correlation coefficient above a certain threshold (e.g., 0.95).
Retraining XGBoost with New Features and Regularization
Now, with the new engineered features and after handling highly correlated columns, we will retrain the XGBoost model. We will re-use the scale_pos_weight for class imbalance and introduce additional regularization parameters to prevent overfitting.
1. Feature Engineering: Aggregating Bill/Payment Series and Creating Ratios
To capture more comprehensive information from the monthly bill and payment statements, we will create new features by:

2. Aggregating BILL_AMT and PAY_AMT: Calculating the sum and mean of these amounts over the six-month period.
Creating Utilization Ratios: Dividing aggregated bill amounts by the LIMIT_BAL to understand how much of the credit limit is being used.

3. Creating Payment Ratios: Dividing aggregated payment amounts by aggregated bill amounts to assess repayment behavior relative to outstanding bills.
# results 
results of this extended feature engineered xgboost model:
You have a good number of True Negatives (correctly predicted no default) and True Positives (correctly predicted default).
However, there's a noticeable number of False Negatives (actual defaults predicted as no default), confirming the model's struggle to identify all defaulting customers. This is consistent with the lower recall for the 'Default' class.
There are also False Positives (predicted default, but actual no default), aligning with the lower precision for the 'Default' class.
Overall Interpretation
Your model successfully identifies several key drivers of credit default, particularly recent payment statuses and credit limits. However, the performance metrics, especially precision and recall for the 'Default' class, indicate that there's still room for improvement in accurately identifying and predicting defaulting customers. The SHAP analysis supports the observation that the model might be heavily reliant on short-term payment features, but overall recall indicator for default category has increased from 37% to 60% but still indicates the model misses a significant portion (40%) of the customers who will actually default (false negatives
