# Update 3


## Recap
In the last month, we have been working on statistical analysis to determine the change in post history from 30 days before and after each users unique initial post of the Netflix Documentary The Social Dilemma. Additionally, we have been working on building a topic modeling algorithm to divide users posts into groups based upon the contents of each post, which we can then further analyze. The results are individually grouped by sentiment and by topic groups. This will inform us on how certain reactions to the documentary may have impacted a user's post frequency or post content(e.g. sentiment).

## Statistical Analysis
For convenience, we will use $m_1$ to indicate the number of tweets a user posted 30 days before their tweet on the documentary(The Social Dilemma) and $m_2$ to indicate the number of tweets a user posted 30 days after his/her tweet on the documentary. The following steps are how we performed statistical analysis on our data.

Starting with hypothesis testing, we made the following assumptions:
- Null hypothesis ($H_0$): Mean standardized ratio changes for different sentiment groups are equal 
- Alternative hypothesis ($H_1$): At least one standardized ratio change group mean is different from the other groups 

1. We first separated the cases where the $m_1=0$ (where a user did not post 30 days before or after their baseline tweet) from the overall dataset. This is because we want to use the ratio change $\left( \frac{m_2-m_1}{m_1} \right)$ as the dependent variable to indicate the change in people’s tweeting behaviors. When manually checking the group of data with $m_1=0$, we found that users in this group are inactive on the twitter platform as a whole, posting infrequently at all times in our time range. So by removing this group of data, there will not be an influence in the overall result of the statistical analysis.
2. When looking at our data's distribution of ratio change $\left( \frac{m_2-m_1}{m_1} \right)$, we are faced with a problem in our analysis. We ultimately want a standard normal distribution of our data so that we can run various hypothesis tests. The distribution is right skewed due to the limitation of our formula. If our $m_2$ value is large, our ratio change value is large but if the $m_2$ value is small our ratio change is only limited to within -1. To determine this problem and identify potential outliers, we decided to normalize the ratio change. We transformed $\left( \frac{m_2-m_1}{m_1} \right)$ into $\log_{10}(\frac{m_2-m_1}{m_1}+1)$. Taking the log of ratio-change did improve our results and gives us the log ratio change with normalized data. However, we are not done yet as the normalized distribution of log ratio change is symmetrically fat-tailed. In technical terms it is called a leptokurtic distribution, when the tails are fatter than the normal distribution. In order to standardize our distribution, we subtracted the mean and divided it by the standard deviation. We chose to set the outliers to be data points that are three standard deviations away from the mean. We found a total of 17 outliers in our sample dataset.
![](https://i.imgur.com/H70QBU4.png)
3. Next, we performed a one-way ANOVA test using the sentiment number as the independent variable for our sample without outliers. The sentiment numbers are -1, 0, 1, which represents negative, neutral, and positive sentiment. The purpose of this analysis was to test for a difference between standardized ratio changes in groups with different sentiment numbers. We are curious to see if individuals with positive, negative, or neutral sentiments also have different tweet behavior after watching the documentary. In order to do so, we utilized the researchpy module on python and the function __stats.f_oneway__.




### Result
The resultant p-value is 0.011. We reject the null hypothesis since our p-value is smaller than our alpha(<0.05). This means the average standard ratio change of at least one group is different from the others. Hence, we are 95% confident that the negative sentiment group has a positive standardized ratio change of 0.1209. This suggests that there is an increase in users tweeting behavior when they had a negative reception about The Social Dilemma in their initial tweet. The neutral group and positive group have an average mean of -0.0116 and -0.0861 suggesting a trivial change of a slight decrease in tweeting frequency. 
![](https://i.imgur.com/zfUL6f8.png)


## Modifications to Topic Modeling and Semantic Analysis
Our current topic-modeling process uses Latent Direchlet Allocation (LDA) and is fully unsupervised, dividing the social media posts into groups depending on the words they contain.
Moving forward, we are looking to training methods that will allow for a partially-supervised approach, since it will create more distinctly separate groups. 

![](https://i.imgur.com/yotrjaS.png)



## Road Map
- Data Visualizations for Statistic Analysis on 2000 Tweets and Reddit Comments:
Although we are still in the process of performing more in-depth statistical analysis for the before and after tweets of the Twitter and Reddit users, we will eventually look to formalize those findings into data visualizations (graphs, charts, etc.) which will be posted on the CITS website along with a general report of our results.
- Topic Modeling Visualization, Generated Example Tweets (examples for each topic)
- Compiled Literature Reviews:
In the first few weeks of the project, our team conducted a broad literature review of related research for analyzing social media user behavior. We are currently in the process of revising and compiling these separate reports into a single literature review document as an additional project deliverable.
Find secondary data set to train more complex sentiment analysis (e.g. sentiment intensity and emotion recognition)

