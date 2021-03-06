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

## Improvement for future work.
To improve the experiment in the future:
* I would increase the amount of data to make it more balanced  to decrease model bias because the AutoMl run alerted me of an imbalanced data potential issue which can be fixed by adding more data. This could also indicate a better output for my model.

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/class%20imbalanced%20data.png">
</p>

* I would increase the number of iterations in my AutoML configuration to create room for better performance.
* I would also run my pipeline as a batch inference pipeline, to run in parallel, using the ParallelRunConfig class thereby increasing development productivity and decreasing end-to-end cost.
* I would interact with the pipeline endpoint to monitor how my model is performing and detect data drift, if any.

## Key Steps
### Process
#### AUTOML EXPERIMENT
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

#### DEPLOY THE MODEL AND CONSUME ENDPOINTS

* This model was deployed using the Azure Container Instance (ACI), authentication enabled and application Insights was enabled with the logs.py script.

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/deploy%20best%20model/app%20insight%20enabled.png">
</p>

* The logs from the experiment

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/logs.py.png">
</p>
<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/logs.py%202.png">
</p>

* The [swagger.json](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/swagger/swagger.json) file was put in the same directory as the [serve.py](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/swagger/serve.py) and [swagger.sh](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/swagger/swagger.sh) file. I changed the port of the swagger.sh file to 9000:8080 from 80:8080, then I accessed the Swagger-UI interactive pane on localhost and viewed the HTTP API methods and responses for the model. View [here](https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/tree/master/screenshots%20exp/swagger%20documentation)
* The Swagger UI generates a web page that documents the APIs generated by the Swagger json file. And enables developers to execute and monitor the API requests they sent and the results they received, making it a great tool for developers, testers and end consumers to understand the end points they are testing.

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

#### CREATE, PUBLISH AND CONSUME THE PIPELINE
* To create a pipeline, I adjusted all the configurations to suit the configuration of the AutoML experiment. Pipeline was created and submitted for a run using the AutoML configuration. I retrieved all the metrics for the 45 child runs in my experiment, then retrieved the best model. After testing the data on the best fitted model, I published the pipeline to enable a REST endpoint to rerun the pipeline from any HTTP library on any platform. And to consume it, I submitted an HTTP POST request to interact with the pipeline endpoint.
* All steps shown below

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/pipeline/pipeline%20created.png">
</p>

Pipeline endpoint
<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/pipeline/pipeline%20endpoint.png">
</p>

Dataset in the automl module
<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/pipeline/dataset%20with%20automl%20module.png">
</p>

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/pipeline/published%20pipeline%20overview%20showing%20active.png">
</p>

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/pipeline/run%20details%20complete.png">
</p>

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/pipeline/run%20details%20widgets.png">
</p>

<p align="center">
  <img src="https://github.com/Ayoyinka-Sofuwa/MLOps-Create-publish-and-consume-a-pipeline/blob/master/screenshots%20exp/pipeline/in%20ml%20studio%2C%20scheduled.png">
</p>

## Screen recording
https://youtu.be/UhZcJk9J2b8

## Standout suggestions
* I exported my data to support ONNX and was able to extract the best model in an ONNX file


