# Overview

Example of build of a Cloud Run compliant container for serving tensorflow prediction model.

# Build the container

You have to retrieve your tensorflow model from Google Cloud Storage. 
Either update `_EXPORT_BUCKET` default value in the `cloudbuild.yaml` file and run this command
```
gcloud builds submit
```

Or simply override the variable from command line
```
gcloud builds submit --substitutions=_EXPORT_BUCKET=gs://path/to/my/models
```

# Deploy on Cloud Run

Deploy on your project (update `PROJECT_ID` in the following command)
```
gcloud beta deploy predict --image gcr.io/<PROJECT_ID>/predict
```

# Test your model deployment

Create a file with the required inputs for your models. Here an example for my models in file `instances.json`.
File can contain up to 100 instances.

Then call your Cloud Run deployment with this input file (replace the `hash` with your Cloud Run URL)
```
curl -X "content-type: application/json" -X POST \
    -d @instances.json https://predict-<hash>.run.app/v1/models/default:predict
```

# License

This repository is licensed under Apache 2.0. Full license text is available in
[LICENSE](https://github.com/guillaumeblaquiere/cloudrun-tensorflom-prediction/tree/master/LICENSE).
