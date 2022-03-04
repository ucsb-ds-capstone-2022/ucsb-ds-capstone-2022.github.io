# Update 2
March 3, 2022

Eoin Hayes, Qiru Hu, Jasmine Kwok, Lucas Nguyen, Xavier Speropoulos

## Overview 
Our team worked on web scraping various social media platforms including Reddit and Twitter for public posts/comments about the movie “The Social Dilemma”. We looked at the users’ social media usage frequency one month before and after viewing the movie and statistically analyze whether this movie leads to any behavior change among its audience. We did sentiment analysis and topic modeling on these posts to determine their attitude towards this movie - agree, disagree, or neutral. We aim at testing the hypothesis of whether this movie changes people’s attitudes toward social media (i.e. increase/decrease social media usage, and sentiment towards social media in general).

[![pipeline.png](https://i.postimg.cc/8cS5DSc3/pipeline.png)](https://postimg.cc/34t7CzgC)

## Current Progress 

### Twitter data

<details>
    <summary> <b>Mining Twitter Data</b>: Developed a data mining pipeline for collecting Twitter data. Sampled 1000 tweets and collected the tweet counts of the Twitter users 30 days before and after posting the tweet. </summary>
    We scraped all English Twitter posts that include the keywords “the social dilemma” or “@SocialDilemma_” between Sep 9, 2020, and Oct 9, 2020, using Twitter API. We removed tweets from verified accounts and tweets entirely consisting of foreign language and URL links for the convenience of sentiment analysis. We also removed tweets posted by the same person but kept the first tweet they posted. After this, we sampled 1000 tweets along with the usernames from a total of 122759 tweets. We further collected the total number of tweets each sampled user posted 30 days before and after their posts related to “the social dilemma”. Our goal is to observe if there were any evident differences in these users posting behaviors before and after they watched the movie. We assume that all the 1000 users who tweeted on the topic have watched the movie. 
</details>

<details>
    <summary> <b>Observations</b>: 
<ul>
 <li>Many tweets had the same content. Is it possible that they were posted by bots? This may cause biases of data. </li>
<li>Analyzed users' tweet counts and tested the hypothesis on the difference of tweet counts 30 days before and after. The hypothesis was rejected.</li> 
</ul></summary>
<ul>
  <li> From the dataset, we observed that there are a huge number of duplicated tweets. There are 51620 tweets which are tweets duplicated more than 5 times. We doubted that these tweets might be produced by bots for propaganda purposes. To check if a tweeter user is likely a bot, We used an API called <b>Botometer</b>, developed by Observatory on Social Media (OSoMe, pronounced "awesome") at Indiana University. This API calculates the bot score of each Twitter user account using a machine learning algorithm. The results showed that among 1000 sampled users, none of them has a probability higher than 95% to be a bot. 6 of them have a chance between 90% to 95% to be bots. We will probably remove the 6 users and resample 6 tweets from the original dataset.</li>
  <li>We also conducted some exploratory analysis and found the mean tweet count for 30 days before the initial social dilemma tweet of 1000 users was 721.019 and the mean tweet count for 30 days after was 743.928. We proceeded to carry out a hypothesis test to check if there were any differences in the tweets counts for 30 days before and 30 days after. At a 95% confidence level, we decided to fail to reject the null hypothesis as there was no statistically significant evidence suggesting that the total tweet counts 30 days before the initial post on social dilemma were different from the total tweet counts 30 days after. From the scatterplot of tweet counts 30 days before and 30 days after, there was a positive linear correlation which makes sense since we would expect people to continue the same posting behavior regardless of their post on the social dilemma. However, an interesting finding is that there is larger variability in posting behavior for users who has fewer tweet counts than those who have higher tweet counts.</li>
</ul>    
</details>
<a href="https://ibb.co/LpHDtdD"><img src="https://i.ibb.co/dKxs24s/Scatter-Plot.jpg" alt="Scatter-Plot" border="0"></a>

Figure 1. Tweets count 30 days before and 30 days after shows a positive linear correlation.


### Reddit data

<details>
    <summary><b>Building the pipeline: </b>collecting, sorting, and categorizing Reddit posts and comments.</summary>
    Focus on learning the various options of accessing subreddit and Redditor data using the various Reddit APIs available. Pushshift API was chosen, but we also used the official Reddit API to look at the metadata of comments and posts.
Creating a pipeline to access, and sort the data collected without being limited by the size of the query. 
Being able to collect Reddit users' (Redditors) posts and comment history to determine if they are active members of the platform. 
Have not run any analysis on the data, cleaning and making sure the data was viable and collectible has been the main task.
Now that the data has been collected and preprocessed, we can look for the activity of users, perform post and comment sentiment analysis. When doing this we will probably have to do topic modeling because some comments might mention the movie but not really give an opinion or express their feelings about it. Example: “Did you watch the Social Dilemma?”
</details>

<details>
    <summary><b> Results from initial efforts: </b> technical and logistical challenges (mostly involving APIs), and how we overcame them.</summary>
    There was a problem that we encountered with limitations on the number of results you could get using the official Reddit API. Only getting about 250 results is fine to look at the potential data but is not sufficient enough to develop any model.
The various APIs that are available that created a workaround for the limitation of the results are not the most up-to-date solutions. The workaround is by archiving the entire activity of Reddit posts and comments for the day and storing the data on a separate server. This can be difficult because posts and comments are allowed to be edited and the private server will not have any of those updates to any comment or post after the posted day. Also, the servers themselves can be down at times not allowing you to work if you rely on the API
Used the keyword Social Dilemma to obtain one of our main datasets. There were around 6400 results for comments relating to the keyword. We decided that going after posts was more difficult because there is no search function to just look for posts. There is a potential workaround by scraping every single subreddit for posts including Social Dilemma, but with Reddit having hundreds of thousands of subreddits this path seemed inefficient. The comment dataset also confirmed this because out of the 6400 comments, they were in 1276 different subreddits. There is potentially more data if we use the comment id to identify the post id and then scrape each post id for the social dilemma but we could run into more problems with trying to clean that data. 
The biggest challenge the group is facing with the Reddit data is figuring out how to collect the users' whole profile and be able to analyze the activity.
</details>


### Analyzing the Data
In our content analysis, we wanted to answer two main questions:
1. Was their post about the documentary, or about social media in general?
2. What was the sentiment of their post? (positive / negative / neutral)
<details>
    <summary><b>Sentiment Analysis:</b> How did people feel about the documentary? How do they feel about social media in general?</summary>
    In order to develop our model we are using a prelabeled data set from Brandwatch. We manually confirmed the accuracy of Brandwatch's own sentiment analysis through random samples of positive and negative tweets. We have developed cleaning and text vectorization methods for modeling training that involve removing hyperlinks, stopwords, duplicates(retweets), lemmatizing, and finalling tokenizing important entities. Our Linear SVC model currently has an accuracy of 71% which we are looking to improve to approximately 80%. We found that our model has a lot of trouble correctly classifying positive tweets, which may have to do with the fact that our dataset has a smaller proportion of positive tweets. 
</details>
<details>
    <summary><b>Topic Modeling:</b> Determining whether people were discussing about the documentary, or social media as a whole (or both).</summary>
    In our exploratory topic modeling procedures, our goal was to try and differentiate between social media posts discussing the documentary and posts discussing the broader topic of social media as a whole. In doing so, we could then figure out where the sentiments were directed towards, which is crucial in answering our research question. A positive sentiment towards Facebook or Twitter is very different than a positive sentiment towards the documentary, and the statistical analyses should reflect that.
</details>
<details>
    <summary><b>Next Steps: </b> increasing the effectiveness of sentiment analysis and topic modeling methods  </summary>
    Since our sentiment analysis and topic modeling tools do not yet accurately predict the nature of the data, we plan to increase the complexity of our models to adapt to the complex nature of the dataset. We have already begun implementing bi-gram and tri-gram analysis to capture commonly occurring phrases, and will look into other word-relation methods to find patterns in the data, perhaps leveraging some supervised machine learning methods with manually labelled data points.
</details>
<br />

[![wordcloud.png](https://i.postimg.cc/G3FPD3F4/wordcloud.png)](https://postimg.cc/K4YgS2sb)

Figure 2. Word cloud of posts on "The Social Dilemma"

## Next Step

### Our goals 

* Further clean our messy text data (contains URL, the mixture of languages, emojis) and communicate with our mentor about the effects of duplicated tweets on our analysis. 
* Conduct advanced statistical analysis by splitting out sample into groups or based on sentiment analysis results. 
* Manually label our sample to act as a training dataset for sentiment analysis and topic modeling carried out to enhance model accuracy
* Create a pipeline to extract Reddit user names to then bin their post and comment history. This will be great to analyze users' activities before and after posting about the social dilemma to see if there was any change in their social media habits. 
* Tune hyperparameters for our Linear SVC and explore other supervised learning models for sentiment analysis.
