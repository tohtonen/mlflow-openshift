## Using examples

These examples are should to be used with Python 3.7 as it is the version used when building MLflwow image to Rahti image repository.
If you encounter any errors, please check your Python version first.

---

### MLflow Wine - tracking example
**Simple example to test tracking metrics to Tracking server**

This notebook utilizes machine learning project packaged as MLflow Projects format, and stored in [Github](https://github.com/mlflow/mlflow-example).

To run this example you need to have jupyter notebook / lab instance on your own computer, or in cloud (like cPouta)

Install mlflow with command:
```bash
pip install mlflow
```
Set up your credentials to environment variables like:
```bash
export MLFLOW_TRACKING_URI=https://<YOUR_APP_NAME>.rahtiapp.fi
export MLFLOW_TRACKING_USERNAME=your_username
export MLFLOW_TRACKING_PASSWORD=your_password

export MLFLOW_S3_ENDPOINT_URL=https://<YOUR_APP_NAME>-minio.rahtiapp.fi
export AWS_ACCESS_KEY_ID=your_generated_access_key
export AWS_SECRET_ACCESS_KEY=your_generated_secret_key 
```
or with notebook if you have Conda environment.

After that, you should be able to run `01_MLflow-Wine-tracking-example.ipynb` to test MLflow tracking.
New experiment and run should appear in Tracking server containing metrics and artifacts of the run.
You can run notebook multiple time with different "alpha" values and compare results in Tracking server UI.

---

### MLflow Wine - model inference example
**Simple example to utilize machine learning model served with MLflow Models**

This example assumes you have run [**MLflow Wine - tracking example**](./01_MLflow-Wine-tracking-example.ipynb) at least once, so you do have model tracked.
**AND you have followed [User Guide *MLflow Models*](../docs/USER_GUIDE.md#mlflow-models) to serve model with MLflow Models.**

To run `02_MLflow-Wine-inference-example.ipynb` you need to have jupyter notebook / lab instance on your own computer, or in cloud (like cPouta).



