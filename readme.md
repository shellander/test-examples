General preparation: Download the Jupyter Notebooks from this repository, or be prepared to Git clone the repo or files as needed.

# MNIST

Purpose is to build and deploy a simple neural network.

Steps:

1. Create a "STACKn Default" project (after, refresh the page until all apps are available)
2. Start a new lab session (_Compute -> New -> Lab_) on the main Overview page. Name can be anything, select "project-vol" as Persistent Volume and leave the rest as defaults. 
3. Upload the _mnist_example_tf_serving.ipynb notebook_ to your project folder in labs (project-vol).
4. Run the notebook to the cell where the model is saved (`tf.saved_model.save...`).
5. In the Jupyter terminal (from your project folder, 'models' should be a subdirectory)

    5.1 `stackn setup` (Name can be anything, Studio host is https://studio.safespring-prod.stackn.dev)

    5.2 `stackn get projects` 

    5.3 `stackn set project -p <project name>` (set your current project)

    5.4 `stackn create object -n mnist -r minor`

    5.5 `stackn get models` (check that the model is listed)

6. Go back to the STACKn interface, go to Serve and deploy the model as a Tensorflow model:
    6.1 Click _Create_ in _Tensorflow Serving_
    6.2 Choose a name, set _Model_ to the model you just created and leave the rest as defaults
    6.3 A Tensorflow Serving service will pop up in the top of the window. Check logs to verify that it has deployed, (look for 'Entering the event loop ...') by clicking the folder icon.
7. Get the endpoint (right click the _Open_ link), paste it into the Jupyter notebook and call the endpoint to verify that it works.


# AML

1. Create a STACKn default project, or create a volume in your existing project.
2. Clone the AML repository to your volume: https://github.com/scaleoutsystems/aml-example-project.git
3. Create a model object in STACKn (see point 4.1-4.3 above, create the model from the AML project directory, so for example 'aml-vol/aml-example-project/ )

    3.1 Create a tar archive of the relevant folders: tar czvf ../aml-model.tar.gz src models requirements.txt setup.py helpers.py (we do this to avoid including the big "dataset" directory)

    3.2 stackn create object -f ../aml-model.tar.gz -n aml -r minor

5. Deploy the model with the "Python Model Deployment" app.
6. Get the endpoint, check the logs, and go to the notebook "predict.ipynb" in the repository. Paste your URL, and make a prediction.

# PyTorch

1. Go to your Minio instance and create a bucket, say "pytorch". Upload the VGG_scripted.mar model archive.
2. Deploy the model with the "PyTorch Serve" app, the endpoint should be "public" and the volume should be your minio volume. Path to model store is your bucket name. List of models can be left empty (then, by default, all models in the directory will be deployed).
3. Create a volume for a Dash app.
4. Launch a VSCode instance where you mount your newly created volume.
5. Clone this repository: https://github.com/stefanhellander/dash-test.git to your volume.
6. Deploy the app with the app "Dash Deployment" (under "Serve", path should be relative to your volume, so probably "dash-test")
7. In VSCode, go to your folder under "/home/stackn" and update the app with your model endpoint.
8. Test the app!

# Transformers example project

1. Clone the repo onto a volume: https://github.com/scaleoutsystems/transformers-example-project
2. In the repository directory, run stackn create object -n afbert -r minor
3. Deploy the model with "Python Model Deployment". It takes a long time for this model to initialize, so keep checking the logs until it is available. "Running" != "Ready"
4. Copy the endpoint url, paste it in the "predict" notebook in the repository, and predict.

# MLflow

1. Create a STACKn default project.
2. Create a Jupyter Lab instance.
3. In your project volume, upload the notebook "mnist_train", and run all cells.
4. In MLflow, click the run, scroll down to "artifacts", select the model, and click "Create model".
5. Verify that the model exists in STACKn under "Objects"->MLflow.
6. Go to Serve, deploy the model with the "MLflow Serve" app.
7. Go to your notebook, upload mnist_predict, change the URL to your endpoint and verify that it works.

# FEDn (Not Ready!)

1. Create a FEDn project.
2. Create two buckets in the project Minio: fedn-models, fedn-context
3. Clone the FEDn repository: https://github.com/scaleoutsystems/fedn
4. Go to the "test/mnist-keras" directory.
5. Create a "FEDn Client" object:
```stackn create object -t fedn-client -n fedn-mnist -r minor```
5. Check under "Models" in the UI that you have a "FEDn Client" object.
6. Deploy a Reducer, wait until it's running. This will take a long time, since it's also building and pushing the client to your project registry.
7. Deploy a Combiner.
8. Run the client by pulling it from your registry:
```docker run -it -v $PWD/data:/app/data <registry_url>/fedn-client:latest fedn run client -in fedn-network.yaml```
9. Run a few rounds of training.
10. Deploy the latest model with the app "FEDn Serve" under the "FEDn" category.
