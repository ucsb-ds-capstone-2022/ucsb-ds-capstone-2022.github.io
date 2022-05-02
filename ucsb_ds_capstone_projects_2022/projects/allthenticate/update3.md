

# Project Update 3 (5/2/2022)

### **Initial Goals/Methods**

As the quarter began and we sat down to refocus/refine our goals for the second half of the project, it became apparent that our initial goals were too broad in some respects. Upon reflecting on what we realistically could accomplish in the next 10 weeks, we decided to focus on 1) generating metrics that could give Allthenticate insights on their customer app data and 2) getting started on experimental design for a usability study that the company wants to do in the near future. 

Still, the early week or two of the quarter was slow to start, as the team was having COVID-19 troubles, and we were still unsure as to how exactly we wanted to generate our metrics. Previously, we were using plots auto-generated through the logging infrastructure setup, but none of it was truly written by us, so it felt restrictive in the choices we could make as data scientists. We began working on our python implementation of these metrics, and now that we had a reliable data pre-processing pipeline from the last update, we were able to work with the data more effectively. Initial efforts included using libraries like matplotlib and seaborn for visualizations, but none were as dynamic as Plotly, which ultimately became the tool of choice as it had the interactive capability built in and was extremely intuitive to interact with our data.

### **Progress/Findings**

We have generated several real time visualizations in the Kibana dashboard on our Elastic instance running on AWS. The first set of visualizations (Figures 1-4) show relevant real time use data for identifying issues within the application. One can infer from a spike of users opening and closing an application, turning bluetooth on and off, or refreshing the app that there is an issue with the performance of the app. Additionally, if there is a spike in messages about the phone trying to connect to local devices, there is clearly an issue on the backend of the app or device infrastructure. Figures 5 and 6 show the distribution of lock and unlock times, and can be used to identify abnormally long lock/unlock times, which can help identify edge-cases resulting in poor product performance.
  
![](images/app_launched_closed.png)
<p align="center">
<em>Figure 1. Count of Apps. Launched/Closed from 15.00hrs to 00.00hrs.</em>
</p> 

![](images/ble_enabled_disabled.png)
<p align="center">
<em>Figure 2. Count of Bluetooth Connections/Disconnections from 15.00hrs to 00.00hrs.</em>
</p> 

![](images/app_refreshed.png)
<p align="center">
<em>Figure 3. Count of times App is refreshed from 15.00hrs to 00.00hrs.</em>
</p>

![](images/device_connecting_not_connecting.png)
<p align="center">
<em>Figure 4. Device connecting to phone/ Device in range but not connecting from 15.00hrs to 00.00hrs.</em>
</p> 


Beyond real-time visualizations, we have also begun working towards generating meaningful static visualizations for deeper insights into the functionality of the company's products. This work has been somewhat slow as the updated app has yet to be deployed to all possible users, but we have worked to create a secure method for accessing data within a Python script using credentials that are ignored from Gitlab commits. 

  ![](images/lock_unlock_time.png)
<p align="center">
<em>Figures 5,6. Histogram on time taken to lock/unlock a device.</em>
</p> 

Additionally, we have created a script for cleaning and formatting the data in a structure that will allow for time series analysis, with logs divided by each unique phone ID and device ID in order of the log’s timestamp.


|    | phone                                | timestamp                  | phoneModel   | event_type   | uuid                                 |   type | locked   | connecting   | connected   | connectable   |   communicating |   processing_command |   action | time_delta             |
|---:|:-------------------------------------|:---------------------------|:-------------|:-------------|:-------------------------------------|-------:|:---------|:-------------|:------------|:--------------|----------------:|---------------------:|---------:|:-----------------------|
|  0 | 1234cf90-5d23-423b-8ffe-caddb46fb123 | 2022-03-02 09:08:11.251430 | DUB-LX0      | seen         | 123f95b7-1bb2-4a54-8340-123e375c480e |      1 | False    | False        | False       | True          |             nan |                  nan |      nan | NaT                    |
|  1 | 1234cf90-5d23-423b-8ffe-caddb46fb123 | 2022-03-02 09:08:15.524714 | DUB-LX0      | seen         | 123f95b7-1bb2-4a54-8340-123e375c480e |      1 | True     | False        | False       | True          |             nan |                  nan |      nan | 0 days 00:00:04.273284 |

### **Next Steps**

In the coming weeks, we want to add the app versions into our logs so that we can differentiate between a new customer problem and an issue for which a fix has already been released. Having app version in our data will also allow us to filter our visualizations on our dashboard by app-version, which would give us better insights on how Allthenticate is improving with each update, and not just on a cumulative distributional level as it is right now. Additionally, we want to be able to differentiate performance by the operating system currently running on the phone, so we will be adding that to the logs as well. Along the same lines, we want our dashboard to be able to give users a time frame they want to see data from (past 7-days, past 30 days, etc), which combined with app-version would be an incredibly powerful tool that Allthenticate can use to see improvements in their product as well as problem instances as they arise. 

Another course of action we have in mind to be able to improve the product is to perform cluster analysis. As introduced in previous posts, we have extracted different variables from the logging dataset (e.g. connection time). Our idea is to, after the clustering is done, analyze these variables within the clusters (e.g. average connection time) to be able to target specific profiles that are having either lots of problems with the app or a very pleasant experience with Allthenticate. However, clustering is an unsupervised learning technique, so it is sometimes difficult to assess the correctness of the results. This is where the feedback of the survey comes into play and why we described it as a “target variable”. If we have survey answers for most of the users (say around 70-90% of the total users), we can (on top of analyzing the statistics of the attributes we extracted within the clusters) analyze the survey results within the clusters. The survey includes 21 questions, but there are two in particular (included in the post-test section) we could use as target variables:

* Rate Allthenticate from 0 to 10 without considering any malfunctioning you've experienced, e.g. low connection time, BLE problems (rate the concept itself)
* Rate Allthenticate from 0 to 10 considering malfunctioning problems you may have experienced, e.g. low connection time, BLE problems

Another big goal for us is to push the logging to all users, as currently it's only active on test phones in the office. The main roadblock here is getting SSL working so that customer data is secure, but this is close to being implemented and will turn our 6-7 user dataset into 100’s of users, which might give us additional challenges with visualization that we want to tackle. SSL security has been implemented within the phone app and our server that records the logs, and is currently blocked by the backend system used to forward the logs to our database. The addition of more users will present new challenges, as the number of logs generated will likely be in the hundreds of thousands weekly. To preempt the strain this puts on our current data pipeline, we are aiming to have our dashboard and graphing code run on a more powerful server continuously.

Our overarching plan is to eventually integrate our dashboard into an Allthenticate employee portal that can allow employees to look at the company’s data, whether it be for marketing, business analysis, or bug-fixing purposes. 
