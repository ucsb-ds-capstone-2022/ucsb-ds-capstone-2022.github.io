# Evidation Julio Update 3

## Overview 

Our goals have remained the same throughout the quarter. We hope to be able to identify factors of stress with wearable technology. Our project’s goal is to utilize wearable device data coupled with survey data to fit a model that would predict stress. If enough time permits, we plan on comparing the two models built from the Oura Ring and Garmin Watch to see which device predicts stress more accurately.

## Current Progress

### Oura Update

The data for the Oura Ring came in three streams: sleep, which measured how well the participants slept, readiness, which determined how well a participant was for the day, and activity, which measured the physical activity the participant undertook throughout their day. We combined the three datastreams together to create one huge Oura Ring dataset. By the nature of wearable technology, the Oura Ring dataset is time-series data so we used outer-joins to combine the datastreams together. By using outer-join rather than inner-join, there are no gaps in our time-series. We assigned self-reported stress scores in the Self-Assessment Mannequin (SAM) dataset to be the ground truth values. We used an inner-join rather than outer-join for this process, as we did not want any missing values in our ground truth values.

| ![](https://cdn.discordapp.com/attachments/920973425038737469/970922769636282408/Screenshot_2022-05-02_223000.png) |
|:--:|
| <b>Figure 1:  Summary Data </b>|

We decided to create a logistic regression model for the Oura Ring. We split up our data into training and test datasets by participants. Our current model right now is being trained on 70% of the participants, and being tested on 30% of the participants. We used the Recursive Feature Elimination (RFE) method to select 25 features. We then manually took out features with p-values < 0.05 from the summary report to only keep statistically significant features. For now, our model accuracy is around 60% (using the messy data).

| ![](https://cdn.discordapp.com/attachments/920973425038737469/970922856743575552/Screenshot_2022-05-02_224039.png) |
|:--:|
| <b>Figure 2: Logistic Regression for Oura Ring </b>|

### Garmin Update

We narrowed down a long list of sensor measurements and features from each Garmin data stream to the following list:
Heart rate measurements (i.e. max heart rate in beats per minute)
Oxygen saturation measurements (i.e. level of oxygen in the blood)
Respiration measurements (i.e. rate of breaths per minute)
Sleep measurements (i.e. duration of deep sleep, starting time, etc.)

Stress measurements 
After filtering these datastreams, we were ambitious and initially tried joining these filtered datastreams (using a full outer join) hoping everything would turn out well. To no surprise, our dataset increased by roughly two orders of magnitude going from about 3,000 observations to 262,378 observations. This amount of growth implied that we had not yet done a good enough job of making sure that our initial data streams were cleaned and prepared to join.

Before attempting to join these data streams again, we examined specific data streams and made a few observations. These are some points of concern we had after doing so:

Duplicate observations – multiple observations per participant in a day

Sleep throughout the night - unpacking a sleep level map to understand participants’ sleep patterns

Normalization - create a comparable metric to evaluate each feature between participants

### redCAP Update

We chose work shift data as the ground truth for both Oura and Garmin datasets. One issue we had given that we were using a Logistic Regression model to predict was that the ground truth classification values were unbalanced. We used the Synthetic Minority Oversampling Technique to fix this issue.

About 90 percent of the demographic is Caucasian, which raised a possibility that maybe race demographic had something to do with some of the data discrepancies. However, nothing was found to support this yet. 

We were able to make some initial findings using this redCAP data:

Surprisingly, most participants experienced their stress when off-shift.
Most feelings of stress lasted 2-3 hours.
It was exceedingly rare for a healthcare worker to have two shifts in a day. Most participants either had one shift or did not have any. Working multiple shifts was very rare.

## Challenges 

### Missing Values

Joining the data streams together came with several challenges that needed to be looked at. Outer joining 3 datasets left us with many missing values. The imputation of these missing values is up for discussion, and it is important to consider the context of each variable with missing values carefully. We must normalize several scores (e.g. sleep scores) across participants; individuals all have varying relationships with sleep, stress, etc. and in order to ensure there is no bias in any models with the scores, normalization is necessary. 

### Time Series

Each observation is not completely independent from another due to the time factor. For instance, sleep is affected by sleep from the days before. With that in mind, it’s important to take the context into consideration before imputing variables. For instance, for imputation of missing sleep values to make more sense, we were advised to take the weighted average of the previous 2-3 days to fill a missing value.

| ![](https://cdn.discordapp.com/attachments/920973425038737469/970923015825162280/Screenshot_2022-05-02_224116.png) |
|:--:|
| <b>Figure 3: Sleep Score Time Series </b>|

These two plots show the range of missing values within participants that we had to work with. Some participants may have very little missing values, while others have more missing values than the actual data.

#### Duplicate Observations

In both the Oura Ring and Garmin Watch datasets, we encountered issues with duplicate observations (i.e.our datastreams included multiple observations for the same participant in a single day). Depending on the intended resolution of your project (think hourly, daily, weekly, etc.) this may or may not be an issue. For our model, we are interested in a daily resolution of wearable sensor data. Instances of duplicate observations were present in many of our datastreams, though we will focus on the Garmin streams for sleep and respiration and use them as case studies to understand some causes of repeated observations and how to deal with them.

### Garmin: Respiration

As we looked further into the respiration datastream, we noticed that one feature named ‘durationInSeconds’ only contained a value of 900. Knowing that ‘durationInSeconds’ indicates the duration of the measuring period for a participant’s rate of respiration, we were interested in the fact that each observation represents a 15 minute measurement period.

| ![](https://cdn.discordapp.com/attachments/920973425038737469/970923169395388426/Screenshot_2022-05-02_224153.png) |
|:--:|
| <b>Figure 4: </b>|

(In this image, we see that one participant in one date had multiple observations of average breaths per min recorded—in fact, the number of observations came out to 87 throughout this particular day.)

We consulted the Garmin API and saw that their wearables take 15 minute measurements and use that to track a change in respiratory rate in each user throughout the day. This is bad news for our desired daily granularity, but it is easily remedied. Our solution was to average out the measurements for each participant for a given day to produce a single daily observation for each participant!

### Garmin: Sleep

We also took a look at the sleep datastream and noticed a similar issue with respect to duplicate observations. Each participant had more than one recorded sleep event during the day. Our leading hypothesis was that the Garmin API was also keeping record of naps throughout the day. To address this, we filtered out each participant's measured sleep periods and removed events that started and ended on the same day as long as they were shorter than 2 hours (this method is based on the assumption that most sleep sessions start during the night time and end the following morning). While this did manage to filter out some observations, it did not make a significant difference overall.

Instead, we took a look at the feature ‘sleepLevelsMap’ which is a dictionary (storing key-value pairs where the key is the depth of sleep achieved during a measurement period (deep, light, awake, and REM) and the values are sleep level time ranges represented as unix timestamps in seconds).

| ![](https://cdn.discordapp.com/attachments/920973425038737469/970923255785476126/Screenshot_2022-05-02_224214.png) |
|:--:|
| <b>Figure 5: Duplicate Example </b>|

(Here we can see the first two duplicate observations for a participant. Note that each observation reports a different sleep duration and start time.)

After noticing this, we wrote a script to unpack these sleep map dictionaries to help us better compare the differences between these two recorded sleeping periods. We arranged the unpacked dictionaries into dataframes and placed them side by side below for easier comparison.

| ![](https://cdn.discordapp.com/attachments/920973425038737469/970923376141041674/Screenshot_2022-05-02_224240.png) |
|:--:|
| <b>Figure 6: Side by side sleep map dictionaries) </b>|

From this comparison: we confirmed that these separate periods were not necessarily naps that were taken and were in fact two different recordings of the same sleeping period; we also noticed that the second observation included REM sleep, a label that is clearly missing from the first observation. With this, we were finally able to understand the discrepancy of multiple sleep period measurements per day. We noticed that each observation from the same day reports a different value for validation, notably they are either 'AUTO_FINAL', 'AUTO_MANUAL', 'AUTO_TENTATIVE', 'ENHANCED_FINAL', or 'ENHANCED_TENTATIVE'. There is no mention of these tags in the Synapse data description; however, we were able to find documentation from fitrockr, a company who provides fitness data solutions, which explains the features that are missing from the Synapse sheet. The descriptions can be found at their website linked [here](https://www.fitrockr.com/data-types/).

| validation            | Description |
| :---                  |    :----   |
| **AUTO_FINAL**          | The sleep start and stop times were auto-detected by Garmin Connect, and enough data has been gathered to finalize the window.       |
| **AUTO_MANUAL**         | Sleep data was auto-detected by Garmin Connect, but the user is overriding the start and stop times or the user started with a manual entry and the sleep was auto-detected later.        |
| **AUTO_TENTATIVE**      | The sleep start and stop times were auto-detected by Garmin Connect using accelerometer data. However, it is possible that further refinements to this sleep record will come later.       |
| **ENHANCED_FINAL**      | Sleep data was collected from a device capable of running an enhanced sleep analysis to detect REM sleep, and no further updates or refinements to this sleep analysis are expected.        |
| **ENHANCED_TENTATIVE**  | Sleep data was collected from a device capable of running an enhanced sleep analysis to detect REM sleep, but an updated sleep summary record may come later with further refinements or a greater sleep period.        |

The next step was to clean this data stream by keeping a single entry for each day, prioritizing the validation tags in this order: 'ENHANCED_FINAL', ENHANCED_TENTATIVE', 'AUTO_FINAL', 'AUTO_TENTATIVE', 'AUTO_MANUAL'. This method reduced the number of observations to 1,301 (down from 6,719).

### Unbalanced Classification

| ![](https://cdn.discordapp.com/attachments/920973425038737469/970923478570135562/Screenshot_2022-05-02_224305.png) |
|:--:|
| <b>Figure 7: Unbalanced Classification </b>|

This figure shows the amount 0’s (no stress) dominates over 1’s (stress) from the redCAP dataset. When we used this data as a logistic regression ground truth value, the model couldn’t detect the 1’s and there was likely bias towards the majority value of 0.

## Future Plans

The next step we must undertake is to normalize the data, for both Oura and Garmin. After we normalize the data, we can interpolate the missing values– which is also its own matter of concern in terms of determining an appropriate method of imputation for each variable. In general, we are still preparing both Oura and Garmin datasets – for instance, after normalization and imputation, we would need to merge the data streams for the Garmin dataset – to be ready to fit models on. After completing the preparation and cleaning of our datasets, we plan to fit and fine-tune models that would predict a stress score and/or the presence of stress given the information recorded by the Oura Watch and Garmin Ring. 






