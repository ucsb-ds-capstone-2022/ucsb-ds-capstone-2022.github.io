# Starting the Project #

## About the Project ##

The main purpose of this project is to create a recommender system that recommends properties to users based on previous user iteractions and property features. In particular, our focus is recommendations to new users, in addition to existing users. We will explore and compare various approaches of recommender systems and apply them to our data. Our final goal is to integrate our model into the [Santa Barbara property website](https://realm-ucsb.herokuapp.com/) created by the UC Santa Barbara's AppFolio sponsored computer science capstone team. 

This project showcases the power of machine learning on the user interaction data collected from AppFolio's property management software. AppFolio will be able to analyze the results from our recommender system to assess the effect of utilizing interaction data to better their products.

## About the Data ##

Our dataset is derived from [LISA](https://www.google.com/url?q=https://www.appfolio.com/news/appFolio-launches-new-ai-leasing-assistant-and-utility-management-offerings&sa=D&source=docs&ust=1644542468101327&usg=AOvVaw0I_O2jmUD38PsniHSAt-e_), Appfolio’s Award Winning AI Leasing Assistant Chat Bot, which is designed for property companies to streamline the process of receiving inquiries about properties from users. Our data is divided into three components: user ratings, item features, and images. It consists of over four million users, ten thousand properties, and five million user interactions. 

## What have we done so far ##

Currently working in teams to clean the data and test out different recommender system models. 

For the data cleaning, we’ve communicated with the sponsor about what the different attributes in the data represent and how it can add as a feature in our model. We’ve changed the current INTERACTION_TYPE, or ratings to numbers so they can be used in the Experimental Library we are using. In addition, will will begin experimenting different rating ranges soon (i.e. using 1-4 or 0-3). 

For the Model Testing, we’ve taken the time to understand the libraries and recommender system concepts that we will be using. Our models are tested using Cornac which makes it easier to test and experiment with multiple different multimodal recommender systems. In addition, this team is focused on applying the BiLateral Variational Autoencoder recommender system to our final project. Right now, we are building a simple model with a smaller dataset with no images. 

We are currently meeting weekly with Appfolio’s CS Capstone to help them with their current recommendations on their website.

## Obstacles ##

### Data Struggles

Our data consisted of uninformative labels and unintuitive names for values which gave us questions to research and ask our sponsors about . We created a documentation page for our entire dataset so we can use it for quick reference and help future collaborators understand our data. The “item features” dataset includes many null observations which we are unsure on how best to deal with. Our options include dropping the observations that include null values and increasing our precision on the data that we do include, but we would miss out on valuable information regarding recommending the properties we left out. Our other option is to find a way to work around the null observations and perhaps figure out a way to derive default values for these observations.

### Model Complexity

![](https://cdn.discordapp.com/attachments/927701734791458819/938995959747198986/Screen_Shot_2022-01-27_at_7.27.47_PM.png)

It has been a learning curve to understand how recommender systems work. Our chosen model consists of four encoders and one decoder to make item recommendations. It has been a puzzle deciphering the research paper for our model and getting it working to train. Lots of data and a complex model makes training time increase tremendously. It currently takes over thirty minutes for one epoch and our model needs a considerable amount of epochs to be accurate. We talked with our sponsors and set up a notebook on Google cloud that enables us to use GPUs to train more efficiently.


## Future Tasks ##

1. Continue learning about the Bilateral Variational Autoencoder
2. Clean the larger dataset that includes the properties of the CS capstone team’s database
3. Consider other ways to deal with missing data without removing it
4. Find default values in replacement with missing data
5. Update our model to suit the CS team capstone’s website
6. Figure out a way to map between our dataset and the CS capstone team’s dataset
7. Implement our prediction model to the CS capstone team’s website (see image of CS team’s website below)

![](https://cdn.discordapp.com/attachments/927701734791458819/938995930642915408/Screen_Shot_2022-02-03_at_6.10.44_PM.png)

For next quarter:

Upgrade our model with complex features including:

- Images

- Latitude and longitude of property

