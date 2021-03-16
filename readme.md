# MNIST

Purpose is to build and deploy a simple neural network.

Steps:

1. Create a "STACKn Default" project (after, refresh the page until all apps are available)
2. Upload the notebook in your project folder in labs.
3. Run the notebook to the cell where the model is saved.
4. In the terminal (from your project folder, 'models' should be a subdirectory)

    4.1 stackn setup (URL is https://studio.safespring-prod.stackn.dev, name can be anything)

    4.2 stackn get projects

    4.3 stackn set project -p <project name>

    4.4 stackn create object -n mnist -r minor

    4.5 stackn get models (check that the model is listed)

5. Deploy the model as a Tensorflow model (Check logs to verify that it has deployed, click folder icon)
6. Get the endpoint, paste it into the notebook and call the endpoint to verify that it works.


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

# FEDn (Not Ready!)

1. Create a FEDn project.
2. Clone the FEDn repository: https://github.com/scaleoutsystems/fedn
3. Go to the "test/mnist-keras" directory.
4. Create a "FEDn Client" object:
```stackn create object -t fedn-client -n fedn-mnist -r minor```
5. Check under "Models" in the UI that you have a "FEDn Client" object.
6. Deploy a Reducer, wait until it's running. This will take a long time, since it's also building and pushing the client to your project registry.
7. Deploy a Combiner.
8. Run the client by pulling it from your registry:
```docker run -it -v $PWD/data:/app/data <registry_url>/fedn-client:latest fedn run client -in fedn-network.yaml```
9. Run a few rounds of training.
10. Deploy the latest model with the app "FEDn Serve" under the "FEDn" category.
