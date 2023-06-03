# devsecops-platform-github-workflows

A collection of workflows to be used in Github Actions to build and deploy Docker Images that are scanned by SecEng Scanners and Tools.


# DevSecOps Workflow for Dockerized Apps

* Implemented as a Github Actions Workflow
* Builds a Dockerized application using BuildX and Docker Caches
* Deploys the built Docker Image to the specified Viasat Artifactory Docker Registry
* Scans the built Docker Image using Prisma  and provides summaries and links about the Scan result


```yaml
# A regular name of the job
name: docker-image-devsecops-workflow

# The single job specification that the pipeline will execute
jobs:
  docker-devsecops:
    name: 🐳 docker
 
    # Speficifcation of the workflow with at a specific version (SHA, Branch or Tag)
    uses: seceng-devsecops-platform/devsecops-platform-github-workflows/.github/workflows/docker-compose-devsecops-workflow.yaml@v0.1.0

    # Properties required
    with:
      seceng-devsecops-dind-runner-tag: "The github action runner tag to run the workflow in"
      docker-compose-context: "The directory where the a docker compose file lives for the build"
      docker-compose-file: "The name of the file used `docker-compose.yaml`"
      docker-compose-service: "The name of the the service within the docker-compose file that contains the image definition"
      seceng-devsecops-prisma-domain: "The optional value of the Prisma/Twistlock Server where to send scan results"
      seceng-devsecops-prisma-project: "The name of the Prisma Project to flow " 

    # Pass the secrets
    secrets:
      # DevSecOps Platform: Scanners and Tools: Prisma Cloud Requirements
      SECENG_DEVSECOPS_CLIENT_PRISMA_USERNAME: ${{ secrets.SECENG_DEVSECOPS_CLIENT_PRISMA_USERNAME }}
      SECENG_DEVSECOPS_CLIENT_PRISMA_PASSWORD: ${{ secrets.SECENG_DEVSECOPS_CLIENT_PRISMA_PASSWORD }}

      # GI-Apps Platform: Artifactory Docker Registry
      ARTIFACTORY_SVC_USER: ${{ secrets.ARTIFACTORY_SVC_USER }}
      ARTIFACTORY_SVC_PASS: ${{ secrets.ARTIFACTORY_SVC_PASS }}

      # GI-Apps Platform: Slack Channels
      SLACK_CHANNEL_AUTOMATION_ID: ${{ secrets.SLACK_CHANNEL_AUTOMATION_ID }}
      SLACK_CHANNEL_AUTOMATION_TOKEN: ${{ secrets.SLACK_CHANNEL_AUTOMATION_TOKEN }}
```

## Azure ChatGPT SPA Service Example

* https://git.viasat.com/Harmony/azure-chatgpt-spa-service/blob/develop/.github/workflows/docker-image-devsecops-workflow.yaml

![Screenshot 2023-04-07 at 6 23 13 PM](https://media.git.viasat.com/user/6318/files/480bbb86-5b3b-432d-bd0c-39ccf7f6d906)


```yaml
####
#### Author: Marcello DeSales (@mdesales)
####
# https://www.freecodecamp.org/news/a-lightweight-tool-agnostic-ci-cd-flow-with-github-actions/
# This is a composable github Actions Workflow that reuses the SecEng DevSecOps Platform workflow for
# Dockerized apps written with Docker Compose.
name: docker-image-devsecops-workflow

on:
  push:
    branches:
      - develop
      - feature/**
      - bugfix/**

    # https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#example-including-paths
    paths:
      - '.github/**'
      - 'components-*/**'
      - 'pages/**'
      - 'public/**'
      - 'styles/**'
      - 'types/**'
      - 'utils/**'
      - '.dockerignore'
      - '.gitignore/**'
      - '.npmrc'
      - 'bootstrap.js'
      - 'docker-compose.yml'
      - 'Dockerfile'
      - '*.ts'
      - '*.js'
      - '*.json'

# https://faun.pub/building-a-ci-cd-pipeline-with-github-actions-and-docker-part-1-a9d8709c31fb
jobs:

  docker-devsecops:
    name: 🐳 docker

    # Updating the pipeline
    # https://docs.github.com/en/enterprise-server@3.5/actions/using-workflows/reusing-workflows#passing-inputs-and-secrets-to-a-reusable-workflow
    uses: seceng-devsecops-platform/devsecops-platform-github-workflows/.github/workflows/docker-compose-devsecops-workflow.yaml@feature/support-docker-compose-devsecops-workflow

    with:
      seceng-devsecops-dind-runner-tag: dind
      docker-compose-file: docker-compose.yml
      docker-compose-context: .
      docker-compose-service: chatgpt-spa-runtime

      seceng-devsecops-prisma-domain: twistlock.infosec.viasat.io
      seceng-devsecops-prisma-project: Container Services

    # Pass the secrets
    # https://docs.github.com/en/enterprise-server@3.5/actions/using-workflows/reusing-workflows#passing-inputs-and-secrets-to-a-reusable-workflow
    secrets:
      # Secrets to authenticate to SecEng Prisma server
      SECENG_DEVSECOPS_CLIENT_PRISMA_USERNAME: ${{ secrets.SECENG_DEVSECOPS_CLIENT_PRISMA_USERNAME }}
      SECENG_DEVSECOPS_CLIENT_PRISMA_PASSWORD: ${{ secrets.SECENG_DEVSECOPS_CLIENT_PRISMA_PASSWORD }}
      ARTIFACTORY_SVC_USER: ${{ secrets.ARTIFACTORY_SVC_USER }}
      ARTIFACTORY_SVC_PASS: ${{ secrets.ARTIFACTORY_SVC_PASS }}
      SLACK_CHANNEL_AUTOMATION_ID: ${{ secrets.SLACK_CHANNEL_AUTOMATION_ID }}
      SLACK_CHANNEL_AUTOMATION_TOKEN: ${{ secrets.SLACK_CHANNEL_AUTOMATION_TOKEN }}
```
