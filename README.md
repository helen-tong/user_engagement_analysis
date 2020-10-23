# User Engagement Analysis

![ideas](images/mobile_likes.png)

## Table of Contents
- [Background](#Background)
- [Project Goal](#project-goal)
- [Data](#the-data)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Modeling](#modeling)
- [Natural Language Processing (NLP)](#natural-language-processing-nlp)
- [Hyperparameters and Feature Importance](#hyperparameters-and-feature-importance)
- [Conclusion](#conclusion)

---

## Background 

One of the funnest tasks as a Data Analyst is to dive deep into the data and extract a story, or any information we can learn from. When working with data related to a product, user engagement is one of the key metrics that tells us whether users find value in a product or service. I was given user login data for the month of October in 2019. Let's buckle up and go right into some data digging!

---

## Project Goal

 My goal is to understand user activities during their duration on the site, discover weekly or daily trends, what features drive the most traffic from our users, what features attract users to come back, how many repeated users, etc. Ultimately, I would like our analysis to generate actionable items that can help us drive user engagement. 

---

## The Data

I was given a dataset with 300 rows and 14 columns, with user activity information for the month of October 2019. User engagement metrics include projects added, likes given, comments given, inactive duration, bugs in session, and session duration. There was one null value in the "session likes given" column and I replaced it with 0. 
I also did some feature engineering and created an extra column to extract day of week from the login date column.

---

## Exploratory Data Analysis

### An overview of data

There were a total of 300 sessions that occurred in the month of October 2019, with **48 unique users, and 30 repeated users**. Out of 300 total sessions, 142 sessions had durations longer than average, and 158 sessions had durations shorter than the average duration. 
As seen below, there was a huge spike in user login on October 26th, which was a Saturday. I would be curious to explore the possible reasons for the spike, there could have been a new feature roll out, a promotion, a mention from a YouTuber, etc. The biggest dip of the month occurred on October 14th, which was the Columbus Day. It could be explained that people were out celebrating the long weekend and gave their brain a little rest. 

![total](images/daily_login.png)

### What day of the week generated the most traffic?

As you can see, users were most likely to log in on a Friday and least likely to log in on a Tuesday.

![dow](images/dow_login.png)    


### How did user activity vary between day of week?

Likes given by users constituted the majority of user activity, which makes sense as they consume the least effort and least time to do. As you can see, the amount of each user activity stay pretty consistently throughout the week. Comments given to projects by users are slightly higher in general on Fridays.  

![user_activity](images/activity_dow.png)

One interesting thing to point out here is, although Fridays generate the highest amount of traffic, **users tend to engage in longer activities on Tuesdays**. Below graph shows average user engagement by activity, and it shows that on average, users added more projects and left more comments on Tuesdays than any day of the week. Some of the reasonings I can think of are: 

- They were seeking inspiration to help with weekly or work/school projects
- Users tend to be more productive on Tuesdays
- Users had more time to conduct longer activities on Tuesdays

![average_activity](images/average_activity.png)


### How do features vary between successful campaigns and failed campaigns?

The median goal for failed campaigns are roughly $7,500 while successful campaigns have a median goal of $3,500. Unsurprisingly, lower goal tend to yield a successful campaign. As for campaign length, longer does not necessary attract more backers. The median campaign length for successful campaigns are around 30 days, versus failed campaigns have a slightly longer length. And lastly, majority of the campaigns that are staff picked end up to be successful campaigns. 

![median](images/median_graph.png)


### How do campaigns do in general by year?

Most campaigns launched between 2010 until the end of of 2013 were able to raised enough funds to meet their goal by deadline. That trend disappeared from 2014 to 2018, and then there was huge spike of successful campaigns in 2019. 

![yearly_summary](images/yearly_summary.png)


---

## Modeling

After doing one hot encoding to all categorical features, I was ready to split the data into training set and testing set, and train them with machine learning models. The three models that I did as my baseline models are logistic regression, random forest classifier, and gradient boost classifier. 

### Logistic Regression
- **Accuracy:** 0.627
- **Cross val score:** 0.623
- **Confusion Matrix:**
     | 21,848   | 2663  | 
    | :-------- | :------: | 
    | **13,206** | **4,681**| 


### Random Forest Classifier
- **Accuracy:** 0.715
- **Cross val score:** 0.714
- **Confusion Matrix:**
     | 18,001    | 6510   | 
    | :-------- | :------: | 
    | **5,529** | **12,358**| 

### Gradient Boost Classifier
- **Accuracy:** 0.742
- **Cross val score:** 0.738
- **Confusion Matrix:** 

    | 20,133    | 4,378   | 
    | :-------- | :------: | 
    | **6,573** | **11,314**| 


Because my data is balanced, the accuracy score would be a metric I want to use to evaluate how accurate my model is. The accuracy score is the ratio of number of correct predictions to the total number of input samples. In combination with the cross val score and the confusion matrix, I am confident to select gradient boost classifier as the best machine learning model I can use for this prediction. 

Let's evaluate it further by looking at the ROC curve. The ROC curve also shows that gradient boost classifier performs the best out of all three models.


![roc_plot](images/roc_plot.png)


## Natural Language Processing (NLP)

Sentiment analysis is a sub-field of natural language processing that tries to identify and extract opinions by gauging the attitude, sentiments, and emotions of a writer through texts. I have decided to use the VADER sentiment analysis from the nltk library in python to evaluate the blurb feature, which is the headline to a campaign, to see if I can improve my model using sentiment analysis. 

The VADER analyzer algorithem outputs sentiment scores to 4 classes of sentiments: negative, neutral, positive, and compound, which is the aggregated score of the other three sentiments. 

After adding the 4 classes of sentiment scores to my features, I fit my training data into my three models and find that adding sentiment analysis did not make an impact to my models. 

## Hyperparameters and Feature Importance

Next, I wanted to tune my hypyerparameters by running grid search. After putting my machine to work for 2 hours, grid search returned the most optimal hyperparameters for my gradient boost classifier model:

- **Learning rate:** 0.1
- **Max Depth:** 6
- **Min Sample Leaf:** 2
- **Max Features:** 1
- **N_estimators:** 500
- **random_state:** 1

### Feature Importance 

I also looked into feature importance to identify what are some of the most impactful features to the dependent variable. These did not come to a surprise as they reaffirm some of the analysis I did in my EDA.

![features](images/feature_importance.png)

## Conclusion

To conclude, here are some of the suggestions for a successful campaign on Kickstarter:
- **Goal:** Have a reasonable funding goal.
- **Staff Pick:** Staff pick campaigns get prime placement on the Kickstarter's website and they appear in Kickstarter's widely-distributed email. It would only make sense that being featured on "Projects We Love" by Kickstarter would help with meeting your project goal.
- **Launched Year:** If you launched your project between 2011 to Dec 2013, or 2019, chances are you would have a successful campaign than if you have launched your campaign between 2014 and 2018.
- **Campaign Length:** Keep your campaign length to 30 days. 

Using machine learning, I am able to predict, with **0.751** accurancy, whether a Kickstarter campaign would meet its funding goal within 60 days of its launch. And here is the most optimal model:

#### Gradient Boost Classifier:
- **Learning rate:** 0.1
- **Max Depth:** 6
- **Min Sample Leaf:** 2
- **Max Features:** 1
- **N_estimators:** 500
- **random_state:** 1


 












