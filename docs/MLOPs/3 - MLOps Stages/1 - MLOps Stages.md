## MLOps Stages
Here we will explain the different stages we must follow to complete the MLW Ops cycle. <br>
 <br>
![img.png](images/img.png) 
 <br>
 <br>
Here we have a global vision of all those stages. <br>
 <br>
We would start in the first stage by implementing version control for the model and the data. <br>
 <br>
Next, we will start automating the process of machine learning model development with auto machine learning tools. <br>
Also in this stage, we maintain version control of the model data and code. <br>
 <br>
In the third stage, we will add model serving. <br>
Model serving could be done through an API, web or mobile application. <br>
 <br>
Finally, in the final stage, we would maintain the above components and include the models monitoring, governance and retraining. <br>
It will ensure that the production model continues to function correctly over time. <br>
 <br>
#### Stage 1: Data Collection and Preparation
![img_1.png](images/img_1.png)
 <br>
The first step consists of data collection and preparation. <br>
 <br>
As we already know, there is no machine learning without data. <br>
The part of feature engineering and data cleaning is one of the most relevant phases when training a model data processing is required. <br>
 <br>
Typically, data science teams need access to different historical data. <br>
This data provides from different data sources and different formats.  <br>
Therefore, it is necessary to catalog and organize the data. <br>
 <br>
In addition, we cannot use raw data to train a model. <br>
 <br>
Also, developing individual processing for each model on the same data makes no sense. <br>
On the contrary, developing a catalog and feature storage is necessary if we use identical data for different models. <br>
We have different solutions for this feature storage. <br>
 <br>
![img_2.png](images/img_2.png)
 <br>
Thanks to feature store data transformation, functions can be processed automatically without manual intervention. <br>
Also, we will define data collection and transformations only once. <br>
 <br>
Finally, features will be stored in a shared catalog. <br> 
 <br>
Above are some of the most important libraries to develop a feature store. <br>
The most used is Feast. <br>
DVC is also commonly used, although it also supports model registration and model versioning. <br>
 <br>
#### Stage 2: Automated Development
![img_4.png](images/img_4.png) <br>
 <br>
For this stage, we will implement the automation of developments. <br>
 <br>
Generally, the development of machine learning models follows the same process. <br>
- We start with the exploratory analysis of the data. <br>
- Then we conduct feature engineering and data cleaning, model training, and finally model evaluation and testing. <br>
 <br>
Much of this process can be automated with auto machine learning tools, and that's what we would do in the second stage. <br>
 <br>
Keep in mind that all executions must be versioned and recorded. <br>
We have to store information about the training, data, metadata and model results. <br>
 <br>
#### Stage 3: Create ML Services
![img_5.png](images/img_5.png) <br>
 <br>
Then came stage 3, where we would put our models into production to create different machine learning services. <br>
 <br>
Once we have trained a model, this model must be integrated with the enterprise application or front end services. <br>
It implies that we implement the model without interrupting the service. <br>
 <br>
To achieve this, pipelines will incorporate the following steps. <br>
- The first one will be data collection, validation and feature engineering. <br>
- Then APIs will be developed to serve as model endpoints. <br>
Also, we can serve the model through applications. <br>

#### Stage 4: Model Monitoring, Governance and Retraining
![img_6.png](images/img_6.png)
 <br>
The final implementation stage of the ML cycle is model monitoring, governance and retraining. <br>
As mentioned, model monitoring is a central component in MLOps. <br>
 <br>
This monitoring allows us to keep the models updated and predict with the highest quality. <br>
It is the only way to guarantee the model's validity in the long term. <br>
 <br>
Here we have an example of a monitoring and retraining of the model. <br>
In this case, it is a classification model where they are monitoring the F1. <br>
 <br>
We can see that in the first few months F1 was relatively high, around 0.8. <br>
It indicates that we had a good model. <br>
 <br>
As the months passed, F1 declined. <br>
 <br>
Once the model goes below the threshold of 0.55, the model will be automatically retrained. <br>
 <br>
Then in October, we would train the model again after model retraining. <br>
Its performance reached an F1 of almost one. <br>
 <br>
We can see that the model was losing quality, but thanks to alerts and automatic retraining, the model will continue working well in the long term. <br>