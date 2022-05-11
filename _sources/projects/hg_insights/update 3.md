# Update 3

## HG INSIGHTS

Team Members: Atherv Gole, Cristian Razo, Eric Cha, Natasha Leodjaja, Qimin Tao <br>
Sponsor: Rob Fox <br>
Mentor: Leron Reznikov <br>

### Recent Work  
For the past few weeks, after completing the data cleaning and standardization process, we've focused on creating a user-score and generating recommendations. There were two main methods we used to generate recommendations:
    
1. Collaborative Filtering
2. Content Based

Collaborative filtering, made popular in part by the Netflix recommendation system, assumes that the data contains users, products, and respective user ratings for products. In our case however, we didn’t necessarily have a ‘rating’ or ‘score’ for each company’s product usage. To tackle this, we created a preliminary score metric that incorporated recency of product usage as well as the ‘weighted intensity’ (this is HG’s proprietary metric that provides a normalized value to indicate how much a product appears on a company’s web pages. In our case, we interpret it as a ‘confidence’ score that a product is actively in use). The formula for the score was simply:

Weighted intensity / recency penalty
We then normalized the scores by company and binned them from 1-5 in an effort to create a user-ratings matrix that closely matched popular collaborative filtering recommendation systems. Essentially, the most used product at a company should be rated a 5, and the rest of the products should be scaled 1-4 appropriately.

We then used the scikit-Surprise Python library to turn our user-ratings matrix into collaborative filtering recommendations. The models were validated by predicting scores for products in use and comparing them to the actual scores to compute RMSE.

### Another Preliminary Model
Our second model combines the use of cosine similarity and the predictive algorithms introduced in the section above. Cosine similarity is a measure of similarity between two sequences of numbers which in this case consists of the company information including the categorical features. 
	
This model first takes the top 10 products of a company with the highest aggregated scores which is essentially the group of products that are most likely to be in use at the time the data was recorded. It then utilizes the cosine similarity function to find 10 similar products for each of the 10 products with high aggregated scores. Repeating products were removed in order to create a set of unique products. Lastly, the model then uses collaborative filtering to predict the ratings for each of those products. 

![cosinesimilarity.png](attachment:cosinesimilarity.png)

### Successes
#### Preliminary accuracy of model and score creation

The Surprise collaborative filtering recommendations actually yielded promising results, with RMSE converging to ~0.91 after including 1 million rows. Other groups have also recommended a couple hybrid models for us to incorporate more company information into our recommendations, which will hopefully increase our accuracy rates. 

#### Collaborative filtering recommendation system without the Surprise validation
For the collaborative filtering recommendation system with recommending a set number of products per company, we got a decent precision score (about 0.9) using normalization + log.

### Challenges
#### Big data problem
Our first results after training our initial collaborative filtering recommendation system with the Surprise library actually had a much higher error than our most recent ones, initially we had trained the models on a 60000 row subset of the data, and k-fold cross validation resulted in an average RMSE of 1.7, meaning an average inaccuracy of around .34%. However, after increasing the size of the data subset, we noticed steadily decreasing RMSE. Below is a figure of the RMSE as we increased the number of rows in the training set:
![RMSE.png](attachment:RMSE.png)

We can see that the RMSE is nearing the minimum at just over 1.2 million rows. Unfortunately, as we increase the number of rows, the runtime of the training and the RAM cost for our machines increases greatly. Handling the size of the data given to us has been one of the most prevalent challenges we’ve dealt with, but fortunately it seems like we’ve approached the minimum error within the limits of our computing power.

Another challenge we face is the quality or the accuracy of the products that our recommendation system outputs. Our personal computers are not able to take in the data that were given to us from the sponsors which was 16GB. Therefore we’ve only been using a subset of the data ~60,000-100,000 rows from 11 million rows. Not only that, the data that were given to us is also only a sample or a subset of a big data. This jeopardizes the accuracy or the quality of the products we’re recommending.


### Future Plans:
1. Incorporating encoded attributes into our base recommendation model to increase the accuracy of product recommendation.
2. Work on validation for content based recommendation system; check the validity of the recommendations.
3. Incorporate a sequence-based recommendation system to include the time dimension of our data. 
4. Recommend different number of products for different companies based on the number of products they have purchased in the past

