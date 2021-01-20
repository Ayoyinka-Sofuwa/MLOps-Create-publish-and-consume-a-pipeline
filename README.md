# Operationalizing Machine Learning

## Overview
In this project, we build a Machine learning workflow from creating an AutoML experiment and training models to deploying the best model, consuming and interacting with an HTTP API. Then we create, publish and consume the pipeline to allow for version control and access.

## Summary
I trained a classifier Automated learning experiment with the bank marketing dataset to determine if the customers will make a fixed deposit or not, using features containing their personal and professional information, and details. Then I deployed the best model and consumed the endpoint using a swagger-ui API. 
After this, I created, published and consumed a pipeline to make it publicly available and allow for version control.

## Architectural diagram
<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/architectural%20diagram.png">
</p>

## Key Steps
### Process
To Create the AutoML experiment int the SDK, I first created a compute instance of STANDARD_DS3_V2.
After loading in the dependencies, defining/loading in the workspace, naming and creating a directory for the experiment, I created a compute target for the AutoMl run and loaded the dataset using the 'key' and URL.
* the dataset was registered in the workspace

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/automl%20experiment/registered%20dataset.png">
</p>

* The classifier AutoML experiment was created by: setting the timeout minutes for the experiment, the maximum concurrent runs to be lower than the maximum no of nodes in the compute cluster and the primary metric as "AUC weighted".
* And configurated with: the compute target, the task type (Classification), the training data, the target column, the directory path, early stopping policy, featuriztaion type and I made it ONNX compatible. And the experiment was submitted.

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/automl%20experiment/experiment%20complete.png">
</p>

* After the AutoML experiment was completed, the best model was VotingEnsemble with an accuracy of 91.4% and AUC weighted of 94.6%.

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/automl%20experiment/best%20model%20summary.png">
</p>

* This model was deployed using the Azure Container Instance (ACI), authentication enabled and application Insights was enabled with the logs.py script. And the logs 

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/deploy%20best%20model/app%20insight%20enabled.png">
</p>
<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/deploy%20best%20model/logs%202.png.png">
</p>

* The [swagger.json](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/swagger/swagger.json) file was put in the same directory as the [serve.py](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/swagger/serve.py) and [swagger.sh](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/swagger/swagger.sh) file. I changed the port of the swagger.sh file to 9000:8080 from 80:8080, then I accessed the swagger-ui interactive pane on localhost and viewed the HTTP API methods and responses for the model. View [here](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/tree/master/screenshots%20exp/swagger%20documentation)

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/swagger%20documentation/swagger%201.png">
</p>

* Consuming the endpoints using endpoint.py file, inputting the REST endpoint URL and the primary key(available because of the authentication enablement), i got a json output
<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/endpoints/endpoint%20with%20json%20output.png">
</p>

* In the benchmark.sh file, view below, my experiment had no failed request and the time per request was within and well below the 60secs limit by Azure

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/endpoints/apache%20benchmark%20output.png">
</p>


## Screen recording
https://youtu.be/UhZcJk9J2b8

## Standout suggestions


publishing the pipeline enables a REST endpoint to rerun the pipeline from any HTTP library on any platform
