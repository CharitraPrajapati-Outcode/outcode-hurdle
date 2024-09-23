# Develop a Boilerplate Django REST Framework Project with Multi-Tenancy Support

## Introduction

Django REST Framework (DRF) is a powerful toolkit for building Web APIs in Django. It provides a flexible and customizable framework for creating RESTful APIs with minimal code. In this guide, we will develop a boilerplate Django REST Framework project with multi-tenancy support.

This project is a multi-tenant Django application using the `django-tenants` package. It supports multiple tenants, each with its own schema, while sharing certain apps across all tenants. The project structure is designed to be scalable and maintainable, making it easy to add new features and endpoints as needed.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Project Setup](#project-setup)
3. [Running the Project](#running-the-project)
4. [Managing Tenants](#managing-tenants)
5. [References](#references)

## Getting Started

### Prerequisites

- Python 3.x (3.8 or higher)
- Docker and Docker Compose
- PostgreSQL

### Installation

1. **Clone the Repository**:
   ```bash
   git clone https://your-repository-url.git
   cd your-project-directory
    ```

2. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
    ```

3. **Environment Configuration**
    ```bash
    cp .env.example .env
    ```

## Project Setup

### Initial Database Setup

1. **Run Initial Migrations**
    ```bash
    python manage.py migrate_schemas --shared
    ```

2. **Make Migrations for Your Apps**
    ```bash
    python manage.py makemigrations
    python manage.py migrate_schemas --shared
    ```

## Running the Project

### First Time Setup

    ```bash
    source ./entrypoint.sh
    ```

### Running the project

    ```bash
    Run `docker compose up` to start the project.
    ```

### Running the migrations

    ```bash
    Run `docker compose exec api python manage.py migrate` to run the migrations.
    ```

### Creating a superuser

    ```bash
    Run `docker compose exec api python manage.py createsuperuser` to create a superuser.
    ```

## Managing Tenants

### Creating a Tenant

1. **Create a Tenant**
    ```bash
    docker compose exec api python manage.py create_schema
    ```

2. **Remove a Tenant**
    ```bash
    docker compose exec api python manage.py delete_schema
    ```

3. **Creating a Super Admin for a Tenant**
    ```bash
    docker compose exec api python manage.py create_tenant_superuser --email=<email> --schema=<schema_name>
    ```

4. **Migrating Specific Schema**
    ```bash
    docker compose exec api python manage.py migrate_schemas --schema=<schema_name>
    ```

5. **Migrating User Roles on Tenant Schema**
    ```bash
    python manage.py tenant_command load_user_groups --schema=<schema_name>
    ```

6. **Creating Objects within a Specific Tenant**
    ```python
    from django_tenants.utils import schema_context

    schema_name_my = 'my-schema'
    with schema_context(schema_name_my):
        p1 = Plant.objects.create(name='Test', address='Test address')
    ```

## Connecting psql and psql Commands

1. **Connecting to psql**
    ```bash
    docker compose exec db psql -U postgres
    ```

2. **Listing all Databases**
    ```bash
    \l
    ```

3. **Connecting to a Specific Database**
    ```bash
    \c <database_name>
    ```

4. **Listing all Tables**
    ```bash
    \dt
    ```

5. **Listing all Schemas**
    ```bash
    \dn
    ```

6. **Switching to a Specific Schema**
    ```bash
    SET search_path TO <schema_name>;
    ```
    OR,
    ```bash
    SET schema '<schema_name>';
    ```
    OR,
    ```bash
    \c <database_name> <schema_name>
    ```

7. **Listing all Tables in a Specific Schema**
    ```bash
    \dt
    ```


## References

- [Django Tenants](https://django-tenants.readthedocs.io/en/latest/index.html)

