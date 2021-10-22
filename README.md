# MLflow deployment for OpenShift container cloud #

This repository contains OpenShift Template of [MLflow](https://mlflow.org) - machine learning lifecycle management tool.  
Following services (pods) will be created:  
- **MLflow Tracking Server** records data and metrics to track and compare performance of machine learning training experiments.  
- **MLflow Models** is used to serve machine learning models for inference.   
- **PostgreSQL** database to store all metrics and metadata from your training runs.  
- **Minio** object storage to work as easy to approach artifact store. CSC Allas is preferred for heavier use.

## Things to note ##

Artifact storage:
- Minio object storage in path `s3://default` is created to work as Default artifact store. This is good for testing and small scale usage.
- Since artifacts can be quite large files, it might be necessary to change `DEFAULT_ARTIFACT_ROOT` to point some external storage system. CSC Allas or some other external S3 compatible object storage should be set up for artifact store to make them available to Tracking server. (instructions TBA)

To utilize artifact store, you have to add generated keys to your programming environment. Keys are shown on summary page when deploying template.
After deployment, keys can be found from Rahti web console `Resources -> Secrets` view. Example below in **Variables for programming environment** -section.

With default settings services are accessible from everywhere. To restrict access modify Whitelist variable
in OpenShift template and add your static ip or your organization ip range.  

MLflow Models is deployed but not started. Starting up Models serving pod needs access to model stored in Artifact store.
You can start Models after setting up
`MODELS_URI` in `models-cfg` config map by increasing pod count to 1 after you have saved your model to MLflow model registry. 

### Variables for programming environment ###
Please note that MLflow itself is not programming environment. You can develop your machine learning code in your own environment 
in your own machine or in CSC cloud or supercomputer services.
You should set up environment variables in your programming environment to access this MLflow Tracking server!
Tracking server uri is the address of your deployed tracking server (for example: https://<YOUR_APP_NAME>.rahtiapp.fi)
```bash
export MLFLOW_TRACKING_URI=https://<YOUR_APP_NAME>.rahtiapp.fi
export MLFLOW_TRACKING_USERNAME=your_username
export MLFLOW_TRACKING_PASSWORD=your_password

export MLFLOW_S3_ENDPOINT_URL=https://<YOUR_APP_NAME>-minio.rahtiapp.fi
export AWS_ACCESS_KEY_ID=your_generated_access_key
export AWS_SECRET_ACCESS_KEY=your_generated_secret_key 
```
OR
```bash
conda env config vars set MLFLOW_TRACKING_URI=https://<YOUR_APP_NAME>.rahtiapp.fi
conda env config vars set MLFLOW_TRACKING_USERNAME=your_username
conda env config vars set MLFLOW_TRACKING_PASSWORD=your_password

conda env config vars set MLFLOW_S3_ENDPOINT_URL=https://<YOUR_APP_NAME>-minio.rahtiapp.fi
conda env config vars set AWS_ACCESS_KEY_ID=your_generated_access_key
conda env config vars set AWS_SECRET_ACCESS_KEY=your_generated_secret_key 
```

## Installation instructions ##

When deploying MLflow template at Rahti, you have to define some variables first:

- App Name: You can define unique name for your MLflow instance. Name will be part of app public address.
- Username: Your self defined username for tracking server.
- Password: Your self defined password for tracking server.
- Storage Size: Storage size for MLflow persistent storage. Default should be enough.
- Backend storage size: Storage size for tracked metrics. Default should be enough.
- Object storage size: Storage size for tracked artifacts.
- Route whitelist: Add your static ip or ip range if you want to restrict access to your service.

For rest of the variables default is fine for most users.
- MLflow image: Latest tested image for Rahti as default value.
- Artifact store access key: Generated by template.
- Artifact store secret access key: Generated by template.
- PostgreSQL connection username: 
- PostgreSQL connection password
- PostgreSQL connection database
- Application Hostname Suffix

After setting up all variables, click Create to start deployment. Summary window shows addresses for Tracking server and Models server.
Also generated keys for artifact store are shown here. Tracking server address, your credentials and artifact store keys should be added to your
machine learning development environment to send metrics and artifacts to MLflow.

**Congratulations!** After deployment is complete, you are ready to start using MLflow to track your machine learning training runs, compare models and to deploy best ones to inference.
There is simple test notebook `MLflow-Wine-example.ipynb` in `examples` directory.

More detailed usage instructions will be published later. 

---

## Version history
**Version 0.8.0 - 22.10.2021
Lowering the learning curve**
- Added mlflow-secret.yaml for CSC Allas configuration
- Added user_guide.md with instructions for Models and CSC Allas
- Renamed tracking example
- Added inference example 
- Added README.md for examples

**Version 0.7.0 - 12.10.2021
Utilizing PostgreSQL and Minio**
- Included S3 credentials as secret and mounted for pods
- Added Minio and configured as default artifact store
- Defaulted PostgreSQL as backend store
- Unnecessary user defined parameters removed 
- Small changes to component names
- Added tensorflow 2.0.0 and h5py libraries to Mlflow image
- Removed user defined object store address from template and set generated credentials (instructions to modify TBA) 
- Improved documentation
- Added simple example notebook to demonstrate tracking metrics

**Version 0.6.0 - 24.9.2021
Distribute metrics to PostgreSQL**
- Added postgres and psycopg2 libraries to Dockerfile
- Added PostgreSQL pod deployment to template - no startup
- Added secrets, pvc and service for postgresql
- Reduced mlflow-ui storage from 10gi to 1gi
- Changed startup-script to default artifact store to ./mlruns
- Default backend-store still pvc-filesystem
- Default artifact store set to ./mlruns and note to instructions

**Version 0.5.1 - 20.9.2021
Update a bit**
- Updated MLflow from 1.13.1 -> 1.20.2
- Python image source back to DockerHub

**Version 0.5.0 - 18.1.2021
Let's get back to basics**
- Removed Helm chart from master
- Removed Allas configuration and instructions from master to make it simpler to setup 
- Created new 'dev' branch for advanced features

**Version 0.4.0 - 27.11.2020
Towards public template**
- Upgraded MLflow to 1.12.0 version
- Changed configuration to read S3 credentials from Secret to env var 
- alpine-htpasswd image source from DockerHub to Rahti Image registry
- README improvements
- Changed template structure

**Version 0.3.0 - 13.11.2020  
From tracking only to serving models:**
- Added Models serving to both Helm chart and template

Known issue: S3 credentials still set in deployment env vars instead of secret

**Version 0.2.0 - 11.11.2020  
Towards multi-purpose receipt:**
- Upgraded MLflow to 1.11.0 version
- Public image from Rahti Registry set as default
- Added OpenShift template for Rahti
- Changes made to prepare for separating Tracking Server and Models to different pods
- Minor fixes and cleanings

**Version 0.1.0 - 9.3.2020  
Towards Public networks:**
- Added support for IP-whitelisting
- Added simple NGINX-proxy authentication


*Created by Juha Hulkkonen*
