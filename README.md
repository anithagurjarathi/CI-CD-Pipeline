Getting started with creating NodeJs APP
A simple Node.js application that uses Docker and Kubernetes for deployment.

# Prerequisites
Make sure you have these installed:

Node.js: Install Node.js
Docker: Install Docker
Kubernetes (kubectl): Install kubectl

# Setup
## 1. Clone the Repo
Clone the project to your local machine:

git clone https://github.com/anithagurjarathi/nodejsproject.git
cd nodejsproject

## 2. Install Dependencies
Run the following to install required packages:

### npm install
Running the Application
To start the app locally:

### npm start
The app will run on http://localhost:3000.

Running Tests
### npm test
Dockerize the Application

## 1. Build Docker Image
Create the Docker image:

docker build -t nodejsproject .

## 2. Run Docker Container
Run the app inside a Docker container:
docker run -p 3000:3000 nodejsproject

## Deploy to Kubernetes
Apply the Kubernetes deployment:

kubectl apply -f k8s/deployment.yaml

Check if the deployment was successful
