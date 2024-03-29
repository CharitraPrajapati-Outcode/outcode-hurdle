# Principles of CI/CD

## Introduction

Continuous Integration/Continuous Deployment (CI/CD) is a method to frequently deliver apps to customers by introducing automation into the stages of app development. The main concepts attributed to CI/CD are continuous integration, continuous delivery, and continuous deployment.

1. **Continuous Integration (CI)**: CI is a development practice where developers integrate code into a shared repository frequently, preferably several times a day. Each integration can then be verified by an automated build and automated tests. The goal of CI is to catch and address bugs quicker, improve software quality, and reduce the time to validate and release new software updates.

2. **Continuous Delivery (CD)**: Continuous delivery takes CI a step further by deploying all code changes to a testing environment and/or a production environment after the build stage. This ensures that you have a deployable build artifact that’s tested and ready for release. CD automates the delivery of applications to selected infrastructure environments.

3. **Continuous Deployment (CD)**: This is a step further than Continuous Delivery. With this practice, every change that passes all stages of your production pipeline is released to your customers. There’s no human intervention, and only a failed test will prevent a new change to be deployed to production.

## Why is CI/CD important?

CI/CD introduces ongoing automation and continuous monitoring throughout the lifecycle of apps, from integration and testing phases to delivery and deployment. Taken together, these connected practices are often referred to as a CI/CD pipeline and are supported by development and operations teams working together in an Agile way with either a DevOps or Site Reliability Engineering (SRE) approach.

The core principles of CI/CD are:

- **Developers should merge their changes as often as possible**, rather than batching them up for end-of-day commits. This helps to avoid integration conflicts.

- **Automate everything**+. From integration to testing and deployment, automation ensures consistency and speed.

- **Build a testing culture**+. CI/CD relies heavily on automated testing, so that you can find and fix problems as early as possible.

- **Deploy with repeatability**+. You should be able to redeploy the same build (with the same behavior) to any environment, from a developer's test environment to production.

- **Keep the build fast**+. This ensures that if something goes wrong, you can identify and fix the problem quickly.

- **Build visibility**+. Everyone involved in creating software should be able to see the state of the new changes.

- **Automate deployment**+. This makes deployments reliable and repeatable, and you can do deployments at any time.

- **Self-testing builds**+. Every change should be tested, to catch issues as early as possible.

- **Everyone is responsible for the build**+. If the build or tests fail, getting them back to a working state should be the team's top priority.

By following these principles, you can create a more efficient, reliable, and effective development process.

