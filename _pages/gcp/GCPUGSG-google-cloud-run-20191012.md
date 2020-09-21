# Google Cloud Run
## Serverless Platform for Containers
- Cloud Run -(Fully managed)
  Websites, API endpoint, Mobile backend, webhook

- Cloud Run for Anthos
  Microservices, Async tasks

  https://github.com/as-a-service

  Read : https://cloud.google.com/anthos/
  
  Read functions vs appengine vs cloud run


  Quiklabs credit - 1s-singapore-9185

## Labs
- http://bit.ly/study-jam-19
- bit.ly/cloud-run-1
- bit.ly/cloud-run-2

## Hello Cloud Run
Cloud Run is a managed compute platform that enables you to run stateless containers that are invocable via HTTP requests. Cloud Run is serverless: it abstracts away all infrastructure management, so you can focus on what matters most — building great applications.

Knative, letting you choose to run your containers either fully managed with Cloud Run, or in your Google Kubernetes Engine cluster with Cloud Run on GKE.

The goal of this lab is for you to build a container image and deploying it to Cloud Run. In this lab, you'll learn how to get started with Cloud Run by deploying and running a stateless container serverless-ly (with the infrastructure abstracted away). Cloud Run offers a fully-managed option as well as the ability to run on top of a GKE cluster.

### Enable the Cloud Run API
From Cloud Shell, enable the Cloud Run API :
```
gcloud services enable run.googleapis.com
```

### Write the sample application
First, you will build a simple express-based NodeJS application responding to HTTP requests.

In Cloud Shell create a new directory named helloworld-nodejs, then change into that directory:

mkdir helloworld-nodejs
cd helloworld-nodejs

Create a package.json file, then add the following content to it:

{
  "name": "cloudrun-helloworld",
  "version": "1.0.0",
  "description": "Simple hello world sample in Node",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "author": "",
  "license": "Apache-2.0",
  "dependencies": {
    "express": "^4.16.4"
  }
}

Most importantly, the file above contains a start script command and a dependency on the Express web application framework.

Next, in the same directory, create a index.js file, and copy the following lines into it:

const express = require('express');
const app = express();

app.get('/', (req, res) => {
  console.log('Hello world received a request.');

  const target = process.env.TARGET || 'World';
  res.send(`Hello ${target}!`);
});

const port = process.env.PORT || 8080;
app.listen(port, () => {
  console.log('Hello world listening on port', port);
});

This code creates a basic web server that listens on the port defined by the PORT environment variable. Your app is now finished and ready to be containerized and uploaded to Container Registry.


Containerize your app and upload it to Container Registry
To containerize the sample app, create a new file named Dockerfile in the same directory as the source files, and add the following content:

# Use the official Node.js 10 image.
# https://hub.docker.com/_/node
FROM node:10

# Create and change to the app directory.
WORKDIR /usr/src/app

# Copy application dependency manifests to the container image.
# A wildcard is used to ensure both package.json AND package-lock.json are copied.
# Copying this separately prevents re-running npm install on every code change.
COPY package*.json ./

# Install production dependencies.
RUN npm install --only=production

# Copy local code to the container image.
COPY . .

# Run the web service on container startup.
CMD [ "npm", "start" ]

### Get your Project ID by running the following, you'll need it for the next step:

gcloud config get-value project

Now, build your container image using Cloud Build by running the following command from the directory containing the Dockerfile, adding your Project-ID from the last output:

gcloud builds submit --tag gcr.io/[PROJECT-ID]/helloworld

Cloud Build is a service that executes your builds on GCP. It executes a series of build steps, where each build step is run in a Docker container to produce your application container (or other artifacts) and push it to Cloud Registry, all in one command.

Once pushed to the registry, you will see a SUCCESS message containing the image name (gcr.io/[PROJECT-ID]/helloworld). The image is stored in Container Registry and can be re-used if desired.

List all the container images associated with your current project using this command :

gcloud container images list

To run and test the application locally from Cloud Shell, start it using this standard docker command:

docker run -d -p 8080:8080 gcr.io/[PROJECT-ID]/helloworld
In the Cloud Shell window, click on Web preview and select "Preview on port 8080".

This should open a browser window showing the "Hello World!" message. You could also simply use curl localhost:8080.

### Deploy to Cloud Run
Deploying your containerized application to Cloud Run is done using the following command adding your Project-ID:

gcloud beta run deploy --image gcr.io/[PROJECT-ID]/helloworld

When prompted:

select Cloud Run (fully managed)
select your region ( us-central1)
confirm the service name by pressing Enter
respond y to allow unauthenticated invocations (that last step is important and can also be avoided by using the --allow-unauthenticated deploy option).
Wait a few moments until the deployment is complete.

On success, the command line displays the service URL:

Service [helloworld] revision [helloworld-00001] has been deployed
and is serving traffic at https://helloworld-wdl7fdwaaa-uc.a.run.app
```
google5606274_student@cloudshell:~/helloworld-nodejs (qwiklabs-gcp-3eddffbbeb58e766)$ gcloud beta run deploy --image gcr.io/qwiklabs-gcp-3eddffbbeb58e766/helloworld
Please choose a target platform:
 [1] Cloud Run (fully managed)
 [2] Cloud Run for Anthos deployed on GKE
 [3] Cloud Run for Anthos deployed on VMware
 [4] cancel
Please enter your numeric choice:  1
To specify the platform yourself, pass `--platform managed`. Or, to make this the default target platform, run `gcloud config set run/platform managed`.
Please specify a region:
 [1] asia-northeast1
 [2] europe-west1
 [3] us-central1
 [4] us-east1
 [5] cancel
Please enter your numeric choice:  3
To make this the default region, run `gcloud config set run/region us-central1`.
Service name (helloworld):
Allow unauthenticated invocations to [helloworld] (y/N)?  y
Deploying container to Cloud Run service [helloworld] in project [qwiklabs-gcp-3eddffbbeb58e766] region [us-central1]
✓ Deploying new service... Done.                                   
  ✓ Creating Revision...
  ✓ Routing traffic...
  ✓ Setting IAM Policy...
Done.
Service [helloworld] revision [helloworld-ghcxf] has been deployed and is serving 100 percent of traffic at https://helloworld-4w47mgqbrq-uc.a.run.app
```
You can now visit your deployed container by opening the service URL in any browser window:

### Remove Image
```
gcloud container images delete gcr.io/[PROJECT-ID]/helloworld
```

### Remove Cloud Run
```
google5606274_student@cloudshell:~/helloworld-nodejs (qwiklabs-gcp-3eddffbbeb58e766)$ gcloud beta run services delete helloworld
Please choose a target platform:
 [1] Cloud Run (fully managed)
 [2] Cloud Run for Anthos deployed on GKE
 [3] Cloud Run for Anthos deployed on VMware
 [4] cancel
Please enter your numeric choice:  1

To specify the platform yourself, pass `--platform managed`. Or, to make this the default target platform, run `gcloud config set run/platform managed`.

Please specify a region:
 [1] asia-northeast1
 [2] europe-west1
 [3] us-central1
 [4] us-east1
 [5] cancel
Please enter your numeric choice:  3

To make this the default region, run `gcloud config set run/region us-central1`.

Service [helloworld] will be deleted.

Do you want to continue (Y/n)?  y

Deleted service [helloworld].
```

## Cloud Run on GKE
Create a GKE cluster with Cloud Run enabled
Navigate to the Google Kubernetes Engine (GKE) section in the Google Cloud Console, Navigation menu > Kubernetes Engine.

Click Create cluster to open the Create a Kubernetes cluster page.

On the cluster template, set the following values:

Name of custer: run-gke

Location type: zonal

Select a zone that is near you. This lab is using us-central1-a

Master version: you must use version 1.12.8-gke.6 or above, 1.13.6-gke.6 or above.

cloudrun_cluster1.png

For the default-pool

Number of nodes: 3

Machine type: 4 vCPUs

Click More Options to continue.

Under Security, change the Access scopes choose Allow full access to all Cloud APIs.

cloudrun_security.png

Click Save.

Click the Availability, networking, security, and additional features link to expand the form.

Select Enable Stackdriver kubernetes Engine Monitoring.
Select the Enable istio (beta).
Select Enable Cloud Run on GKE (beta).
Then click Create.

cloudrun_cluster_addl2.png

You will create a cluster with this configuration:

Cloud Run on GKE enabled
Kubernetes version, default -1.12.8-gke.6 in GCP environment (see recommended versions for other choices)
Zonal cluster in us-central1-a zone. You can use regional as well for the location type with Cloud Run on GKE.
Nodes with 4 vCPU (3 nodes)
Scopes to access cloud-platform, write to logging, write to monitoring
You can also build a container from the command line: gcloud beta container --project "YOUR_PROJECT_ID" clusters create "YOUR_CLUSTER" \ --zone "us-central1-a" \ --no-enable-basic-auth \ --cluster-version "1.12.8-gke.6" \ --machine-type "n1-standard-4" \ --image-type "COS" \ --disk-type "pd-standard" \ --disk-size "100" \ --metadata disable-legacy-endpoints=true \ --scopes "https://www.googleapis.com/auth/cloud-platform" \ --num-nodes "3" \ --enable-stackdriver-kubernetes \ --no-enable-ip-alias \ --network "projects/YOUR_PROJECT_ID/global/networks/default" \ --subnetwork "projects/YOUR_PROJECT_ID/regions/us-central1/subnetworks/default" \ --addons HorizontalPodAutoscaling,HttpLoadBalancing,Istio,CloudRun \ --istio-config auth=MTLS_PERMISSIVE --enable-autoupgrade \ --enable-autorepair Output: NAME LOCATION MASTER_VERSION MASTER_IP MACHINE_TYPE NODE_VERSION NUM_NODES STATUS run-gke us-central1-a 1.12.8-gke.6 104.155.172.65 n1-standard-4 1.12.8-gke.6 3 RUNNING
Creating the Cloud Run-enabled cluster will take a few moments. Please wait for the cluster to be ready before moving to the next step.

### Deploy to Cloud Run on GKE
To deploy a container to the cluster you just created, go to Navigation menu > Cloud Run then click Create service.

Use these settings for your service:

Enter gcr.io/cloudrun/hello as the sample container image
Give the service a name: hello-run-gke
In the Location dropdown, select the cluster you just created
Use default namespace
Keep External as the Connectivity choice
cloudrun_service.png

Click Create and wait for the deployment to finish

When you see the green checkmark you have successfully created the service and deployed it to Cloud Run on GKE:

162ea37342ae880c.pngThe URL listed on the top. This will be needed to test the deployed service in the next and final step.

Accessing your deployed service
Once you've deployed your service, you can use curl to send a request and verify the service is working, using the cluster's IP address.

To avoid having to setup DNS, you'll test the deployed service by sending a request to the Istio ingress gateway (Knative is built using Istio) with the target host that should handle the request as an HTTP header. That hostname should be the URL listed in the previous deployment step and of the form: http://*service-name*.*namespace*.example.com

From the Console, go back to Kubernetes Engine section and click Services in the left navigation panel to display the list of services.

Scroll down to the istio-ingressgateway service and copy the IP address shown next to the load balancer. Ignore the other values under that IP address:

94178e48703304bd.png

From Cloud Shell, use curl to access the service . Replace [YOUR-IP] with the IP address you obtained in the previous step, and if you used a service name other than "hello-run-gke" you'll need to replace that as well.

curl -v -H "Host: hello-run-gke.default.example.com" http://[YOUR-IP]

With in the top section of the response should be HTTP 200 along with the default "Congratulations | Cloud Run" HTML content :

* Rebuilt URL to: http://173.255.129.211:80/
*   Trying 173.255.129.211...
* TCP_NODELAY set
* Connected to 173.255.129.211 (173.255.129.211) port 80 (#0)
> GET / HTTP/1.1
> Host: hello-run-gke.default.example.com
> User-Agent: curl/7.52.1
> Accept: */*
>
< HTTP/1.1 200 OK
< content-type: text/html; charset=utf-8

<...>

<title>Congratulations | Cloud Run</title>
...

  <p>Follow a tutorial to build a "Hello World" application in your favorite language into a container image, and then deploy it to Cloud Run:</p>

Clean up
Qwiklabs will take care of shutting down all the resources we've used so far, but here's what you would need to do on your own environment to save on cost and to be a good cloud citizen.

While Cloud Run on GKE is in beta, you must delete the cluster if you wish to stop the Cloud Run on GKE components from running. This will permanently delete workloads in the cluster and all other cluster state.

To delete the cluster, go to the GKE section in the console, select the cluster and click delete.