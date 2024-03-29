# Dockerize django project

## Introduction

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing, and deploying code, you can significantly reduce the delay between writing code and running it in production. [Docker](https://docs.docker.com/get-started/overview/)

Docker is a software platform that allows you to build, test, and deploy applications quickly. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime. Using Docker, you can quickly deploy and scale applications into any environment and know your code will run. [AWS - Docker](https://aws.amazon.com/docker/)

Dockerizing the Django project ensures that it can run consistently across different environments. This facilitates easy collaboration among team members, reduces the "it works on my machine" problem, and provides a portable solution for development, testing, and deployment.

## Why Dockerize Django Application?

Dockerizing a Django application is a good practice for several reasons:

- **Consistency**: Docker containers ensure that your application runs consistently across different environments, such as development, testing, and production.

- **Isolation**: Docker containers isolate your application from the underlying host system, which can help prevent conflicts and ensure that your application runs as expected.

- **Portability**: Docker containers are portable and can be run on any system that supports Docker, making it easier to move your application between different environments.

- **Scalability**: Docker containers can be easily scaled up or down to meet the demands of your application, making it easier to handle increased traffic or load.

- **Ease of Deployment**: Docker containers make it easy to deploy your application to different environments, such as cloud platforms or on-premises servers.

- **Reproducibility**: Docker containers make it easy to reproduce the exact environment in which your application runs, which can be useful for debugging and troubleshooting.

- **Security**: Docker containers provide a layer of security by isolating your application from the underlying host system, which can help prevent unauthorized access and protect sensitive data.

## What is Dockerfile?

A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

### Steps to dockerize the django project

**ps** Make sure that you have installed Docker and Docker Compose in your local machine. Also, you have basic django project setup.

1. Create a `Dockerfile` in the root directory of the project.

    This is the sample `Dockerfile` for the django project.

    ```Dockerfile
    FROM python:3.12.2-slim

    # Set the working directory in the container
    WORKDIR /usr/src/app

    # set environment variables
    ENV PYTHONDONTWRITEBYTECODE 1
    ENV PYTHONUNBUFFERED 1

    # install dependencies
    RUN pip install --upgrade pip
    # Set the working directory in the container
    WORKDIR /usr/src/app

    # set environment variables
    ENV PYTHONDONTWRITEBYTECODE 1
    ENV PYTHONUNBUFFERED 1

    # install dependencies
    RUN pip install --upgrade pip
    COPY ./requirements.txt .
    RUN pip install -r requirements.txt

    # copy project
    COPY . .
    COPY ./requirements.txt .
    RUN pip install -r requirements.txt

    # copy project
    COPY . .
    ```

2. Create a `docker-compose.yml` file in the root directory of the project.

    This is the sample `docker-compose.yml` file for the django project.

    ```yml
    version: '3.8'

    services:
    web:
        build: .
        command: python manage.py runserver 0.0.0.0:8000
        volumes:
        - ./:/usr/src/app/
        ports:
        - 8080:8000 # Host:Container
        env_file:
        - ./.env
    ```

3. Create a `.env` file in the root directory of the project.

    This is the sample `.env` file for the django project.

    ```env
    DEBUG=True
    SECRET_KEY=your_secret_key
    ```

4. Build the docker image.

    ```bash
    docker-compose build
    ```

5. Run the docker container.

    ```bash
    docker-compose up
    ```

6. Open the browser and navigate to `http://localhost:8080` to see the running project.

7. You can also run the project in the background.

    ```bash
    docker-compose up -d
    ```

8. To stop the running container, run the following command.

    ```bash
    docker-compose down
    ```

9. To remove the stopped container, run the following command.

    ```bash
    docker-compose down
    ```


### Steps to dockerize the django project with PostgreSQL

1. Create a `Dockerfile` in the root directory of the project.

    This is the sample `Dockerfile` for the django project with PostgreSQL.

    ```Dockerfile
    FROM python:3.12.2-slim

    # Set the working directory in the container
    WORKDIR /usr/src/app

    # set environment variables
    ENV PYTHONDONTWRITEBYTECODE 1
    ENV PYTHONUNBUFFERED 1

    # install dependencies
    RUN pip install --upgrade pip
    COPY ./requirements.txt .
    RUN pip install -r requirements.txt

    # copy project
    COPY . .
    ```

2. Create a `docker-compose.yml` file in the root directory of the project.
    
        This is the sample `docker-compose.yml` file for the django project with PostgreSQL.
    
        ```yml
        version: '3.8'

        services:
        web:
            build: 
            context: .
            dockerfile: Dockerfile.stage
            command: python manage.py runserver 0.0.0.0:8000
            volumes:
            - ./:/usr/src/app/
            ports:
            - 8000:8000 # Host:Container
            env_file:
            - ./.env.stage
            depends_on:
            - db
        db:
            image: postgres:12
            environment:
            POSTGRES_DB: 'postgres'
            POSTGRES_USER: 'postgres'
            POSTGRES_PASSWORD: 'postgres'
            ports:
            - 5435:5432 # Host:Container
        ```

3. Create a `.env` file in the root directory of the project.

    This is the sample `.env` file for the django project with PostgreSQL.

    ```env
    DEBUG=True
    SECRET_KEY=your_secret_key

    # PostgreSQL settings
    DB_NAME=postgres
    DB_USER=postgres
    DB_PASSWORD=postgres
    DB_HOST=db
    DB_PORT=5432
    ```

**Note:** The above steps are the sample steps to dockerize the django project. You can modify the steps as per your project requirements.

### Steps to dockerize the django project with multiple docker-compose files for stage and production

1. Create a `Dockerfile` in the root directory of the project.

    This is the sample `Dockerfile` for the django project with multiple docker-compose files for stage.

    ```Dockerfile
    FROM python:3.12.2-slim

    # Set the working directory in the container
    WORKDIR /usr/src/app

    # set environment variables
    ENV PYTHONDONTWRITEBYTECODE 1
    ENV PYTHONUNBUFFERED 1

    # install dependencies
    RUN pip install --upgrade pip
    COPY ./requirements.txt .
    RUN pip install -r requirements.txt

    # copy project
    COPY . .
    ```

2. Create a `docker-compose.yml` file in the root directory of the project.
    
    This is the sample `docker-compose.yml` file for the django project with multiple docker-compose files for stage.

    ```yml
    version: '3.8'

    services:
    web:
        build: 
        context: .
        dockerfile: Dockerfile.stage
        command: python manage.py runserver 0.0.0.0:8000
        volumes:
        - ./:/usr/src/app/
        ports:
        - 8000:8000 # Host:Container
        env_file:
        - ./.env.stage
        depends_on:
        - db
    db:
        image: postgres:12
        environment:
        POSTGRES_DB: 'postgres'
        POSTGRES_USER: 'postgres'
        POSTGRES_PASSWORD: 'postgres'
        ports:
        - 5435:5432 # Host:Container
    ```

3. Create a `.env` file in the root directory of the project.
    
    This is the sample `.env` file for the django project with multiple docker-compose files for stage.

    ```env
    DEBUG=True
    SECRET_KEY=your_secret_key

    # PostgreSQL settings
    DB_NAME=postgres
    DB_USER=postgres
    DB_PASSWORD=postgres
    DB_HOST=db
    DB_PORT=5432
    ```

4. Create a `Dockerfile.prod` in the root directory of the project.
    
    This is the sample `Dockerfile.prod` for the django project with multiple docker-compose files for production.

    ```Dockerfile
    FROM python:3.12.2-slim

    # Set the working directory in the container
    WORKDIR /usr/src/app

    # set environment variables
    ENV PYTHONDONTWRITEBYTECODE 1
    ENV PYTHONUNBUFFERED 1

    # install dependencies
    RUN pip install --upgrade pip
    COPY ./requirements.txt .
    RUN pip install -r requirements.txt

    # copy project
    COPY . .
    ```

5. Create a `docker-compose.prod.yml` file in the root directory of the project.
        
    This is the sample `docker-compose.prod.yml` file for the django project with multiple docker-compose files for production.

    ```yml
    version: '3.8'

    services:
    web:
        build: 
            context: .
            dockerfile: Dockerfile.prod
        command: gunicorn config.wsgi:application --bind 0.0.0.0:8000 --reload
        volumes:
            - ./:/usr/src/app/
        ports:
            - 8000:8000 # Host:Container
        env_file:
            - ./.env.prod
        depends_on:
            - db
    db:
        image: postgres:12
        environment:
            POSTGRES_DB: 'postgres'
            POSTGRES_USER: 'postgres
            POSTGRES_PASSWORD: 'postgres
        ports:
            - 5435:5432 # Host:Container
    ```

6. Create a `.env.prod` file in the root directory of the project.
        
    This is the sample `.env.prod` file for the django project with multiple docker-compose files for production.

    ```env
    DEBUG=False
    SECRET_KEY=your_secret_key

    # PostgreSQL settings
    DB_NAME=postgres
    DB_USER=postgres
    DB_PASSWORD=postgres
    DB_HOST=db
    DB_PORT=5432
    ```

**Note:** The above steps are the sample steps to dockerize the django project with multiple docker-compose files for stage and production. You can modify the steps as per your project requirements.

7. Build the docker image for stage.

    ```bash
    docker-compose -f docker-compose.yml build
    ```

8. Run the docker container for stage.

    ```bash
    docker-compose -f docker-compose.yml up
    ```

9. Build the docker image for production.

    ```bash
    docker-compose -f docker-compose.prod.yml build
    ```

10. Run the docker container for production.

    ```bash
    docker-compose -f docker-compose.prod.yml up
    ```

11. Open the browser and navigate to `http://localhost:8000` to see the running project.



## Inspiration

- [Dockerize Django Application (Testdriven)](https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/)

## Additional Resources

- [Dockerfile Best Practice (Docker)](https://docs.docker.com/develop/develop-images/instructions/)
- [Dockerfile Best Practice (GitHub: dnaprawa)](https://github.com/dnaprawa/dockerfile-best-practices)
- [Docker Development Best Practice (Docker)](https://docs.docker.com/develop/dev-best-practices/)

- [Docker compose version](https://docs.docker.com/compose/compose-file/compose-versioning/)
- [Docker compose file](https://docs.docker.com/compose/compose-file/)

- [Dockerfile dockerignore](https://www.linkedin.com/pulse/securing-docker-builds-comprehensive-guide-usage-best-ilyas-ou-sbaa)