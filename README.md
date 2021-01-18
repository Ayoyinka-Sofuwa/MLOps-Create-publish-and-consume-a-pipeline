# Operationalizing Machine Learning

## Overview
In this project, we build a Machine learning workflow from creating an AutoML experiment and training models to deploying the best model, consuming and interacting with an HTTP API. Then we create, publish and consume the pipeline to allow for version control and access.

## Summary
I trained a classifier Automated learning experiment with the bank marketing dataset to determine if the customers will make a fixed deposit or not, using features containing their personal and professional information, and details. Then I deployed the best model and consumed the endpoint using a swagger-ui API. 
After this, I created, published and consumed a pipeline to make it publicly available and allow for version control.

## Process
To Create the AutoML experiment int the SDK, I first created a compute instance of STANDARD_DS3_V2.
After loading in the dependencies, defining/loading in the workspace, naming and creating a directory for the experiment, I created a compute target for the AutoMl run and loaded the dataset using the 'key' and URL.
* The classifier AutoML experiment was created by: setting the timeout minutes for the experiment, the maximum concurrent runs to be lower than the maximum no of nodes in the compute cluster and the primary metric as "AUC weighted".
* And configurated with: the compute target, the task type (Classification), the training data, the target column, the directory path, early stopping policy, featuriztaion type and I made it ONNX compatible.
And the experiment was submitted.

After the AutoML experiment was completed, the best model was VotingEnsemble with an accuracy of 91.4% and AUC weighted of 94.6%.
This model was deployed and application Insights was enabled with the logs.py script.

