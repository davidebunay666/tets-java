# Test CI/CD DEVOPS

This repository contains a CI/CD pipeline example for testing, building, packaging, and deploying a Java application on a Kubernetes cluster.

## Getting Started

### Prerequisites
- GitLab CI/CD platform
- `gitlab-ci.yml` file
- Configured CI/CD environment variables

## Description
The CI/CD pipeline consists of six stages configured in the `.gitlab-ci.yml` file. The pipeline utilizes CI/CD variables to store Kubernetes connection credentials, database credentials, Docker Hub registry credentials, namespace, and deployment names for Kubernetes.

The CI process includes the following stages:
- **test**: Executes SAST scanning, secrets detection, and dependency scanning.
- **unit-test**: Runs unit tests, saving artifacts at `sourceCode/target/surefire-reports/`.
- **coverage**: Fails the job if code coverage is less than 30%.
- **build**: Compiles Java code and saves the artifact (`*.jar`) at `sourceCode/target/`.
- **build-image**: Creates a Docker image based on `Dockerfile` and pushes it to Docker Hub registry.

The CD process includes one stage:
- **deploy**: Reads the `KUBECONFIG` CI/CD variable for Kubernetes cluster credentials. It then reads the `deployment.yml` manifest located in `integrationDeployment/manifests` to create Kubernetes components:
  - deployment
  - service
  - hpa
  - secret (containing database credentials in base64)
  - configMap (created from files in `integrationDeployment/configuration/devConfig`)

## Kubernetes Components
- **deployment**: Includes a readiness health check on port 8000 and endpoint `/api/users` for service readiness.
- **service**: LoadBalancer type to handle external requests.
- **hpa**: Configured for minimum 2 replicas, maximum 5 replicas, and scaling when CPU usage reaches 60%.
- **secret**: Stores database credentials encoded in base64, fetched from CI/CD variables (`USER_DB` and `PASS_DB`).
- **configMap**: Created from files located in `integrationDeployment/configuration/devConfig`.

## Usage

The ci/cd pipelines is executed on public  gitlab repository
[https://gitlab.com/davidebunay666/tets-devops/-/pipelines](https://gitlab.com/davidebunay666/tets-devops/-/pipelines)
The application image is deployed to a public Docker Hub repository on each commit to the main branch. You can pull the image using:
**docker pull davidbunay66/test-java:1.0**

The application is deployed on an AWS Kubernetes cluster and accessible externally at:
[http://44.202.237.68:31245/api/users](http://44.202.237.68:31245/api/users)

To access the Swagger UI for testing the application, use:
[http://44.202.237.68:31245/api/swagger-ui/index.html](http://44.202.237.68:31245/api/swagger-ui/index.html)

## Authors and Acknowledgments

- David Bunay Moncayo

## License

For open-source projects

