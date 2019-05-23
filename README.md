# Flatiron School Module 4 Project
---

## Data Science Immersive NYC - Jan 28th, 2019 Cohort

## Contributors - [Luke Borsare](https://github.com/eb9982) and [Derrick Lewis](https://github.com/lewi0332)

The requirements of this project are to use one of the following topics covered in module 4 to build a model.

- Time Series Analysis
- Recommendation Systems
- Clustering
- Deep Neural Networks
- Convolutional Neural Networks

We will test several models and feature engineering practices to determine the best application for this domain. 


# A Social Media Influencer Recommendation 

We connected with [Fohr](http:www.fohr.co), an influencer marketing platform, to assist this business with a real world problem.  Fohr connects retail fashion brands and social media influencers with the intention to develop an advertising relationship between them. Influential social media accounts use their channel to promote products to their followers and Fohr connects influencers to new brands. These relationships are becoming a core strategy of modern marketing efforts and creates a more engaged and credible endorsement for brands. However, while the resulting endorsement is more impactful, it is also smaller in reach and requires a lot of labor to discovering the right brand/influencer partnership. 

Fohr asked to create a more sophisticated recommendation system which could speed up the search process for brands looking for social media accounts to work with.  

To solve this problem we have data from each post of the social media accounts that participate with Fohr. This includes: 

|  Data  |  |  | |
| :-------------: | :-------------: | :-------------: | :-------------: |
|  Post Time | Post Followers | Post Likes | Post Interactions |
| Post Comments | Post Caption | Post Mentions | Post Hashtags |


Additionally, with are given data supplied by each user:

|  Categorical Data  |  |  | | |
| :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| Beauty  | Photography  | Illustration  |  DIY | Travel |
| Vlog  | Food | Artist | Musician/DJ | Fashion |
| Tech | Parenting | Entertainment | Fitness | Lifestyle |
| Health/Wellness | Automotive | Home Decor | Art/Design | Menswear |
| Models | News | Personal Style | User Birthday | User Gender |



**The problem: Given 'User X', find a specified quantity of similar users.**

While the business has filtering capabilities for the attributes of each user, we believe we can use Machine Learning techniques to create a more sophisticated correlation. We will combine set of features that will include a mathematical representation of each users caption data using Natural Language Processing. This will allow brands to find new social media influencers they may not have previously considered or may not have had the time to research caption topics.

Importantly, most brands are looking for users with a similar follower count as this generally determines the cost per post and matches marketing strategy: 

- small following/wide distribution = credibility
- large following/narrow distribution = awareness

We will address this issue by carefully constructing our model to be certain this feature is prominently represented. 

---
## 1. Data Import 

Fohr will share a .csv file with a small slice of post data.  The Data has been extracted from an Elasticsearch database and manually imported. The file arrived in a format that was far from ideal for our intention to use Python and Pandas for data manipulation. This required some significant manipulation even before we began preprocessing and early data analysis. 

**Data Manipulation**

Upon creating a Pandas DataFrame with the CSV file we saw that all data was concatenated into a single cell. This required some creativity in splitting into new columns. From there we performed the following preprocessing actions on the data features: 

- Convert categories into single columns with a "One Hot" label.
- Convert birthday into a single year value
- Convert Post time into the Day-of-week and Hour. 
- Clean text data to remove foreign characters and some punctuation from Captions, Mentions, and Hashtags

**Text Data**

Next we further cleaned our 3 text based columns and joined all text into a single grouped by each user id. We then saved this data frame to a new csv to be used in the next step of Natural Language Processing.

**Numerical Data** 

In our final data cleaning process, we joined all of our numerical based features in to single DataFrame. We then grouped each users data into a single row and aggregated their per post data by the mean. 

For detailed steps in our cleaning process and code snippets, please see: 

[Data Cleaning Detail](https://github.com/lewi0332/flatiron_mod_4_project/wiki/1.-Data-Cleaning-Process) 
-

---

## 2. Natural Language Processing 

The goal of using NLP in this project was to achieve some level of understanding of the semantic relationships between individual users and highly weighing these relationships so as to recommend users with similar semantic styles/contents. Ideally, the semantic similarity metric would see users fall into clusters of like users based on style/content. Thus, we generated three dimensional and two dimensional images, paying particular attention the spatial relationships between users, in order to evaluate the effectiveness of our methods of organizing users based on semantic similarity. The visualization process entailed dimensionality reduction via Latent Semantic Analysis(by way of Truncated SVD) for our Count and TF-IDF Vectors and Principal Component Analysis (PCA) for our Semantic Similarity Index. Ultimately we decided that a Semantic Similarity Index established more accurate and effective semantic relationships between users so this is the method we used for the final recommendation system.


[NLP Detail](https://github.com/lewi0332/flatiron_mod_4_project/wiki/2.-Natural-Language-Processing)
-

---

## 3. Cosine Similarity

In this step we combine both of our DataFrames and calculate a distances between users given all features combined. 

Initially a K-Means Clustering algorithm was used to model the data into a pre-determined number of clusters of like users. This method produced interesting results that may work for another application. Certain clusters were much more popular than others. This would produce vague results for many users and very specific results for others. 

We determined that a Cosine Similarity will allow for a mathematical distance between users and additionally add functionality in choosing the number of users to view in closest proximity. Additionally, this method allows for very interpretable results. 

- Scale data
- Mathematically adjust features to give them bias in our recommendations 
- Write a function to calculate distance from a particular user and return "x" number of similar users

A detailed explanation of the process in using Cosine Similarity can be seen here: 

[Cosine Similarity Detail](https://github.com/lewi0332/flatiron_mod_4_project/wiki/3.-Cosine-Similarity)
--

---

## Results

In this project we attempted many different methods to create a working recommendation. K-Means Clustering and Neural Networks for our Natural Language Processing. While the steps involved in the natural language processing are sophisticated, ultimately our recommendation is done best with a fairly simply calculation performed twice in succession. 

This recommendation can be run quickly with light processing power. 

We paired the Cosine Similarity distance of user 17 with the original data and looked at several features. We see that both follower counts and text similarity are very close for these top candidates. These are important features, but truly we need a human to verify similarity in style and tone of these users. 

However, we looked at the caption data between user 17 and user 245... both users primarily mention their pets in the captions! Clearly two users that are alike. 

![Results_data](https://github.com/lewi0332/flatiron_mod_4_project/blob/master/Images/results_1.png)

Here is another view of a user with less text similarity to the population. 

![Results_data](https://github.com/lewi0332/flatiron_mod_4_project/blob/master/Images/results_2.png)

---

## Next Steps

Going forward we would like to create a working tool that this business can use. To do this we would like to explore the process of streaming data from their Elasticsearch database into the script directly. This could allow for frequent updates to the data.  Secondly we would like to output the recommendation function to produce a .json file that could be interpreted by the business site to display results. 
