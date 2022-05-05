# Evidation Health Project Update 3 

## Current Focus

Akin to the previous update post, our goal is to be able to build a predictive and interpretable model that allows us to determine which factors contribute to stress. To garner some familiarity about the data and participants, we decided that we should try to predict whether or not a participant was stressed on a particular day, given several attributes. This will be the stepping stone for building more robust models and selecting the best subset of predictor variables for stress predictions. 

To begin our process, we analyzed various survey datasets. After some consideration we decided to merge our Oura Ring data with the Daily Stress Measure dataset, ACE (Adverse Childhood Effects) dataset, the PTSD (Post-Traumatic Stress Disorder) dataset, and the Demographics dataset. Additionally, we decided that the sleep score should be lagged by one day; otherwise, the sleep score would be associated with a previous date-- which does not make intuitive sense! Some more cleaning was done to account for participants who slept past midnight and filled out a survey more than once.

After merging data together from multiple different datasets, our focus shifted on building a predictive model for stress. Our first step was to create some new variables in the merged dataset. These variables included the ACE survey total score for each individual and the PTSD survey total score for each individual. Our focus for this model was generalization, therefore, our next step was to drop participant ids and date. This was done in order to create a generalized model which would tell us information about predictors of stress, independent of individuals. Next, Sleep variables were divided by 3600 in order to convert them from seconds into hours. Score variables were also divided by 100 since those variables were on a scale of 1 to 100. Following these steps, we selected as many variables from the dataset as possible, keeping in mind the multicollinearity phenomenon. The following figure is a showcase of the dataset with which we would begin modeling, having an array of predictors and stress as the response variable:

![](https://cdn.discordapp.com/attachments/969726610284609606/969726782062362724/PP3Pic2.png)

With the guidance of our mentors and research done on our part, we decided that running a Logistic Regression model would be a great first step in beginning modeling. Logistic Regression would be the best option in assisting us to statistically determine significant variables in predicting stress. Therefore, we ran a Logistic Regression model with all of the predictors showcased in the figure above. This allowed us to select the predictors that are statistically significant in predicting stress. With this information, we created a new data frame containing only the statistically significant predictors and the response variable stress. This is showcased in the figure below:

![](https://cdn.discordapp.com/attachments/969726610284609606/969728003208773672/PP3Pic3.png)

With the reduced number of predictors, we ran another Logistic Regression model:

![](https://cdn.discordapp.com/attachments/969726610284609606/969728321950732358/Screen_Shot_2022-04-29_at_1.32.40_PM.png)

After running the Logistic Regression model and given a challenge with our response variables which will be discussed later, our focus shifted to figuring out the best threshold for the classification of stressed vs. not stressed. In order to figure out the threshold, we decided to plot the ROC curve:

![](https://cdn.discordapp.com/attachments/969726610284609606/969728736935153755/PP3Pic5.png)

The best threshold that we calculated from the ROC curve was 0.225. This is much different than the default 0.5 threshold and is due to the class imbalance in the response variable that we have. With the calculated threshold, we classified stress vs. not stressed. For the classification report, the focus should be on the Recall score for the prediction of stressed individuals. This is because recall is a metric best used when there is high cost associated with a false negative. In our case, the false negative would be predicting an individual as not stressed when they are actually stressed. This is much more important than falsely predicting someone as stressed when they actually are not.

![](https://cdn.discordapp.com/attachments/969726610284609606/969729019266338846/Screen_Shot_2022-04-29_at_2.01.02_PM.png)

From the figure above, which is the classification with the best threshold from the ROC curve, we can see that we have a recall of 0.7 for the predicted stressed class of our data. Overall accuracy is not as significant in our case because of the reason with false positives as mentioned before. To summarize the modeling we have done until now, the confusion matrix below is an ideal method:

![](https://cdn.discordapp.com/attachments/969726610284609606/969729239257595954/Screen_Shot_2022-04-29_at_2.39.11_PM.png)

## Successes and Challenges

After successfully running our model with the aforementioned variables, we decided to add in each participant’s id to also create models at the participant dependent level. Ideally, this would take into account a particular participant’s number of days stressed and increase the strength of our logistic regression model. However, adding in the participant’s id proved to be more complicated than expected… when ACE and PTSD totals are in the model! Somehow, if either of these two totals are included in the model, they mess with each predictor’s interpretability by introducing a large amount of nan values under most predictors in the model’s summary. We are currently investigating the cause of this phenomenon, with our main hypothesis being multicollinearity between participant id and the totals. It is a bit disappointing since the model still runs fine and has a decent ROC curve.

To explain a bit about why our focus has been on a generalized model:

A surprising challenge we came across was deciding if we wanted to build predictive models for each participant or a model across all participants. Since stress, as well as some of our predictors like number of hours slept, are very subjective and vary across individuals, a model for each participant seemed to be the better call. However, some participants filled out the daily stress survey only a handful of times and therefore did not leave us with a lot of information. In addition, building individual models would not allow us to say anything about predictors which, independent of individuals, can be indicative of stress in this population. Therefore, we decided to focus on building a model that can generalize well and predict stress across all participants. It is in this model that we find our biggest successes and another large challenge:

We performed exploratory data analysis and sought out variables that seemed correlated to stress in our population. One such variable was a participant's Adverse Childhood Events (ACE) score. This survey asks participants about situations they dealt with in the home as a child. For challenging events, such as having parents divorce, participants receive a point. As you can see from the visualization below, participants with low ACE scores (between 0 and 3), had a different distribution of stress than participants with high ACE scores (between 4 and 10), indicating that participants with low ACE scores generally experience less stress than those with high ACE scores.

![](https://cdn.discordapp.com/attachments/969726610284609606/969747754018557972/Screen_Shot_2022-04-29_at_4.45.59_PM.png)

Variables such as ACE– that seem to indicate stress– were used in the variable selection of our logistic regression model. For the time being, our choice of predictor variables is one of our greatest successes in the project. This subset allowed us to build a predictive model that performed quite well on new data.

In building this model, we discovered yet another challenge in our project which was hinted at earlier. Our dataset has a very imbalanced number of stressed and not stressed days (ground truths). This imbalance is so substantial that it leads our model to predict almost everything as not stressed, since the model is able to achieve relatively good overall prediction accuracy with this naive approach. A possible solution to this is focusing on interpreting the results of our model instead of trying to predict stressed or not stressed; thus, we may start to focus on the differences in log odds instead. We have begun shifting this way by changing our model to lend itself more easily to comparison and interpretability by adding an intercept to act as a baseline and standardizing variables for later coefficient comparison.

![](https://cdn.discordapp.com/attachments/969726610284609606/969747904342396988/Screen_Shot_2022-04-29_at_4.46.15_PM.png)

## Future Work

Our end goal for our project is to create a mixed effects model[^1] that is able to predict based on the various variables we have such as ACE, PTSD, and wearable sleep data. Before being able to run the model, we are hoping to work on implementing some regularization techniques such as LASSO[^2] and Ridge Regression[^3]. To perform the regularization, we are planning on using sklearn and cross-validation to hypertune as well as exploring the RPy2 package. We are also in the process of further variable selection and would like to continue hypertuning our model, selecting significant variables, and observe AUC[^4] score for our ROC[^5] curves. We also wanted to focus on creating more visualizations for our final model as well as each of the datasets we worked with. For visualizations, we wanted to produce both across individuals vs. within individual plots and, if time permits, explore how to use Tableau[^6] to eventually create a dashboard of all the visualizations together. 

## Definitions:
[^1]:**Mixed Effects Model**: Model containing both fixed effects and random effects 
[^2]:**LASSO**: Regularization technique that shrinks data values towards the mean (encourages a more simple model with fewer parameters)
[^3]:**Ridge Regression**: Method of estimating the coefficient of multiples regression models
[^4]:**AUC**: Area under the ROC Curve which measures the performance across all possible classification threshold
[^5]:**ROC**: Graph that shows how well a model classifies the true positive and false positive rate
[^6]:**Tableau**: Data visualization tool used for data analysis


