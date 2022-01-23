# Mobile Ad Click Prediction
Detecting whether a mobile ad will be clicked or not in the age of information overflow, fast changing trends and short spanned attention.

## What are mobile ads and why we need to care about them ?
A mobile ad is a type of ad that can appear on web pages and apps that are viewed on a mobile device like a cell phone or tablet. For Google Ads, “mobile” is defined as where the ad can appear on “mobile” devices. These include high-end mobile devices with smaller screens, such as smartphones. A decade ago, when smartphones were almost a cutting-edge idea, nobody would have expected that in just a few years, these devices will become indispensable to us. Almost half of the online traffic comes from mobile devices. The mobile digital advertising set to grow from $162.6 in 2018 to $384.9 billion, worldwide, by 2023 according to eMarketer research. So, we can say that mobile is the future of digital advertising. Therefore, any marketing or advertising campaign should also focus on them. 

## Business Objectives and constraints
<b> Objective </b> :  Predict the pClick (probability of click) as accurately as possible. <br>

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
