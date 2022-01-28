# Mobile Ad Click Prediction
Predicting whether a mobile ad will be clicked or not in the age of information overflow, fast changing trends and short spanned attention.

## Detailed Blog explaining the whole case study is here:
* part-1: https://medium.com/@rohanvailalathoma/mobile-ad-click-prediction-machine-learning-models-behind-the-billion-dollar-ad-industry-part-1-3971c60da991
* part-2: https://medium.com/@rohanvailalathoma/mobile-ad-click-prediction-machine-learning-models-behind-the-billion-dollar-ad-industry-part-2-325307d2e65a

## What are mobile ads and why we need to care about them ?
A mobile ad is a type of ad that can appear on web pages and apps that are viewed on a mobile device like a cell phone or tablet. For Google Ads, “mobile ad” is defined as where the ad can appear on “mobile” devices. These include high-end mobile devices with smaller screens, such as smartphones. A decade ago, when smartphones were almost a cutting-edge idea, nobody would have expected that in just a few years, these devices will become indispensable to us. Almost half of the online traffic comes from mobile devices. The mobile digital advertising set to grow from $162.6 in 2018 to $384.9 billion, worldwide, by 2023 according to eMarketer research. 
> <b><i>So, we can say that mobile is the future of digital advertising. Therefore, any marketing or advertising campaign should also focus on them. </i></b>

## Business Objectives and constraints
<b> Objective </b> :  Predict the pClick (probability of click) for a given ad as accurately as possible. <br>

<b> What is CTR (Click Through Rate) ? </b>
  * Clickthrough rate (CTR) is a ratio showing how often people who see your ad end up clicking it. Clickthrough rate (CTR) can be used to gauge how well your keywords and ads are performing. CTR is the number of clicks that your ad receives divided by the
number of times your ad is shown: clicks ÷ impressions = CTR. For example, if you had 5 clicks and 100 impressions, then your CTR would be 5%. A high CTR is a good indication that users find your ads helpful and relevant.

<b> The applications of having CTR are numerous: </b>
* Performance estimation for pricing<br>
* Optimization of the expected gain for real-time bidding.<br>
* Variance reduction for A/B testing: when comparing two publishing policies, using powerful predictive models allows to get rid of the context as much as possible, resulting in higher test significance.<br>

<b>Constraints :</b> Low latency, Interpretability.<br>
* We only have seconds to grab a viewer’s attention and on mobile devices people tend to browse quickly, and skip things of no interest. So it is very important that we need to get the ads that are having highest probabililty of clicking and then display them in the ranking order in the actual bussiness application. So latency is extremely important.
* We need to understand what kind of features make an ad more clickable so that we can get a better insight about how to maximize the ad revenue which needs the model to be interpretable as well.

## Data Overview 
* Source of the data : https://www.kaggle.com/c/avazu-ctr-prediction/overview/description  <br>

The problem we are working on is from the Kaggle competition which was conducted by Avazu which is a leading multi-national corporation in the digital advertising space. We have been provided with 10 days’ worth of Avazu data to build and test prediction models. Here all the given features are categorical in nature and they are anonymized by hashing to protect the privacy of the users.
Here datapoints in the dataset are arranged according to the time stamps but the accurate time stamp information relative to minutes and seconds has been removed and only hourly time stamps are given and this is done to prevent data leakage from accurate sequencing of events.
The dataset consists of 24 columns out of which we have 22 features and the 2 columns are ad-id and clicked or not information. The information about the columns is described below.

The data is given in the form of a CSV file
<table style="width:100%">
  <caption style="text-align:center;">train.csv <br> <br></caption>
  <tr>
    <th>Feature</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>ID</td>
    <td>The unique id for each Ad</td>
    </tr>
  <tr>
    <td>Click</td>
    <td>Whether the Ad is clicked or not (yes/no as 1/0)</td>
  </tr>
  <tr>
    <td>Hour</td>
    <td>date and hour information of when the ad is displayed</td>
  </tr>
  <tr>
    <td> </td>
    <th>user based features</th>
  </tr>
  <tr>
    <td>Device ID</td>
    <td> unique device identifier which can be treated as user id </td>
  </tr>
  <tr>
    <td>Device IP</td>
    <td>Ip address of the device at the time when the ad is shown can also be proxied for user id</td>
  </tr>
  <tr>
    <td>Device Model</td>
    <td>Model of the device like samsung or iphone etc</td>
  </tr>
  <tr>
  <tr>
    <td>Connection type</td>
    <td> Type of connectiion the mobile device is connected to like wifi,3G or 4G etc </td>
  </tr>
  <tr>
    <td>Device type</td>
    <td> Type of the device whether it is smart phone or tablet etc </td>
  </tr>
  <tr>
    <td> </td>
    <th> Context based features </th>
  </tr>
  <tr>
    <td> Site id </td>
    <td> Unique id of the website visited </td>
  </tr>
  <tr>
    <td>App id </td>
    <td> Unique id of the app used by the user </td>
  </tr>
  <tr>
    <td>Site Category</td>
    <td> Category of the Site visited</td>
  </tr>
  <tr>
    <td> App Category </td>
    <td> Category of the app used </td>
  </tr>
  <tr>
    <td> Site domain </td>
    <td> Domain of the site used </td>
  </tr>
  <tr>
    <td> App domain </td>
    <td> Domain of the app used </td>
  </tr>
  <tr>
    <td> Banner Position </td>
    <td> Position of the Ad on the mobile screen </td>
  </tr>
  <tr>
    <td> </td>
    <th> Anonymized Categorical variables </th>
  </tr>
  <tr>
    <td> C1,C14 to C21 </td>
    <td>  Anonymized Categorical variables </td>
  </tr>
</table>

## Mapping the business problem to a machine learning problem
### Type of machine learning problem <br>
* It is a <b> binary classification </b> problem where given an Ad and its related information, we need to predict whether it would be clicked or not, for that we will predict the probability of click and set a suitable threshold which works best with our data and then if the probability is above the threshold, then we will predict that it would be clicked or vice versa.

### Performance metric
* <b><i>Binary Log-loss</i></b> : The metric to measure the performance of the model is chosen to be log-loss because it penalizes the model for every wrong prediction and also it uses the probability values directly to estimate the value.
* <b><i>Confusion matrix along with precision and recall matrices </i></b>:  With the help of these matrics, we get an idea of how the model is performing for classes 1 and 0 since the ad click prediction data is always an imbalanced data.

### Machine Learning Objectives and constraints
 * <b><i>Objective</i></b> : Reduce the log-loss as low as possible 
 * <b><i>Constraints</i></b> : As low latency is the main priority , the models should be chosen in such a manner that they are light weight and also powerful so that they are fast during the deployment and give accurate results.

## Notebooks Overview
### <a href="https://github.com/Rohan-Thoma/Mobile_Ad_Click_Prediction/blob/main/Mobile_Ad_Pred_EDA.ipynb"> Mobile_Ad_Pred_EDA.ipynb </a> :
* This notebook contains the exploratory data analysis of the given data and detailed insights about the given features is being described in the notebook.
* The features given are all categorical in nature and the ditruibution of the numbers among the features are highly skewed which means that some feature values are very large in number and some feature values are very few in number.
* One interesting observation that was made during the analysis is that the feature values with large number of impressions and clicks do not necessarily result in high Click Through Rate values , hence showing more ads does not increase the percentage of ads clicked among the total number of ads shown to the user.
* The analysis is done on each feature to judge its usefulness by computing the CTR per categorical value of the feature , so that we could identify if there is any trend in the CTR value which will help us identify exactly what is the reason behind the ad being clicked more or less and at what value of the feature this is happening.
* Based on the analysis , 6 new features are being engineered which are very useful for classification and they are listed as follows: <br>
1. <b><i> device_id_counts </i></b>- We have seen that counts of the impressions per device id shows a nice variation of per category CTR so we have added that as a feature.
2. <b><i> device_ip_counts </i></b> - We have seen that counts of the impressions per device ip shows a nice variation of per category CTR so we have added that as a feature.
3. <b><i> hour_of_the_day </i></b> - We have seen that CTR shows a nice trend as the hour of the day changes throughout the day so we have added that as a feature.
4. <b><i> day_of_the_week </i></b> - We have seen that CTR changes wrt the day of the week during the EDA so we have added that as a feature.
5. <b><i> hourly_user_count </i></b> - We have seen that number of users are increasing depending on the hour of the day and peak at the noon and when we compute the CTR per count of the users per hour we got a nice trend ,so we have added that as a feature.
6. <b><i> GBDT features </i></b> - We have trained a Gradient Boosted Decision Trees Model on the train data which is encoded with the help of CTR values itself and taken the leaf node number where the data point falls in to while prediction and taken that as the feature , as we have 100 trees as base learners , so we will have 100 latent features for each data point.

### <a href="https://github.com/Rohan-Thoma/Mobile_Ad_Click_Prediction/blob/main/Mobile_Ad_Pred_modelling.ipynb"> Mobile_Ad_Pred_modelling.ipynb </a> :
* This notebook consists of the results and insights after trying out various machine learning models
* As we have huge dataset of 50 million data points, 5 million datapoints are picked by selecting the data points in a uniform ramdom fashion spanning the entire dataset. This is done due to the resource contraints that so not support the processing of all the 50 million data points.
* As all the features are categorical in nature, so it makes sense to label encode them , so as we have already computed the CTR per categorical value for each feature, those values can be taken as a better proxy instead of label encoding them manually and with this method we get only 1 numerical feature column corresponding to each feature which also keeps the dimensionality from increasing if we would opt for traditional encoding schemes such as one-hot encoding especially when the features are having very high number of unique categorical values such as this dataset.
> Here the test log-loss of <b><i> 0.39065</i></b> is obtained which is a very good loss which is equivalent to score of the <b><i>160 to 170th</i></b> position in the public leaderboard of the kaggle.
* The actual dataset given by the kaggle is of 50 million datapoints but we have taken a 5 million data points and trained our model due to the resource constraints and the test data given the kaggle itself is close to 5 million data points , so we cannot predict on this dataset using the current model because there will be a lot of unseen data points for which will not be able to predict correctly.
* By using this current model on the kaggle test data, it has generated a score of log-loss = 0.41 which is not bad for a model that is trained on just 5 million data points when the actual dataset is very large , this means that we have trained on the 10% of the given data.
* If we could train the model on whole 50 million data points , then the score would be much better but keeping in mind the real world application of the solution , the reduction in log-loss would contribute very little value in the business standpoint as we have not been able to see any considerable improvements in the values of the confusion matrix.
* So we can conclude that by engineering 6 different types of new features , we are able to get one of the least log-losses 0.39065 on the test data even though we are not able to get significant improvement in the confusion matrix values.
 
### Scores from the various machine learning models are given below:
<table style="width:100%">
  <tr>
    <th>Model</th>
    <th>Features</th>
   <th>Test Log-loss</th>
  </tr>
  <tr>
    <td>Random model</td>
    <td>original features + 5 engineered features</td>
   <td>0.88575</td>
    </tr>
  <tr>
    <td>Logistic regression with hyper-parameter tuning</td>
    <td>original features + 5 engineered features</td>
   <td>0.40037</td>
  </tr>
  <tr>
    <td>XG_Boost with hyper-parameter tuning</td>
    <td>original features + 5 engineered features</td>
   <td>0.39457</td>
  </tr>
  <tr>
    <td>Field Oriented Factorization Machines with hyper-parameter tuning</td>
    <td>original features + 5 engineered features</td>
   <td>0.40614</td>
  </tr>
 <tr>
    <td>Logistic regression with hyper-parameter tuning</td>
    <td>original features + 5 engineered features + GBDT features</td>
   <td>0.39660</td>
  </tr>
  <tr>
    <td>XG_Boost with hyper-parameter tuning of 2 parameters</td>
    <td>original features + 5 engineered features + GBDT features</td>
   <td>0.39366</td>
  </tr>
 <tr>
    <td>XG_Boost with hyper-parameter tuning of 4 parameters</td>
    <td>original features + 5 engineered features + GBDT features</td>
   <td>0.39065</td>
  </tr>
    
