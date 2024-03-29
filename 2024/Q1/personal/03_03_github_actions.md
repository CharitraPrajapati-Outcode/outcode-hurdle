# GitHub Actions

## Introduction

GitHub Actions is a CI/CD service provided by GitHub. It allows you to automate, customize, and execute your software development workflows right in your GitHub repository. You can discover, create, and share actions to perform any job you'd like, including CI/CD, and combine actions in a completely customized workflow.

## Key Concepts

### Workflows

A workflow is a configurable automated process that you set up in your repository to build, test, package, release, or deploy any project on GitHub. Workflows are made up of one or more jobs and can be scheduled or triggered by an event.

### Events

An event is a specific activity that triggers a workflow run. GitHub webhooks handle the event delivery system. An event can be a push to a repository, a pull request, a fork, etc.

### Jobs

A job is a set of steps that execute on the same runner. By default, a workflow with multiple jobs will run those jobs in parallel. You can also configure a workflow to run jobs sequentially.

### Steps

A step is an individual task that can run commands in a job. A step can be either an action or a shell command. Each step in a job executes on the same runner, allowing the steps to share data with each other.

### Actions

Actions are the smallest portable building block of a workflow. You can create your own actions, or use and customize actions shared by the GitHub community.

### Getting Started

To get started with GitHub Actions, you need to create a .github/workflows directory in your repository. Then, create a new YAML file within this directory to define your workflow (e.g., .github/workflows/main.yml).

Here's an example of a simple GitHub Actions workflow that runs tests for a Node.js application whenever code is pushed to the repository:

```yaml
name: Django CI

on:
    push:
        branches: [ main ]

jobs:
    build:
        # runs-on: ubuntu-latest
        runs-on: self-hosted

        steps:
        - name: Checkout repository
          uses: actions/checkout@v3

        - name: Make .env file
          run: |
            touch .env
            echo DEBUG=${{ secrets.DEBUG }} >> .env 
            echo DJANGO_SECRET_KEY="${{ secrets.DJANGO_SECRET_KEY }}" >> .env
            echo ALLOWED_HOSTS="${{ secrets.ALLOWED_HOSTS }}" >> .env
            echo DB_NAME="${{ secrets.DB_NAME }}" >> .env
            echo DB_USER="${{ secrets.DB_USER }}" >> .env
            echo DB_PASSWORD="${{ secrets.DB_PASSWORD }}" >> .env
            echo DB_HOST="${{ secrets.DB_HOST }}" >> .env
            echo DB_PORT="${{ secrets.DB_PORT }}" >> .env

        - name: Build the docker image
          run: docker compose build

        - name: Clean up docker images
          run: docker rmi -f $(sudo docker images -f "dangling=true" -q) || true
        
        - name: Run migration
          run: docker compose run web python manage.py migrate

        - name: Collect static files
          run: docker compose run web python manage.py collectstatic --noinput
```

### More Information

For more detailed information, check out the [GitHub Actions documentation](https://docs.github.com/en/actions). It includes a comprehensive guide to getting started, as well as detailed reference information for all aspects of GitHub Actions.

## Steps in summary

1. **Setup a self-hosted runner**: Go to the repository on GitHub, click on `Settings`, then click on `Actions` and then click on `Runners`. Click on `New self-hosted runner` and follow the instructions to set up the runner on the EC2 instance.

2. **Create a workflow file**: Create a new file in the .github/workflows directory of your repository. This file should be a YAML file that defines your workflow.

3. **Define the workflow**: Define the workflow using the YAML syntax. You can specify the events that trigger the workflow, the jobs that run in the workflow, and the steps that make up each job.

4. **Commit and push the workflow file**: Once you've defined your workflow, commit and push the workflow file to your repository. This will trigger the workflow to run based on the events you specified.

5. **Monitor the workflow**: After pushing the workflow file, you can monitor the workflow runs in the Actions tab of your repository. You can see the status of each workflow run, view the logs for each job, and troubleshoot any issues that arise.

** More Information (Optional)**

6. **Iterate and improve**: As you gain more experience with GitHub Actions, look for ways to improve your workflows. This might include speeding up build times, improving test coverage, or automating additional tasks.

7. **Explore the GitHub Actions marketplace**: GitHub Actions has a marketplace where you can discover, create, and share actions to perform any job you'd like. You can find actions for a wide variety of tasks, including building, testing, deploying, and more.
