# Evidation Update #2 (Julio's Team)

## Intro

Our last blog post was centered around Garmin and Oura data found on Kaggle. Obtaining the data we will be using for the final project proved to be a lengthier process. For ethical and safety reasons, all members had to become verified on Synapse. The reason for the use of Kaggle data was for our team to familiarize ourselves working with Garmin watch and Oura ring data and to assemble code that should be easy to port when working on the actual data. In this way our time was spent productively as we waited for access to the actual dataset. 
Over the past week we have begun data exploration on the actual dataset that we will be using for the final project. Most of this data exploration has centered around analyzing the missing values in the dataset. During this initial data exploration we will discuss trends and our analyses to solidify the questions we will be asking for the final project. 

## Progress and Methods

**Exploratory Data Analysis** -  We began our analysis by plotting histograms of each feature to understand the distribution of the data set. We then created heatmaps to see the correlations, as well as coming up with questions and answers using data visualization and summary (refer to results section). We also found Pandas Profile Report to be a great tool for data exploration for small data sets, as it provides simple yet efficient statistical reports of each data feature, with just a few lines of code.

**Dealing with missing values** - Initially, we dropped all missing values, which were 62 rows and around 21% of the data (Oura Ring). We decided to use the forward-fill method, a common imputation for time series models, as it uses previous values to fill in the missing values. Similarly, in the Garmin data we found that variables of interest like time in deep, light, and REM sleep were missing values for all years starting from 2015, except 2018. We then decided to focus only on data from 2018, and imputed the 10 missing values in 2018 using the forward-fill method. 

**Fake ground truth generation** - Our last update working on the Kaggle data was that we practiced fake data generation. A fake “stress score” attribute was created using coefficients combined with other attribute values and a noise term. Then, a linear regression was run to detect these coefficients. This practice simulated what we would hope to do with the real actual data. Our results from running these practice models can be seen below, such as in **Figure 4**.

## Results
| ![](https://media.discordapp.net/attachments/436308679537459211/949435338017869894/image__3_.png?width=747&height=666) |
|:--:|
| <b>Figure 1: Heatmap of Hours of Sleep by Week (2014 - 2018) </b>|

Figure 1 was created to visualize potential patterns in the amount of sleep that the Garmin user had over the course of a week. For the experimental data, the data was collected over a shorter amount of time (less than four years like in the Kaggle dataset), so if we were to create a similar heatmap, it may be easier to observe any sleep patterns. 

| ![](https://media.discordapp.net/attachments/436308679537459211/949435561066774538/image__1_.png?width=1061&height=657) |
|:--:|
| <b>Figure 2: Violin Plot of Day of the Week vs. Total Sleep in minutes </b>|

As for Figure 2, this plot is ordered by increasing median, which is the white dot along the line that goes through each violin-like shape. The least amount of total sleep appears to be on Fridays, and the most appears to be Saturday and Sunday, which aligns with the assumption that one would get more sleep on the weekends than the weekdays. When we explore the experimental data more, it may be interesting to replicate this plot and observe whether that same assumption holds as well. Then to expand on that hypothesis, we could perform statistical tests to see whether that difference in total sleep between the weekdays and the weekend is significant or not. 

| ![](https://media.discordapp.net/attachments/436308679537459211/949435852172427324/image.png?width=1061&height=398) |
|:--:|
| <b>Figure 3: Shaded Plot of Deep and Total Sleep in 2018 </b>|

Figure 3 was made to practice the Principle of Proportional Ink and a visualization of time series data after reading from the Fundamentals of Data Visualization by Claus O. Wilke (https://clauswilke.com/dataviz/). This figure aimed to show the proportion of deep sleep to total sleep of this Garmin device user throughout 2018. This plot could be applied to the experimental data as well, but it may not be the most informational.

| ![](https://cdn.discordapp.com/attachments/436308679537459211/949434919090794516/IiIiIiIiIiIx6v8DNvmamh7PH8wAAAAASUVORK5CYII.png) |
|:--:|
| <b>Figure 4: Prediction results on fake ground truth data </b>|

Figure 4 is the predicted values vs actual values of the linear regression model on fake ground truth data. We expected this model to perform the best out of the other models we ran(Decision Tree, Random Forest), because the generated stress values were generated using a linear regression formula.

## Future Endeavors

We have officially been given access to our dataset in Week 8. So far we have done some initial data analysis, such as exploring the missing data and understanding some features included in the data. Since it has only been a week since we were given access to our dataset, there are many steps that still need to be done:

**Conduct More Exploratory Data Analysis** - Our first goal is to conduct more exploratory data analysis. We were given many data streams, and so far we have only conducted some initial analysis in the Oura and Garmin data. There are many other data streams to explore, and in order to gain a more holistic understanding of our data, we must look deeper than our initial analysis.

**Selecting Useful Data** - After conducting EDA on the majority of the data streams, we can narrow down our focus and select which features to build our model on. In the Kaggle dataset the features were already decided, but in the real dataset, we were given access to a larger variety of variables. Once we understand our data on a deeper level, we have to select the features that we think would work best for our interest and the client’s needs.

## Technology
**Python**
- Packages: NumPy, pandas, matplotlib, seaborn, sci-kit learn, ProfileReport
