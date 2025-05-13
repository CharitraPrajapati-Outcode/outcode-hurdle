# Project deploy checklist basic for AWS DRF project

## DRF Application Deployment Checklist on AWS
1️
### Infrastructure Setup
- Create an EC2 instance (Ubuntu/Debian-based recommended)
- Configure security groups (allow SSH, HTTP, HTTPS, and required ports)
- Set up Elastic Load Balancer (ELB) for high availability (if needed)
- Configure Auto Scaling for better traffic management (optional)
- Set up Amazon RDS (PostgreSQL/MySQL) for the database
- Configure S3 buckets for static and media files
- Enable SES (Simple Email Service) for email handling
- Set up IAM roles & policies for secure access control
- Assign IAM permissions for EC2, S3, RDS, and SES

### Server Configuration & Deployment Preparation
- SSH into the EC2 instance and update the system (sudo apt update && sudo apt upgrade -y)
- Install Docker & Docker Compose for containerized deployment
- Set up Git and clone the DRF project repository
- Configure environment variables (.env file) securely
- Install Nginx and set up reverse proxy configuration
- Install PostgreSQL/MySQL client if needed for database connectivity
- Configure systemd services for process management (if not using Docker)

### Application Setup & Configuration
- Install Python & required dependencies (pip install -r requirements.txt)
- Apply database migrations (python manage.py migrate)
- Set up Django admin user (python manage.py createsuperuser)
- Collect static files & upload to S3 (python manage.py collectstatic --noinput)
- Configure CORS, CSRF, and security settings in Django settings
- Test local application server (gunicorn --bind 0.0.0.0:8000 project.wsgi)
- Set up Redis & Celery (if using async tasks)

### Nginx & SSL Configuration
- Set up Nginx configuration
- Install Certbot and configure Let's Encrypt SSL for HTTPS
- Restart Nginx and verify service is running correctly (sudo systemctl restart nginx)

### CI/CD Pipeline & GitHub Actions Runner
-Set up GitHub Actions Runner on EC2 instance
-Create GitHub Actions workflows for automated deployment
-Configure CI/CD pipeline for testing and deployment
-Set up Git branching & versioning strategy for release management
-Test automatic deployment by pushing a new commit

### Docker & Containerization (Optional but Recommended)
- Create a Dockerfile for the DRF application
- Build and test the Docker container locally
- Set up Docker Compose for managing multi-container environments
- Push Docker images to Amazon Elastic Container Registry (ECR)
- Deploy application using Docker on EC2

### IAM Policies & Security Best Practices
- Create and assign IAM roles with least-privilege permissions
- Restrict database access to allowed IPs only
- Configure AWS Secrets Manager for storing sensitive credentials
- Enable CloudWatch monitoring for logs and performance metrics

### Testing & Final Checks
- Verify API endpoints & application functionality
- Check database connection & query performance
- Confirm email sending via SES
- Run load tests to evaluate performance under traffic
- Set up monitoring & alerts using AWS CloudWatch

### Production Deployment & Maintenance
- Deploy the application to production environment
- Set up log rotation & monitoring tools
- Create regular backups for database & storage
- Document the deployment process for future reference

### Final Confirmation Before Going Live
- Application is running smoothly on AWS
- Security configurations (IAM, SSL, firewall rules) are properly set
- CI/CD pipeline is successfully deploying updates
- Monitoring and alerts are set up for tracking performance
