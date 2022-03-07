# Evidation Health Project Update 2
## Overview 

Our project focuses on the Stress and Recovery of frontline healthcare workers. Using wearable, survey, and biomedical data from healthcare workers measured during the pandemic, we aim to analyze changes in sleep patterns, cognition, stress, etc. We hope that our findings will give insights into how wearable technology can help detect stress and recovery patterns. 

In addition to the sample data we have been working with, we have obtained access to real data since our last project update. The sample data included only wearable data from one Garmin watch and one Oura ring worn by two different participants. The real data includes wearable data from hundreds of participants in addition to extensive survey and activity data. Participants completed ten background surveys that were collected daily, weekly, bi-weekly, etc. that measured stress, cognition, etc. These surveys will aid our ability to understand and model how wearable data can detect stress and cognition changes by serving as ground truth observations—what stress and cognition levels were actually like at that time. 

## Kaggle Data Findings
Since our team did not have access to the wearable data until week 8, we were tasked with simulating ground truths for the [Kaggle data](https://www.kaggle.com/) (website that contains free datasets). The goal here was to see if we could predict correlations among variables—specifically sleep and stress—and then transfer the same logic and applications onto the real data set. 

## Strategy
Our strategy was to pick two people to simulate data, while the rest of the team tried to determine the relationship—if any—using models and other methods. We utilized linear regression and logistic regression methods to generate the ground truths. For linear regression, we used the sleep variable's [\beta] coefficient (as there was only one variable for stress) and added noise for realism. For logistic regression, we also manipulated the [\beta] coefficient. The [\beta] value determines how strong a relationship is between a predictor(s) and its response.

For the Oura ring data, two sets of data were simulated. Both datasets simulated fake stress scores based off of the real sleep scores from the oura ring. 

The first datasets’ simulated stress scores ranged between 0 (not stressed) and 1 (stressed). Despite trying many different models on this simulated data none of them were able to predict well. The simulated dataset used a small $beta$ and a model should not be detecting a large signal and therefore performed poorly. As we can see from the plot below the mean sleep score for participants given stress scores of 0 is not very different from participants given stress scores of 1.

![](https://cdn.discordapp.com/attachments/841166503402405888/949437824371933194/Oura_Stress_Score.png)

The simulated stress scores were numeral values ranging from 140 to 280. Upon plotting the simulated stress scores and sleep scores they appeared to have a strong linear relationship. Therefore, we ran a linear regression model and the model performed very well. A plot of the data with the linear regression line is shown below. 

![](https://cdn.discordapp.com/attachments/841166503402405888/949437824132861972/Oura_plot.png)
## Multiple Imputation for Kaggle Data
While doing exploratory analysis on the Garmin data from Kaggle, we found that three variables had a significant proportion of missing values: awakeSleepSeconds, deepSleepSeconds, and lightSleepSeconds. These values were converted all to hours later on, but to deal with missing data we explored multiple imputation—which is a process of going through multiple iterations of imputation to reduce bias and errors. 

To perform a multiple imputation, we utilized the miceforest package on Python which performs multiple imputations by utilizing random forests and the mice algorithm to calculate the values to impute. After performing multiple imputation, we analyzed how the original distribution of data compared to the imputed distributions of data. We noticed that there was an underestimation of deepSleepHours and an overestimation of lightSleepHours, but this may be because the mice algorithm imputates based on the relation to other variables. Since deepSleepHours and awakeSleepHours are correlated, the algorithm may have under or over estimated for each variable as a result. 

![](https://cdn.discordapp.com/attachments/841166503402405888/949437829421891634/Multiple_Imputation.png)

After completing the imputation, we ran a linear regression model and also generated fake stress scores and compared it to our linear regression model which used data that was not imputed. We observed that the multiple imputation data had higher mean squared error and mean absolute error for predicting totalSleepHours. However, there was lower mean squared error and mean absolute error, compared to the non imputation data, when it came to predicting stress_scores. This may be because the more data that is available the more accurate the predictions of the stress_scores are. To visualize the differences in the multiple imputation (left) vs. no imputations (right) we created 3D graphs down below. 

Multiple Imputation           |  No Imputation
:-------------------------:|:-------------------------:
![](https://cdn.discordapp.com/attachments/841166503402405888/949437828918566962/3D_Imputation.png) | ![](https://cdn.discordapp.com/attachments/841166503402405888/949437828406865920/3D_Non_Imputation.png)
![](https://cdn.discordapp.com/attachments/841166503402405888/949437829157634128/Imputated.png) | ![](https://cdn.discordapp.com/attachments/841166503402405888/949437828662693938/Non_Imputated.png)

## Real Data Finding 
The real dataset included a Demographic Survey that was submitted by participants in the study. In order to familiarize ourselves with the population, we wanted to take a closer look at the Demographic information. We created custom age ranges in which participants would fall under. Custom age ranges enabled us to interpret the data much more easily. Next, we separated the participants into Female and Male categories in order to take a closer look at the age distribution of the population. Participants had the following options for gender: Male, Female, Non-binary/Third Gender, Other, and Prefer Not To Say. However, all of the participants had submitted Male/Female as their gender. The following graphs are the results of our exploration of the ages of the study’s participants:

![](https://cdn.discordapp.com/attachments/841166503402405888/949437802582507591/AgesPlot.jpeg)

It is evident that the majority of the participants are between the ages of 20 years old and 39 years old with less that 20 participants between the ages of 60 years old and 69 years old. It is also evident that across all age ranges, the majority of participants are Female. More specifically, out of the 365 participants in the study, 325 are Female participants of different ages. 

We also wanted to explore the Ethnicity alongside the Race of the participants in the study. The Ethnicity of the participants in the dataset broke down into the following categories: Not Hispanic or Latino, Hispanic or Lation, and Unknown/Not Reported. The Race of participants broke down into the following categories: White, More than One Race, Black or African American, Asian/Pacific Islander, Native American or American Indian, and Unknown/Not Reported. The following graphs are the results of our exploration of the Ethnicities (top) and Races (bottom) of participants:

![](https://cdn.discordapp.com/attachments/841166503402405888/949437802158895165/EthPlot.jpeg)

![](https://cdn.discordapp.com/attachments/841166503402405888/949437802368630904/RacePlot.jpeg)

For ethnicity, it is evident that the majority of the participants were Not Hispanic or Latino with Hispanic or Latino participants being less than 50. 

It is evident that the majority of participants are White with all other races having less than 50 participants each. 
Following our exploration of the Demographics of the participants, we decided to take a look at the Garmin Watch Sleep Data in the dataset. The Garmin Sleep Data includes data for 77 participants from the 365 total participants in the study. We chose the top 6 participants with the most data entries to further examine. While exploring the 6 selected participants, we noticed there were different sleep data corresponding to the same day. For the same day, sleep data was validated automatically, manually, or changed in some other way. For example, there were three different recorded total hours of sleep for one of the participants. In order to overcome this finding/challenge, we decided to look at how different validations differed for the same day. Since the difference is negligible, we proceeded to take the mean amongst the different sleep data entries with different validations for the same day. To simplify it, we ended up with one sleep data entry for each day for all 6 selected participants. Then, the days of the week were broken down into weekdays and weekends and the total hours of sleep was plotted against Weekday or Weekend for each of the 6 selected participants. The following are the results:

![](https://cdn.discordapp.com/attachments/841166503402405888/949437801961754634/SleepHPlots.jpeg)

The most important finding in our exploration of the Garmin Sleep Data between the 6 selected participants is the variation of total hours of sleep between individuals. Although some of the participants exhibit similar total hours of sleep, there exists a large variation between them and other individuals. In simple words, some of the participants sleep more during weekdays than weekends and vice versa. It is also evident that some individuals sleep more in general than others. 

In order to take our exploration a step further, we decided to look at one available stress survey that we had in the dataset. In the stress survey data, we had variables describing whether the participants worked 1 or 2 or no shifts on a specific day, and whether they were stressed or not at any point during that day. These variables were used to separate the days of the week into days with shifts and days without shifts alongside separating the days of the week into days with stress and days with no stress. The same 6 participants as selected before were selected again for further analysis. The following graphs are the results: 

![](https://cdn.discordapp.com/attachments/841166503402405888/949437802809020426/StressPlots.jpeg)

It is evident that there exists a lot of variation between the participants as to who is stressed and who is not and whether they are stressed or not during the days when they work and the days when they do not. This brings us to our most important finding and the biggest difference between the real data and the Kaggle data. With the Kaggle data, our main focus was on within individual data/differences, however, with the real data our focus has shifted to across individuals data/differences.[^1],[^2] 
## Next Steps 
In the future, we want to take a closer look at the relationship between stress and other variables. Our intuition leads us to believe that sleep, and possibly other variables, has a positive and moderate correlation with stress scores. As a result, we decided that we want to build predictive models that gauge whether or not some variables are significant predictors for stress levels. We believe such an investigation would be helpful for those who are interested in what factors are most relevant when it comes to stress.

We want to conduct further analyses on the survey data along with the wearable data. The vast amount of questions in the survey data must be considered when performing the analysis. Here are some questions we would like to explore: 

* Is this survey question going to be relevant for model fitting? 
* Will considering too many survey questions overfit our model?
* How should we subset our groups for our analysis? For example, should we conduct separate analyses for the 1-2 Shifts group and the No Work group?

In summary, we would like to familiarize ourselves with the survey data to prevent any assumptions or flawed interpretations that could lead to misleading conclusions. We also hope to build interpretative models that could capture the overall data. 

## Definitions:
[^1]: **Within individual** - variability of a particular value/s for AN individual
[^2]: **Across individuals** - variability/differences of a particular value/s BETWEEN individuals 




 

