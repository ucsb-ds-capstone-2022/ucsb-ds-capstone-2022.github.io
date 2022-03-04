# End of Winter Quarter Progress #

## Recap 

We are working with AppFolio, a tech company based in Santa Barbara that provides innovative software, services, and data analytics to the real estate industry. More specifically, we are working with their most popular and successful platform, their Property Management Service.

## About the Project

Our project aims to create a recommendation system that recommends properties to users who interact with the company’s website. Currently, we are working with AppFolio’s Computer Science Capstone team to implement our recommender model into their application that allows users to search rental properties in Santa Barbara county and quickly find information about those properties. As users navigate through the website, they are able to “like” certain properties. These user-item interactions are what we use to recommend properties that users might be interested in.

## Hyperparameter Tuning
For this project, we are implementing Grid Search and Random Search to help us determine the best hyperparameters to use in our model based on the training data. First, we hyperparameter tuned the data to include only Santa Barbara properties, which consists of only about 9500 observations. Because of this smaller dataset, we opted to use Grid Search since it searches through every possible combination of hyperparameters passed through the function into the model. When we move up to a larger dataset, and need to account for a larger region like all of Southern California or possibly the entire country, we would need to use Random Search to obtain a random sample of the possible combinations. This is purely because of the larger dataset and the lack of powerful computational power to iterate through all the possible combinations. 

For now, since we focused on preparing a better model for Appfolio’s CS Capstone which only contains Santa Barbara Counties, tuning on just the Santa Barbara Counties was our best bet. 

The hyperparameters we tuned are k = LATENT_DIM, encoder_structure = ENCODER_DIMS, act_fn = ACT_FUNC, likelihood = LIKELIHOOD, n_epochs = NUM_EPOCHS, batch_size = BATCH_SIZE, learning_rate = LEARNING_RATE and beta_kl = BETA. The metric we want to look at is Mean Average Precision. For each metric comparison, we tune with and without Constraint Adaptive Priors, i.e. with and without longitude and latitude features. 

#### Testing Results for non cap





#### Testing Results for cap





Above are the results from running the experiment and applying the best models to the testing data. Since we are looking at comparing the models using the Mean Average Precision, we see that metric increasing slightly when we add features using Constraint Adaptive Priors. 

## Data Cleaning
A major component to the BiVaE model is incorporation of item features. While the low dimensionality and completeness of our user dataset posed few obstacles, the higher dimensionality and various null observations in the items dataset is something that we continue to work around. Further, some of the data in our features is nested within JSON keys that include additional data, which is something we’ve worked to extract into separate columns. However, these extra features tend to have a high rate of null values, and part of our experimentation is based on how to incorporate or not incorporate the data we have.

We have not implemented the multi-modality component that encodes items into our model yet, but this is a problem that we foresee for next quarter. We anticipate that we will have to experiment with multiple options, whether it be including only subsets of the features or imputing the data we don’t have records for, to yield our most desirable results. 


## Metrics and Baseline Models

Metrics: Several approaches for evaluating model performance are demonstrated along with their respective metrics. Rating metrics, ranking metrics, classification metrics and non accuracy based metrics are some of the examples. In our projects we mostly focused on the ranking metrics. The use of the Ranking meetrics in the model is that they evaluate how relevant and effective recommendations are for the users. In general the more relevant an item is, the more likely we’ll want to predict it. Precision and recall are some of the most simple and easy to interpret metrics available to us. For our purposes, we are dealing with an imbalanced classification problem and will define these metrics under this consideration.


We use precision to measure the proportion of recommended items that are relevant. And we used recall to measure the proportion of relevant items that are recommended. Ametrices that we looked at is normalized discounted cumulative gain (NDCG) which evaluates how well the predicted items for a user are ranked based on relevance. Also we looked at mean average precision (MAP) which means it average precision for each user normalized over all users			


Baseline models: We used a baseline model for comparison. In general we wanted to see if our model is actually performing well. So we just compare the accuracy of our model with the accuracy of the baseline to see if we improved upon it. The baseline models that we used in our projects are variational autoencoder for collaborative filtering, poisson factorization vs BPR,  generalized matrix factorization (GMF), and neural matrix factorization (NeuMF)/neural collaborative filtering (NCF).
















## Future Goals
After meeting our goal for the end of this quarter, which was  implementing a simple recommendation model in the CS capstone, we look forward to improving our current model by incorporating multimodality. By incorporating multimodality, we would be looking at different features to use like latitude, longitude, property type, prices and images. In addition, we would still take time to test other models if we can achieve a better performance than the current model on the website. 

Although the CS Capstone website is showing only properties in Santa Barbara County, we would like to see if we can build a model that can be applied to a larger geographic area, like all of California or the entire country. 

![Alt Text](https://media.discordapp.net/attachments/927717200247275561/949101457871892511/1_0_GIF_2.gif)
