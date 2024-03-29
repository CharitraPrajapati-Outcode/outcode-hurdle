# Hosting Django Application in AWS EC2 Instance

## Introduction

Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers. Amazon EC2’s simple web service interface allows you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and lets you run on Amazon’s proven computing environment.

In this guide, we will host a Django application in an AWS EC2 instance.

## Prerequisites

- AWS account
- Django application
- SSH key pair
- Git

## Steps

1. Launch an EC2 instance
2. Configure OS for running application
3. Clone the project from the git repository
4. Run the application


## Launch an EC2 instance

1. Log in to the AWS Management Console.

2. On search bar, type `EC2` and click on the `EC2` service.

3. On the left-hand side, click on `Instances`.

4. Click on `Launch instances`.

5. Select desired configuration for the instance.

## Configure OS for running application

1. Update the package list and install the latest version of all the packages.

    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    ```

2. Docker engine installation

    - Reference: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

    - Install Docker Engine

3. Git installation

    - Reference: 
        - [Install Git on Ubuntu](https://git-scm.com/download/linux)
        - [How to install Git](https://phoenixnap.com/kb/how-to-install-git-on-ubuntu)

    - Install Git

4.  Configure SSH key pair for connecting git repository

    - Create a new SSH key pair in your local machine
    - Add the SSH key to your GitHub account

Now, you can use the SSH key to connect to the git repository and clone the project.

## Clone the project from the git repository

1. Open terminal

2. Navigate to the directory where you want to clone the project

3. Run the following command to clone the project

    ```bash
    git clone your-project-repository-url
    ```

## Run the application

1. Navigate to the project directory

2. Build the Docker image

    ```bash
    docker compose -f docker-compose.yml build
    ```

3. Run the Docker container

    ```bash
    docker compose -f docker-compose.yml up
    ```

4. Access the application in the browser using the public IP address of the EC2 instance

## Additional Steps

1. Configure the security group to allow traffic on the required ports

    - In security group settings, add inbound rules to allow traffic on the required ports (e.g., 80, 443, 22, etc.)

2. Configure the domain name for the application

    - You can use Route 53 for domain name registration

3. Configure the SSL certificate for the application

    - You can use AWS Certificate Manager for SSL certificate

4. Configure the database for the application

    - You can use AWS RDS for the database

5. Configure the environment variables for the application

6. Configure the logging and monitoring for the application

    - You can use AWS Systems Manager Parameter Store for storing and managing the environment variables

## Deploy without docker using gunicon service

1. Install gunicorn

    ```bash
    pip install gunicorn
    ```

2. Create a gunicorn service file

    ```bash
    sudo nano /etc/systemd/system/gunicorn.service
    ```

    Add the following content to the file:

    ```bash
    [Unit]
    Description=gunicorn daemon for myproject
    After=network.target

    [Service]
    User=ubuntu
    Group=www-data
    WorkingDirectory=/home/ubuntu/myproject
    ExecStart=/home/ubuntu/myproject/myprojectenv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 myproject.wsgi:application

    [Install]
    WantedBy=multi-user.target
    ```

3. Reload systemd

    ```bash
    sudo systemctl daemon-reload
    ```

4. Start the service

    ```bash
    sudo systemctl start gunicorn
    ```

5. Enable the service to start on boot

    ```bash
    sudo systemctl enable gunicorn
    ```

6. Check the status

    ```bash
    sudo systemctl status gunicorn
    ```

7. For reference, [running gunicorn using service](02_02_running_guncorn_using_service.md)

## Result

- Currently, the Django application is running in an AWS EC2 instance [http://65.0.89.211:8000/]

- The application is accessible using the public IP address of the EC2 instance.

- The application is running in a Docker container, which provides isolation and portability.

- The application can be further configured for security, scalability, and performance based on the requirements.

## Conclusion

In this guide, we have hosted a Django application in an AWS EC2 instance. You can now access the application using the public IP address of the EC2 instance. This is a basic setup to get your application running in an EC2 instance. You can further configure the instance for security, scalability, and performance based on your requirements.
