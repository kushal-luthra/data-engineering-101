## Parts of MLOps

Here we can see where each of the parts of MLOps is located. <br>
We start with the value proposition, then data versioning, data analysis and experimentation, code repository, pipeline orchestration, model registration and versioning, production deployment, model serving and finally model and application monitoring. <br>
 <br>
In the value proposition, we will identify different machine learning use cases. <br> 
Then we identify the available data sources, analyze them version, and register the data sets. <br>
Once we have done this, we move on to the development and training of the model. <br>
Here, the feature stores are generated, the model is registered and versioned and the metadata is stored. <br>
Finally, the application is generated and monitored in the production startup part. <br>
 <br>

![img_1.png](img_1.png) <br>
 <br>
Even though the ML ops field is quite broad, we will look at the most critical parts. <br> <br>

#### Feature Store
The feature store is a critical part of ML ops. <br>
We must _store_ the functions used in the training of a model. <br>
Ensures that there is no duplication in the creation of functions. <br>
Features can also be searched and used to build other models or analyze the data. <br>
Functions are also _versioned_. <br>
It ensures that you can revert to a previous version of the feature. <br>
 <br>
#### Data Versioning
Data versioning ensures reproducibility in model creation. <br>
In addition to functions, you can also store the data set used to train a model. <br>
It is also essential during the audit to identify the data sets used. <br> <br>

#### Metadata Store
Another essential part is the metadata store. <br>
In machine learning, it is vital to store the configuration and metadata of the model. <br>
It is essential to increase reproducibility. <br>
Some artifacts that we have to record is the random seed used to split the data. <br>
We can store other metadata, for example, model evaluation metrics, hyper parameters or configuration files. <br> <br>

#### Model Versioning
Model version control is vital because it lets you switch between models in real time. <br>
Apart from that, multiple models can be used to monitor performance. <br>
Version control is also critical from the governance point of view of model and compliance. <br> <br>

#### Model Registration
Model registration should not be mistaken for model versioning. <br>
Once we have trained the model, we should store it as a new record. <br>
Each model in the registry will have a version. <br>
In addition, each model is stored with its hyper parameters, metrics version of functions used and version of the training data set. <br> <br>

#### Model serving
Once the model is versioned and registered, we will move on to the model service part. <br> <br>

It is nothing more than deploying the model to serve users. <br>
When we create model consumption endpoints, we can use those endpoints to get predictions. <br>
A model container could also integrate an application. <br>
However, if we want to use the model in several applications simultaneously, it is better to use endpoints with APIs. <br>
 <br>
Another alternative is to deploy the models on edge devices with tiny machine learning. <br> <br>

#### Model Monitoring
We should monitor models for possible deviations and biases. <br>
The deviation occurs when the statistical characteristics between training data and new data change unexpectedly. <br>
Therefore, the performance of the model degrades. <br>
We can detect these problems early by monitoring the statistical properties of both training and prediction data. <br>
In addition, production bias occurs when the deployed model has a different performance than the local model. <br>
 <br>
Errors can be caused during the training process, service errors or discrepancies in the data. <br> <br>

#### Model Recycling / Retraining
Another part of ML ops would be model recycling, also known as model retraining. <br>
Models can be retrained for two reasons. <br>
The first reason is to improve performance. <br>
The second reason is when new training data is available. <br> <br>

#### CI/CD
If we detect newly available data, it should trigger model retraining. <br>
It is where continuous integration and implementation come in. <br>
Continuous integration and continuous deployment ensure that models are built and deployed frequently. <br>
Continuous delivery ensures that code is frequently merged into a central repository where automated builds and tests are applied in machine learning. <br>
This would involve testing the code and the resulting models. <br>
 <br>
It also involves creating a container of the models so that users can make use of that model. <br>
