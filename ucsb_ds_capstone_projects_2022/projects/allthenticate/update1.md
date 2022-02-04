# Project Update 1 (2/4/2022)

### **Domain Background**

Allthenticate is a cybersecurity company founded in Santa Barbara, CA by UCSB alumni that aims to transform the way that people will interact with their devices. Their products enable the use of a smartphone to unlock everything, from doors to your office, to logging into a website, and even unlocking your car. 

The company has broad goals to simplify yet bolster modern network security, where anyone can be authenticated and be able to communicate securely yet easily. With ambitions set high, it’s important to be able to quantify and provide evidence of a superior product, which leads to the need for a team of data science students. 

The capstone team, which consists of undergraduates Akshat Ataliwala, Calvin Jenkins, Bosco De Enrique Romeu, and Yuki Yamazki, as well as graduate advisor Leron Reznikov aims to accomplish 2 goals in this project.

### **Motivation**

Why are we doing Data Science at Allthenticate?
As you already know, Allthenticate is a cybersecurity company which allows users to, using their phone and the application; combine any kind of authentication (doors, passwords, cars) into a single action. So, as you can imagine, the amount of (very sensitive) data Allthenticate generates is huge. Curently that data is not being exploited for any business purposes, therefore: Why not join forces with a talented and passionate group of students and give Data Science a try? Though having data available for analysis is not the only motivation behind this project, it is an important one.

Allthenticate was founded in 2019, so it is a relatively young and fast growing company. Although the members of Allthenticate are in close contact with most of their clients; the company lacks an understanding of how the general user interacts with the product. How many times are users typing their passwords? How many times do they need to open the app? What settings are they using? 

We believe this exciting data-centric project will be able to quantify and shed a light on the role Allthenticate occupies in its customers day-to-day, and much more.
 
### **Data**

**Important Terms**

* **ELK**: Elasticsearch, Logstash, and Kibana running together

* **Elasticsearch**: open source, JSON-based search engine and storage systemetc.) on assigned readings

* **Logstash**: open source, filtering and aggregation software for logs

* **Kibana**: open source, browser-based visualization dashboard

* **MQTT**: real time lightweight IoT communication protocol

* **Filebeat**: lightweight log forwarding software

There are two main datasets we will be using to analyze user statistics and Allthenticate’s usability. 
The first dataset is a collection of logs from the door readers and computer software that Allthenticate has stored in MongoDB. This data was collected using HTTP posts to the server running MongoDB. We do not yet know the structure of this data.
The second dataset is a separate collection of logs, this time from the app running on the phones. This data is stored in a server running Elasticsearch, with a Kibana dashboard for live visualization and log tracking. This data was collected from the phones using the MQTT IoT communication protocol, where there is a client running on every phone that connects to a broker running on a server in order to send the logs. Once these logs reach the MQTT broker, they are forwarded to the Elasticsearch server instance using Filebeat. Before they reach the server, they can be filtered and modified using Logstash. This infrastructure is modeled below. 

![](images/log_infrastructure.png)
<p align="center">
<em>Figure 1. Model of logging infrastructure for phone data.</em>
</p> 

This data comes in two forms, both in JSON format. The first type of data is logged when there is an action on the phone taken by the user. This data contains the phone's unique ID, the action taken, and the time. This data tracks when the user opens the app, when they close it, when they turn bluetooth on or off, and when they lock or unlock a door or computer. This format is shown below


Phone | Action | Time
---|---|---
7564cf90-5d27-423b-caddb46 | lock | 2022-01-28 04:47:23.957818
7564cf90-5d27-423b-caddb46 | ble enabled | 2022-01-28 00:12:31.482374
 
The second type of data is information that the phone stores about all of the door readers and computer that it connects to. This data is updated whenever the information for this device changes from the phone’s perspective. This data contains fields for the phone’s unique ID, the time, and then the device info of the device. This device info includes fields for the type of device it is (1 for door, 2 for computer), whether it is locked, whether it is connected, whether it is processing a command, whether it is connectable, whether it is communicating, the unique ID for the device, and the strength of the signal from the device. This data is shown below.

Phone | Time | Device Type | Connected | Connectable | Locked | Connecting | UUID | RSSI
---|---|---|---|---|---|---|---|---
471bf5ea-887b-4eaf-8324 | 2022-01-28 00:01:40.798130 | 1 | true | true | false | false | 10f9ef4-14fc-42a7 | -51

### **Objectives**

The primary objective of this project is to improve the Allthenticate app by analyzing the user data provided by the logs from doors, computers, and phones. To achieve this, we have two goals we aim to accomplish.
First, we want to quantify how much of an improvement Allthenticate’s security solution provides as opposed to existing solutions. Second, we intend to determine how well the product is working, and use that data to build the product into catering the best experience for the user. In order to reach these goals, we must collect and analyze the user data (such as the number of times the app was used in a day, the amount of time saved by using the app, the app status throughout the day, etc.) through using ELK (Elasticsearch, Logstash, Kibana).
Another objective we would like to achieve through tracking the growth of this young company is to publish at least one academic publication at a top-tier conference. There are sure to be a lot of surprises in our data and we believe it would be fun and interesting to track.
