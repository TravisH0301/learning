# Codefresh
Codefresh is a Continuous Integration (CI) and Continuous Deployment (CD) platform designed specifically for Docker and Kubernetes. 

- [Pipelines](#Pipelines)
  - [Pipeline Steps](#Pipeline-Steps)
  - [Pipeline Stages](#Pipeline-Stages)
- [Pipeline Examples](#Pipeline-Examples)

## Pipelines
A pipeline in Codefresh is a sequence of steps that are executed as part of the CI/CD process. 
The pipelines are configured using a YAML file, and each step in a pipeline is an isolated Docker container that performs a specific task. 
The pipelines can be used for:

- Compiling and packaging code
- Building Docker images
- Pushing Docker images to any Docker Registry
- Deploying applications/artifacts to VMs, Kubernetes clusters, FTP sites, S3 buckets, etc.
- Running unit tests, integration tests, acceptance tests, etc.
- Executing any custom action defined by the user

### Pipeline Steps
Codefresh offers the following built-in step types:

- Git clone steps: clones code in your pipeline from a Git repository.
- Freestyle steps: run any command within the context of a Docker container. (= docker run)
- Build steps: take a Dockerfile as input and build a Docker image. The built image is automatically pushed into the default Docker registry of the account. (= docker build)
- Push steps: push and tag your docker images, that are created by the build step, in any external Docker registry. (= docker tag, docker push)
- Composition steps: run multiple services together in the Codefresh infrastructure and execute unit tests or other commands against them. (= docker-compose)
- Launch test environment steps: behave similarly to compositions, but they persist after the pipeline ends. This can be used as a preview environment from a pull request.
- Deploy steps: perform Kubernetes deployments in a declarative manner. They refer to Continuous Deployment.
- Approval steps: pause pipelines and wait for human intervention before resuming. They refer to Continuous Delivery.

Additionally, all steps share the same workspace in the form of a shared Docker volume.

### Pipeline Stages
Pipelines steps can be grouped into stages to organise steps. 
Stages are executed in the order they are defined. 
By default, if a step fails, subsequent steps and stages will not run.

## Pipeline Examples
### Unit Test
    version: '1.0'
    stages:
      - main_clone
      - test
      
    steps:
      main_clone:
        title: Cloning demo repo
        type: git-clone
        stage: main_clone
        git: ${{GITHUB_INTEGRATION_NAME}}  # Integrated GitHub app
        repo: 'demo/demo-repo'
        working_directory: '${{CF_VOLUME_PATH}}'  # Shared volume path
        revision: '${{CF_REVISION}}'  # Git hash
        
      unit_test:
        title: Unit testing Python script  # Freestyle step doesn't require a type definition
        stage: test
        image: hub.artifactory.gcp.anz/python:3.8
        commands:
            - cd ./app
            - python -m unittest unit_test.py
