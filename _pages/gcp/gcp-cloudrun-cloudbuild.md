# GCP - CloudRun and Cloudbuild

## Quickstart for Docker
https://cloud.google.com/cloud-build/docs/quickstart-docker

### Log in to Google Cloud
Authorize the gcloud command-line tool to access your project:

gcloud auth login

Configure your project for the gcloud tool, where [PROJECT_ID] is your Google Cloud project ID that you created or selected in the previous section:

gcloud config set project [PROJECT_ID]

### Preparing source files
You'll need some sample source code to build. In this section, you'll create a simple Hello World example and a Dockerfile.

Create a file named quickstart.sh with the following contents:

#!/bin/sh
echo "Hello, world! The time is $(date)."
Create a file named Dockerfile with the following contents:

FROM alpine
COPY quickstart.sh /
CMD ["/quickstart.sh"]
Open a terminal window (if not already open).

Run the following command to make quickstart.sh executable:

chmod +x quickstart.sh

### Build using Dockerfile

Cloud Build allows you to build a Docker image using a Dockerfile. You don't require a separate build config file.

Run the following command from the directory containing quickstart.sh and Dockerfile, where [PROJECT_ID] is your Google Cloud project ID:

gcloud builds submit --tag gcr.io/[PROJECT_ID]/quickstart-image .

### Build using a build config file
In this section you will use a build config file to build the same Docker image as above. The build config instructs Cloud Build to perform tasks based on your specifications.

In the same directory that contains quickstart.sh and the Dockerfile, create a file named cloudbuild.yaml with the following contents. This file is your build config file. At build time, Cloud Build automatically replaces $PROJECT_ID with your project ID.

steps:
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/quickstart-image', '.' ]
images:
- 'gcr.io/$PROJECT_ID/quickstart-image'
Start the build by running the following command:

gcloud builds submit --config cloudbuild.yaml .

You've just built quickstart-image using the build config file and pushed the image to Container Registry.
See output on https://console.cloud.google.com/cloud-build and you can see the image path.

