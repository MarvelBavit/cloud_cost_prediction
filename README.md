# cloud_cost_prediction
Prediction of AWS and GCP cloud usage using time series ML algorithms

# AWS Data Collection, EDA, Preprocessing, Modeling, Prediction
Problem statement
-----------------
To predict cloud cost for BUs across organisation. 


AWS Data Collection
-------------------
Manually downloaded DailySummary files from 's3://centralizedbilling' (CBSi Master Billing) for four months 
Manually uploaded them to 's3://c3-training-data/' (CBSi)
Unzipped & merged monthly data, for four months

EDA - Exploratory Data Analysis
---
Checked for seasonality, removal of null values & outliers

Data Preprocessing
------------------
Encoded Categorical values using OHE, Target Encoding

Read: https://docs.h2o.ai/h2o/latest-stable/h2o-docs/data-munging/target-encoding.html#:~:text=Target%20encoding%20is%20the%20process,convert%20categorical%20columns%20to%20numeric. - Target Encoding

https://machinelearningmastery.com/why-one-hot-encode-data-in-machine-learning/ - One Hot Encoding

Grouped data by UsageAccountID, now each UsageAccountID will have a dataset of their own. We are predicting for UsageAccountIDs, which can roll up to their respective BU's

Modeling
--------
Using LSTM, a variant of Recurrent Neural Networks to learn order dependence in sequence prediction.
Initially using 4 LSTM layers with dropout regularization, to avoid overfitting.

Read: https://medium.com/x8-the-ai-community/a-7-minute-introduction-to-lstm-5e1480e6f52a - LSTM

https://machinelearningmastery.com/dropout-for-regularizing-deep-neural-networks/ - Dropout

https://elitedatascience.com/overfitting-in-machine-learning - Overf
Using Adam Optimizer and Mean Squared Error as loss function

Read: https://deepdatascience.wordpress.com/2016/11/18/which-lstm-optimizer-to-use/ - Optimizer for LSTM


Model Training
--------------
Training and validation with 75% data and testing with 25% data, initially the model is trained with 50 epochs and a batch size of 500

Read: https://machinelearningmastery.com/difference-between-a-batch-and-an-epoch/#:~:text=The%20batch%20size%20is%20a%20number%20of%20samples%20processed%20before,samples%20in%20the%20training%20dataset. - Epochs and Batch size


# GCP Data Collection, Algorithm overview, Preprocessing, Modeling, Prediction

Problem statement
-----------------
To predict cloud cost for BUs across organisation. 

GCP Data Collection
-------------------
Collected data by querying BigQuery table directly from SageMaker notebook using access key (4 mos)

Algorithm overview
-----------------
Using Facebook's Prophet to generate prediction.
Since there are not many columns are statically significant to the cost column, decided to use Prophet, since it requires only DateTime and target columns

Read: https://towardsdatascience.com/predicting-the-future-with-facebook-s-prophet-bdfe11af10ff

Data Preprocessing
------------------
Format invoice.month column in the form '%Y%m'

Grouped data by 'project.id', now each 'project.id' will have a dataset of their own. We are predicting for 'project.id', which can roll up to their respective BU's

Modeling & Prediction Generation
--------------------------------
Prophet is a black-box model, feed 4 months data to model and generate prediction for 5th month

        model = Prophet()
        model.fit(min_master_df)


