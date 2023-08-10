# Build and Deploy a Docker Image to a Kubernetes Cluster

## Prerequisites 

EXPORT ZONE=
EXPORT CLUSTER_NAME=
EXPORT APP_NAME=
EXPORT MACHINE_TYPE=

1 Before starting this lab, we need to download the source code of the application which is given in the lab instruction.
```
gsutil cp gs://$DEVSHELL_PROJECT_ID/echo-web.tar.gz .
tar -xvf echo-web.tar.gz
cd \echo-web
```

## Task 1: Create a Kubernetes Cluster 
```
gcloud container clusters create $CLUSTER_NAME --num-nodes 2 --zone $ZONE --machine-type $MACHINE_TYPE
```

## Task 2: Build a tagged Docker Image & Push the image to the Google Container Registry
```
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/$APP_NAME:v1 .
```

## Task 3: Deploy the application to the Kubernetes Cluster

3.1 Connect to the previously created cluster so deployment of application
```
gcloud container clusters get-credentials CLUSTER_NAME --zone $ZONE --project $DEVSHELL_PROJECT_ID
```
It will take approx 4-5 mints to create cluster so please be patience

3.2 Deploy the sample application image, which we created in the previous step
```
kubectl create deployment echo-web --image=gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1 --port=8000
```
3.3 Expose your deployment by creating a service
```
kubectl expose deployment echo-web --type LoadBalancer --port 80 --target-port 8000
```
