Author:
Vala Rahmani
7/12/2019

## Webscraping and Classification 

### Description

In this project we will be using classification model to compare contents of each subreddit with Natural Language Processing and classify the contents to one of the classes of Vegan or Paleo. The results from this model is used by the advertising agency working for Beyond Meat, a plant based meat company, to identify the phrases and the words that people are talking about in the social Media. The final model includes Logistic Regression and will be optimized with GridSearch

### About the API
**Why Reddit?**
Reddit is the Front page of the internet, and allows anyone to express their opinion freely. This creates an honest platform where we can gather data that can be trusted. Also, reddit developers have made the platform organized which can be used to scraped in a neat wat.

**How data has been collected?**
For the data collection in this project, a webscraper has been designed that uses the json of each subreddit and extracts the content of each post. This process is in done repeatedly to collect 1000 posts for each subreddit. However, because the Paleo subreddit was not as active the vegan subreddit an unbalanced ratio of 64% to 36% was collected for vegan and paleo respectively.
After the data has been collected all of the duplicate titles are deleted to make sure we have unique content for all of the data.


### Organization 

**Notebook Organization**
- The project has been seperated in 5 different notebooks 2 of which are the web scraping for each of the subreddits. Reddit_EDA is used for the cleaning, formatting and finding patterns that may be impactful to the final model. 
- The Reddit_modeling is used for the NLTK modeling to compare all different types of classification models.
- The Reddit_visualization notebook is dedicated for the visualization of the most impactful and frequent words of the dataset.


### Exploratory Data Analysis

This project is aimed to predict whether or not each post belongs to the vegan subreddit or the paleo subreddit. However, to add up the challenge and making the model generalize better, we only use the title and the selftext column, and remove all of the obvious words that can leak data into our model.
1. The first step in EDA is to drop the values that are duplicate based on the titles. A lot of the posts have been reposted repeatedly under the same subreddit. Therefore, those rows are removed to only keep the unique values thru out the dataframe.
2. Then, twp columns of `title` and `selftext` are added up together and the `title_selftext` column is created. Using this method is important because will give the most complete content. Also, selftext column has a 1100 missing values and cannot be used by itself even though with the rows that have content have instrumental effect on the predicting power of the model.
3. The most important part of the EDA process is where the `title` and the `title_selftext` columns get cleaned up.
    - Reddit allows users to post HTML code on their posts however, this introduces noise to the model. Therefore, the html codes 
      are removed from the posts and their text portions are extracted
    - The non text part of the posts have been removed.
    - Since python is a case sensitive language all of the letters have been lowercased. 
    - Then all the words have been tokenized based on the space between them
    - Stop words are removed and the common words that have been identified to have the unproportional correlation with the
      subreddits are also removed to prevent data leakage to our data.
    - Now data is ready to be used for the visualization and modeling.


### The Data Science process

- In order to build a comprehensive model, sentimental analysis, alongside Countvectorizing and TfidfVectorizer are used to better capture the information each word and phrases have in the model.
- In order to choose which model is best fit for our data, couple of different classification models as the following are fitted to the training countvectorized data.  
    1.KNeighbors
    2.DecisionTree
    3.RandomForest
    4.Logistic Regression
    5.AdaBoost
    6.Multinomial
    


| Model | Cross_val_score|
|------|------|
|DecisionTree|0.717|
|RandomForest|0.767|
|AdaBoost |0.787|
|KNeighbors |0.724|
|LogisticRegression|0.792|
|Multinomial|0.790|

Among all of the above models Logistic Regression had the best performance with the cross_val_score of 0.79.

Therefore, moving to the optimization of the model a pipelin is used to make use of the sentimental analysis part of the content.
The pipeline create will first go thru Countvectorizing the content and then add the sentiment scores to the data frame and finally fit everything into the logistic regression. Even though the Multinomial gives one of the best scores, it is not used as part of the final model since it does not accept negative numbers therefore, the vader scores cannot be implemented.

**As a result of all of the above models the best model is achieved by CountVectorizing and adding the Vader scores optimized on the Logistic Regression**

#### *Best Model*

|  Model  | Training Score | Test Score | F1_Score |
|---------|----------------|------------|----------|
|LogisticRegression| 0.9947 | 0.7986 | 0.8499 |

Eventhough the model is overfit, still has a decent predict power and has increased about 18% compared to the baseline model. Better scores could be achieved for this model. However, in order to create a model that better generalizes we decided to drop the common stop words from the model therefore, it has been challenging for the model decide which post belongs to which subreddit.

### Visualization
Visualization in Natural Language Processing can be tricky. However, in this project we decided to build 2 different types of visualization. The first one are the bar charts were by changing the positive class we have determined which keywords have higher coefficient with each class and then used a CountVectorizer to count the frequency of the words in the dataset. Obviously this is done by removing the stop words and the words that are obviously related to either one of the subreddits. 
- Word Cloud 
- Bar charts 

### Recommended keywords to be used in the advertisements.
**Most common words were:**

*meat, recipe, chicken, new, burger, good, breakfast, best, people, meal, free, help, cheese, make, diet*

**Highly correlated words with the vegan subreddit are as follows:**

*share, restaurant, meat, good, people, july, life, animal, tofu*

**Highly correlated words with the paleo subreddit are as follows:**

*years, best, like, th, meat, tofu, animal, people, veganism*


