# Udagram Image Filtering Application

Udagram is a simple cloud application developed alongside the Udacity Cloud Engineering Nanodegree. It allows users to register and log into a web client, post photos to the feed, and process photos using an image filtering microservice.

The project is split into two parts:
1. Frontend - Angular web application built with Ionic Framework
2. Backend RESTful API - Node-Express application

## Getting Started
> _tip_: it's recommended that you start with getting the backend API running since the frontend web application depends on the API.
### Prerequisite
1. The depends on the Node Package Manager (NPM). You will need to download and install Node from [https://nodejs.com/en/download](https://nodejs.org/en/download/). This will allow you to be able to run `npm` commands.
2. Environment variables will need to be set. These environment variables include database connection details that should not be hard-coded into the application code. A file named `set_env.sh` has been prepared as an optional tool to help you configure these variables on your local development environment.

### Database
Create a PostgreSQL database either locally or on AWS RDS. Set the config values for environment variables prefixed with `POSTGRES_` in `set_env.sh`.

### S3
Create an AWS S3 bucket. Set the config values for environment variables prefixed with `AWS_` in `set_env.sh`.

### Backend API
* To download all the package dependencies, run the command from the directory `udagram-api/`:
    ```bash
    npm install .
    ```
* To run the application locally, run:
    ```bash
    npm run dev
    ```
* You can visit `http://localhost:8080/api/v0/feed` in your web browser to verify that the application is running. You should see a JSON payload. Feel free to play around with Postman to test the API's.

### Frontend App
* To download all the package dependencies, run the command from the directory `udagram-frontend/`:
    ```bash
    npm install .
    ```
* Install Ionic Framework's Command Line tools for us to build and run the application:
    ```bash
    npm install -g ionic
    ```
* Prepare your application by compiling them into static files.
    ```bash
    ionic build
    ```
* Run the application locally using files created from the `ionic build` command.
    ```bash
    ionic serve
    ```
* You can visit `http://localhost:8100` in your web browser to verify that the application is running. You should see a web interface.

### CI/CD
Continuous Integration & Deployment is realized via Docker/Dockerhub and Travis.CI.

You can find instructions on how to set up Docker on https://docs.docker.com/get-docker/. You will also have to create an account on dockerhub.com.

After creating an Account on travis-ci.org and syncing it to your Github-Repo, Travis will look for the travis.yml file placed in the root directory and execute the stated instructions. These Instructions include building the Docker-Images and pushing them to www.dockerhub.com, so please set up your Docker-Credentials as Environment Variables for the specific repo on travis-ci.org and tag the Docker-Images accordingly.

Kubectl is  configured and run bei Travis.CI via the travis.yml file to apply the corresponding Kubernetes-files placed in udagram/deployment/k8s. Kubernetes will then pull the newly-build images and deploy them to your Cluster automatically.

### A/B Testing
All Docker-Images are labeled to enable rolling-updates and A/B testing. For runnning two seperate versions of the same application, just adjust the tags accordingly before deployment. 

### Public Access

The services in kubernetes for the frontend as well as the reveseproxy (backend) are set up as type Load-Balancer. To make your app publicly accessible deploy the application to the cluster, then get the external-IP assigned to the reverseproxy and set it as the apps apiHost in /udagram-frontend/src/environments. Afterwards get the external IP for the frontend and set is as the URL-Variable in your environment variables (set_env.sh & docker-compose.yml). Commit and push your changes to Github so the CI/CD Pipeline will newly build and deploy the application, making it available at the respective public IP`s. 


## Tips
1. The `.dockerignore` file is included for your convenience to not copy `node_modules`. Copying this over into a Docker container might cause issues if your local environment is a different operating system than the Docker image (ex. Windows or MacOS vs. Linux).
2. It's useful to "lint" your code so that changes in the codebase adhere to a coding standard. This helps alleviate issues when developers use different styles of coding. `eslint` has been set up for TypeScript in the codebase for you. To lint your code, run the following:
    ```bash
    npx eslint --ext .js,.ts src/
    ```
    To have your code fixed automatically, run
    ```bash
    npx eslint --ext .js,.ts src/ --fix
    ```
3. Over time, our code will become outdated and inevitably run into security vulnerabilities. To address them, you can run:
    ```bash
    npm audit fix
    ```
4. In `set_env.sh`, environment variables are set with `export $VAR=value`. Setting it this way is not permanent; every time you open a new terminal, you will have to run `set_env.sh` to reconfigure your environment variables. To verify if your environment variable is set, you can check the variable with a command like `echo $POSTGRES_USERNAME`.