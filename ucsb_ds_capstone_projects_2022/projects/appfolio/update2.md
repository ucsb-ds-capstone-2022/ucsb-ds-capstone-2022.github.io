# End of Winter Quarter Progress #

## Recap 

We are working with AppFolio, a tech company based in Santa Barbara that provides innovative software, services, and data analytics to the real estate industry. More specifically, we are working with their most popular and successful platform, their Property Management Service.

## About the Project

Our project aims to create a recommendation system that recommends properties to users who interact with the company’s website. Currently, we are working with AppFolio’s Computer Science Capstone team to implement our recommender model into their application that allows users to search rental properties in Santa Barbara county and quickly find information about those properties. As users navigate through the website, they are able to “like” certain properties. These user-item interactions are what we use to recommend properties that users might be interested in.

## Hyperparameter Tuning
For this project, we are implementing Grid Search and Random Search to help us determine the best hyperparameters to use in our model based on the training data. First, we hyperparameter tuned the data to include only Santa Barbara properties, which consists of only about 9500 observations. Because of this smaller dataset, we opted to use Grid Search since it searches through every possible combination of hyperparameters passed through the function into the model. When we move up to a larger dataset, and need to account for a larger region like all of Southern California or possibly the entire country, we would need to use Random Search to obtain a random sample of the possible combinations. This is purely because of the larger dataset and the lack of powerful computational power to iterate through all the possible combinations. 
For now, since we focused on preparing a better model for Appfolio’s CS Capstone which only contains Santa Barbara Counties, tuning on just the Santa Barbara Counties was our best bet. 
The hyperparameters we tuned are k = LATENT_DIM, encoder_structure = ENCODER_DIMS, act_fn = ACT_FUNC, likelihood = LIKELIHOOD, n_epochs = NUM_EPOCHS, batch_size = BATCH_SIZE, learning_rate = LEARNING_RATE and beta_kl = BETA. The metric we want to look at is Mean Average Precision. For each metric comparison, we tune with and without Constraint Adaptive Priors, i.e. with and without longitude and latitude features. 


