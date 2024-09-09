# Guide to setting up a CI/CD pipeline

## Table of content

- [Introduction](#introduction)
- [Setting up GitHub Actions Runners](#setting-up-github-actions-runners)
- [Creating a workflow file](#creating-a-workflow-file)
- [Running tests](#running-tests) 
- [Building and deploying your code](#building-and-deploying-your-code)
- [Conclusion](#conclusion)


## Introduction

This guide will walk you through setting up a CI/CD pipeline for your project. We will be using GitHub Actions as the CI/CD tool. GitHub Actions is a CI/CD tool that is built into GitHub. It allows you to automate your workflow and build, test, and deploy your code directly from your GitHub repository. This guide will cover the following topics:

- Setting up GitHub Actions Runners
- Creating a workflow file
- Running tests
- Building and deploying your code

## Setting up GitHub Actions Runners

GitHub Actions Runners are the machines that run your workflows. You can use GitHub-hosted runners or self-hosted runners. GitHub-hosted runners are virtual machines that are hosted by GitHub, and you can use them for free. Self-hosted runners are machines that you set up and manage yourself. You can use self-hosted runners if you need more control over the environment or if you need to run workflows on machines that are not supported by GitHub-hosted runners. In this guide, we will be using GitHub-hosted runners OR, Github self-hosted runner.

To setup self-hosted runner, follow the steps below:

1. Go to your GitHub repository.
2. Click on the "Settings" tab.
3. Click on "Actions" in the left sidebar.
4. Click on "Runners" in the left sidebar.
5. Click on "New self-hosted runner".
6. Follow the instructions from the page to add new self-hosted runner. (ps. You might need to setup a new runner on your machine like ec2 instance)
7. Choose the operating system and architecture that you want to use for the runner. You can choose from Windows, macOS, and Linux.
8. Download the runner package for your operating system and architecture. You can just follow the instructions from "Download" section.
9. Configure the runner by running the configuration script. You can just follow the instructions from "Configure" section.

## Creating a workflow file

To create a workflow file, follow the steps below:

1. Go to your GitHub repository.
2. Click on the "Actions" tab.
3. Click on "New workflow".
4. Choose a template for your workflow. You can choose from a list of templates or start from scratch.
5. The file shohuld be saved in `.github/workflows` directory.

Here is an example of a workflow file that runs tests on every push to the main branch:

```yaml
name: Project Stage CICD
on:
    workflow_call:
    workflow_dispatch:
    push:
       branches: [develop]

jobs:
  build:
    runs-on: self-hosted
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: "Make .env file"
        run: |

            touch .env
            echo DEBUG=${{ secrets.DEBUG }} >> .env 
            echo SECRET_KEY="${{ secrets.SECRET_KEY }}" >> .env
            echo ALLOWED_HOSTS="${{ secrets.ALLOWED_HOSTS }}" >> .env

      - name: "Build the Docker image"
        run: "docker compose -f ./docker-compose.stage.yml build"

      - name: "Build the stack"
        run: |
            docker compose -f ./docker-compose.stage.yml down || true
            docker compose -f ./docker-compose.stage.yml up -d
            docker rmi -f $(docker images -f "dangling=true" -q) || true
            docker compose -f ./docker-compose.stage.yml exec api python manage.py migrate
      
      - name: "Run collectstatic"
        run: "docker compose -f ./docker-compose.stage.yml exec api python manage.py collectstatic --noinput"
```

This workflow file runs tests on every push to the main branch. It checks out the code, creates a .env file, builds the Docker image, builds the stack, and runs collectstatic.

Here is an example of a workflow file that runs tests on every pull request:

```yaml
name: Project Stage CICD
on:
  workflow_call:
  workflow_dispatch:
  pull_request:
    branches: [develop]

jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      image_id: ${{ steps.build_image.outputs.image_id }}
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: "Make .env file"
        run: |
          touch .env
          echo DEBUG=${{ secrets.DEBUG }} >> .env 
          echo SECRET_KEY="${{ secrets.SECRET_KEY }}" >> .env
          echo ALLOWED_HOSTS="${{ secrets.ALLOWED_HOSTS }}" >> .env

      - name: "Build the Docker image"
        id: build_image
        run: |
          docker-compose -f ./docker-compose.stage.yml build
          echo "::set-output name=image_id::$(docker images -q <your-image-name>)"

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: "Run tests"
        run: |
          docker-compose -f ./docker-compose.stage.yml exec api python manage.py test

```

This workflow file runs tests on every pull request. It checks out the code, creates a .env file, and runs tests.

## Running tests

To run tests, follow the steps below:

1. Go to your GitHub repository.
2. Click on the "Actions" tab.
3. Click on the workflow that you want to run.
4. Click on "Run workflow".

This will run the workflow and display the results in the Actions tab.

## Building and deploying your code

To build and deploy your code, follow the steps below:

1. Go to your GitHub repository.
2. Click on the "Actions" tab.
3. Click on the workflow that you want to run.
4. Click on "Run workflow".

This will run the workflow and build and deploy your code.

## Conclusion

In this guide, we have covered setting up a CI/CD pipeline for your project using GitHub Actions. We have covered setting up GitHub Actions Runners, creating a workflow file, running tests, and building and deploying your code. By following this guide, you should be able to set up a CI/CD pipeline for your project and automate your workflow.
