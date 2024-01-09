Deploy to Amazon ECR and EKS GitHub Action
This GitHub Action automates the deployment process of a Docker container to Amazon Elastic Container Registry (ECR) and subsequently deploys the containerized application to an Amazon Elastic Kubernetes Service (EKS) cluster. The workflow is triggered on pushes to the master branch or when pull requests are merged into the master branch.

Workflow Configuration
Triggers
Push: The workflow is triggered on every push to the master branch.
Pull Request: The workflow is triggered whenever a pull request is merged into the master branch.
Environment Variables
AWS_REGION: The AWS region where the ECR repository and EKS cluster are located (e.g., ap-south-1).
ECR_REPOSITORY: The name of your Amazon ECR repository.
EKS_CLUSTER_NAME: The name of your EKS cluster.
Secrets
AWS_ACCESS_KEY_ID: Secret key for AWS authentication.
AWS_SECRET_ACCESS_KEY: Secret key for AWS authentication.
Workflow Steps
Checkout: Clones the repository to the GitHub Actions runner.
Configure AWS Credentials: Configures AWS credentials using the provided access key, secret key, and region.
Login to Amazon ECR: Uses AWS CLI to log in to Amazon ECR with the configured credentials.
Build, Tag, and Push Image to Amazon ECR:
Builds a Docker container from the source code.
Tags the container with the GitHub commit SHA.
Pushes the Docker container to the specified Amazon ECR repository.
Update kubeconfig: Updates the kubeconfig file with the credentials for the specified EKS cluster.
Deploy to EKS:
Replaces the placeholder DOCKER_IMAGE in the git-deployment.yml file with the ECR repository and image tag.
Applies the updated Kubernetes deployment and service configurations to the EKS cluster.
Note
Make sure to have the necessary AWS credentials set up as secrets in your GitHub repository (AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY).
Ensure that your Kubernetes manifests (git-deployment.yml and service.yml) are correctly configured for your application.
Example Usage:
name: Deploy to Amazon ECR and EKS

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

env:
  AWS_REGION: ap-south-1
  ECR_REPOSITORY: rahulecr95
  EKS_CLUSTER_NAME: test-eks

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    environment: github_credentials

    steps:
      # ... (steps mentioned in the original workflow)
Make sure to adapt the workflow to your specific requirements and customize the Kubernetes manifests according to your application's needs.
