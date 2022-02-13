# Update 1
February 4, 2022

Eoin Hayes, Qiru Hu, Jasmine Kwok, Lucas Nguyen, Xavier Speropoulos

## Overview 

We are working with The Center for Information Technology and Society (CITS) at UC Santa Barbara for this research project. Our project sponsor is Qing Huang, a Ph.D. student in the Department of Communication at UCSB. 

The CITS lab is described as, “being dedicated to research and education about the cultural transitions and social innovations associated with technology, particularly in the highly dynamic environments that seem so pervasive in organizations and societies today. There is also work to improve engineering through infusing social insights into the innovative process.”

## Project Description

In 2020, the social media documentary The Social Dilemma has once become the most popular show on Netflix. This documentary reveals the downsides of the major social media platforms. The popularity of The Social Dilemma reflects the public’s general concerns about the information they shared on social media. 

![](https://usustatesman.com/wp-content/uploads/2020/11/the-social-dilemma-1050x700.png)

In response to these concerns, our research project is launched by CITS to examine these technology and society dilemmas from a well-balanced and research-based view with empirical data: how social media platforms, the digital devices that host them, and the algorithms that enable them, produce both beneficial and potentially harmful effects for their users; and how different generations of users share and manage their private information on social media platforms.

## Data

Our team will be web scraping various social media platform data such as Reddit, Twitter, and potentially Facebook user data. We will collect public comments about the movie "The Social Dilemma". We will look at the users' social media usage one month before and after viewing the movie. 

Specifically, we are going to collect the data as described below: 
- All Twitter posts that include the phrase “the social dilemma” between Sep 9, 2020 - Oct 9, 2020 (the month when the movie became the top on Netflix);
- All Twitter posts that include this key words (case-sensitive)“@SocialDilemma_” (the official account of the movie) between Sep 9, 2020 - Oct 9, 2020;
- The account information of all users who posted these posts OR participate in threads under these posts (minimal info: Twitter ID, verified or not);
- Randomly choose 1000 public accounts from this list after removing all verified accounts, and collect all their posts 30 days before and after they posted about either “the social dilemmas” or “@SocialDilemma_“. For example, if one posted on Sep 10, we want to collect all their posts from Aug 11- Oct 10 (minimal info: the total amount of posts within 30 days before and after that post, e.g., Aug 11- Sep 10, Sep 10- Oct 10);
- All Reddit posts that include this phrase (not case-sensitive): “the social dilemma” between Sep 9, 2020 - Oct 9, 2020;
- All public posts (may limited to public figures) from the Social Dilemma Facebook homepage (https://www.facebook.com/TheSocialDilemma/);


## Challenges 

* One of our challenges was data gathering off of different social media platforms. The process of extracting text data using APIs and saving them online (not on our personal computers) was a problem
* Another challenge is our concerns with privacy regarding profiling people and the process of storing these sensitive data. 
* We also realized that one of the main challenges for our project would be finding the right sentiment analysis model whether it is using a pre-existing model or building our own. This is mainly due to the complex nature of social media and language which is largely conversation (slangs and abbreviations) and changes over time.
* We are questioning the effectiveness of popular sentiment analysis packages for our project regarding how they categorize certain comments as positive, negative, or neutral.

## Questions 

1. How do we extract and analyze users' private data without obtaining sensitive information such as age, gender, name, etc? 
2. What are some assumptions we have about social media platforms prior to our analysis? We assume that the text data would reveal and reflect the preferences and actual perspectives of the users. 
3. What are some sentiment analysis techniques/tools or classification techniques that are suitable for the types of data we are working with? (possibly rank them - if we are looking to try and see which works best or a combination of different tools)

## What We Have Done

- Go through the 7-days Social Media Reboot plan and examine the effectiveness of the tools provided by the social dilemma website which aims to regulate social media usage: https://www.thesocialdilemma.com/take-action/
- Literature review of various sources pertaining to social media algorithms and privacy
- Apply for twitter developer API
- Learn advanced Twitter search rules
- Work on different data science notebooks - Google Colab and Deepnote to compare which is more suitable for our project and data 
- Practice data cleaning and sentiment analysis using the NLTK package. Gain Exposure using NLP and how we are able to use it to analyze social media text data more specifically on Reddit and Twitter
Herer's an example of the sentiment analysis result we get:
```
['Just curious, but what makes them privacy friendly besides accepting Bitcoin?'] 
compound: 0.8537, neg: 0.0, neu: 0.461, pos: 0.539, 
['Get Brave browser. It protects your privacy'] 
compound: 0.6908, neg: 0.0, neu: 0.467, pos: 0.533, 
['it stands for Pretty Good Privacy'] 
compound: 0.7269, neg: 0.0, neu: 0.396, pos: 0.604, 
['Privacy wise, this isnt very good, security wise, quite good.'] 
compound: 0.3259, neg: 0.285, neu: 0.352, pos: 0.363, 
['Immediate privacy, security, and legal concerns aside, I hope we eventually reach a safe world-wide meshnet capability.'] 
compound: 0.8316, neg: 0.0, neu: 0.481, pos: 0.519, 
['I love Apple, really the best there is if you care about privacy'] 
compound: 0.9166, neg: 0.0, neu: 0.457, pos: 0.543, 
['That doesn’t help me help you but it was interesting. Good luck on your privacy journey✨'] 
compound: 0.9337, neg: 0.0, neu: 0.421, pos: 0.579, 
['brave imo the best privacy focused browser.'] 
compound: 0.7783, neg: 0.0, neu: 0.424, pos: 0.576, 
['Discord sadly dont like Privacy. 2020 and still no e2e..'] 
    compound: -0.7278, neg: 0.504, neu: 0.496, pos: 0.0, 
```






| Subreddit   | Positive    |Neutral      | Negative
| ----------- | ----------- | ----------- | ----------- |
| All subreddits | 3 |2495 |2
|r/privacy |12|4987|1
|r/socialmedia|0|32|0|
|r/facebook|0|251|0|
|r/twitter|0|115|0|

Figure 1. Analyzing comment sentiments with keyword “privacy” across different Reddit subforums (called “subreddits”). Note that due to logistical constraints, query results were capped at 2500 comments for this preliminary analysis.








## Next Step
We will mine various platforms (Twitter, Reddit, etc.) for data about the movie The Social Dilemma. We aim to analyze the hypothesis about whether this movie has changed audience’s attitude towards social media and increased/decreased people’s social media usage. Specifically, to verify the hypothesis, we want to see people’s social media usage frequency change one month before and after watching the movie and compute the continuous correlation between the usage frequency and users’ attitudes towards the movie.


