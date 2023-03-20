# flask_app_ops

This repo contains Kubernetes Deployment and Jenkins CI/CD pipeline configuration regarding Flask APP.

### Kubernetes Manifests

The Kubernetes folder contains the following manifest files:
- configmap.yaml: deploys a ConfigMap Kubernetes Object which contains a map of the environment variables for the application.
- secret.yaml: deploys a Secret Kubernetes Object which contains the encrypted database password.
- deployment.yaml: deploys a Deploymnet Kubernetes Object which contains the app pod template to be deployed.

### Jenkins Pipeline

The Jenkins pipline is a CI/CT/CD pipeline, hence it builds, tests, and deploys the application. The pipeline consists of 5 stages defined in the jenkinsfile as follows:

- Stage 1 - Checkout Flask-APP : in this stage the app source code is cloned
- Stage 2 - Build and Test : this stage builds the docker image which also has unit test embeded in the Dockerfile, so if the unit tests fail the image build wont continue
- Stage 3 - Login : login to DockerHub, to be able to push the built image
- Stage 4 - Push : Push the Docker image to DockerHub
- Stage 5 - Deploy: deploy the latest image into the kubernetes cluster by restarting the flask-app deployment pods, since the specifed image tag for the app deployment is latest whenever a pod is restarted the latest image is pulled from the registry
