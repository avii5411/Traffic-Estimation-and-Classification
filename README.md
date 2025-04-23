Urban traffic congestion is a critical challenge that affects both transportation efficiency and environmental sustainability.
The project was divided into two major phases: Part A, focusing on data preprocessing and exploratory data analysis (EDA)
Part B, which involved implementing Decision Tree classifiers and Neural Network regression models for traffic flow prediction.
**The dataset used in this project originates from the Caltrans Performance Measurement System 
(PeMS), specifically the PEMS03 dataset, which contains traffic data from District 3 (Sacramento and 
Marysville areas) over a period of three months (September–November 2018). This data includes 
traffic flow (vehicles per hour), occupancy, and speed measurements collected from 358 sensor 
stations across the region. 
Data - Preprocessing was a crucial step to structure and prepare the raw dataset for analysis. The 
primary focus was on data extraction, station mapping, and transformation into a structured format. I 
have loaded and sorted the station IDs that would be used in the analysis. The Station_IDs.txt file 
contained a list of 358 station IDs, which were loaded and sorted for consistency. A mapping 
dictionary was then created to assign a unique index to each station, ensuring efficient referencing 
during later stages. 
Each station is associated with specific metadata, including freeway number, direction, county, 
latitude, and longitude. This information was extracted from the Station_Metadata.txt file and 
structured into a dictionary. 
• The metadata file was read using Pandas with tab-separated values. 
• A nested dictionary structure was created, where each station ID served as a key, storing 
corresponding metadata as a dictionary. 
• This metadata was later used for spatial analysis and feature extraction. 
Temporal Data Transformation 
To facilitate time-series analysis, the dataset was transformed into a NumPy array of shape (2184, 
358, 3), where: 
• 2184 represents the total number of time intervals, 
• 358 represents the number of stations, 
• 3 corresponds to traffic flow, occupancy, and speed. 
Steps followed: 
1. Dataset Loading: The traffic data from PEMS03.txt was read into a DataFrame. 
2. Unique Timestamp Mapping: The time column was extracted, sorted, and mapped to indices. 
3. Final Transformation: The dataset was iterated, and each observation was placed into the 
corresponding time-station index in the NumPy array. 
The final structured dataset was saved as PEMS03.npz for future use in predictive modeling. 
To understand more I have done Exploratory Data Analysis (EDA), to understand traffic patterns, 
trends, and variations across different stations and time periods. 
Traffic congestions exhibit predictable patterns, underscoring the necessity for time-aware forecasting 
models. 
The connection between speed and flow is clear: with the rise in traffic volume, the average speed 
reduces. 
Because spatial and temporal dependencies are robust, predictive models need to include both time
based and station-based relationships to ensure accurate forecasting. **

Following data processing and feature engineering, we have implemented two machine learning 
models, a Decision Tree Classifier for categorical traffic flow estimation and a Neural Network 
regression model for continuous traffic prediction. However, in the 
development of the Decision tree model, In this model I categorize the flow into 3 categories: Low, 
Normal and High. The roadmap for the development: 
Feature Selection a total of 14 features were taken which account to temporal, spatial, traffic-specific 
and Geospatial. 
Post the feature selection, I have decided to train the model by splitting the data, into 3 subsets, 70% 
of the data  is used for training, and 20 percent for the validation, which is used for hyperparameter 
tuning, and the rest 10% for evaluating the unseen data. DecisionTreeClassifier’ from the sklearn.tree 
model is used, one of the important aspect when considering the decision tree model, is selection and 
tuning of hyperparameters, so based on the base case, I have selected  
• “gini” as the criterion 
• Max_depth of 7 is selected to avoid overfitting 
• Min sample split: 300 – thereby reducing unnecessary complexity. 
• Min sample leaf – 100 – prevent over specific splits 
• Max Features – sqrt – creates randomness at each split and reduces the overfit 
• Max Leaf nodes – 50  
• Random state – 42- without random state, every execution might produce different Decision 
structures and causes inconsistencies, thus maintaining reproducibility. 
With these, Decision tree was trained on the data set that was fixed on the whole data set, and is 
evaluated later on the test data set.  
When the model is evaluated it showed an overall accuracy of 91 %, showing strong performance. 
The feature importance analysis revealed that rolling averages and average distance to neighboring 
sensors were the most influential predictors. 
for Neural Network regression Model,In the neural network regression section of the notebook, a function is defined to prepare the input data for training a model that predicts traffic flow. The process involves creating a variety of features that capture both temporal and spatial aspects of the data. These include time-based features like hour of the day, day of the week, and month; lag features such as traffic flow one hour and twenty-four hours earlier; rolling statistical features like average and variance over recent hours; and spatial features such as the average traffic and distance of neighboring stations. Additional inputs like road occupancy, vehicle speed, and geographical coordinates are also included. For each station and time step, the function extracts these features using only past data to maintain the integrity of the time series. The result is a set of feature vectors and corresponding target values (actual traffic flows), ready to be used for training a neural network model. I worked with the fine 
tuning of the model and checking on different parameters and measure how the model is responding, 
When we were discussing we tried to check on the models performance, on when it was subjected to a 
Full batch gradient, Mini Batch gradient and  and we have found out that, for training Neural Network 
for regression, Mini-bacth Gradient descent is the best approach, as it provides balance. As suggested 
we have used Adam as the optimizer,  
(batch_size = 64, epochs = 15, initial_lr = 3e-2, lr_decay_rate = 0.99, lr_decay_steps = 100, patience 
= 5, min_delta = 1000) 
When we have trained the model, using Adam optimizer and Mean squared Error as loss function. The 
training is executed for the set value of 15 epochs, with early stopping mechanism, We have identified 
that both the losses from training sharply during the first epochs and validation losses are decreasing 
over epochs, which shows that the model is learning. The key takeaways I have identified from the 
model is after few epochs, the improvement becomes minimal and indicating the saturation point. 
However, we are still working on the model, and since form the literature studies, I have suggested 
that to check the performance of early stopping, to identify and tune the patience and min_delta 
parameters. As form the picture it suggests that validation loss is consistently low than training losses, 
it suggests that some of the features are under utilized, so feature selection mapping methods can be 
utilized to increase the performance. 
Conclusion: For the chosen data set, Decision Tree performed better, with high accuracy of 91%, 
however Regression model has a higher Mean Absolute error of 149.69 and I suggested fine tuning of 
the hyperparameters. 
