# Running gunicorn using service

## how to serve django project using guvicorn in stage server

1. Install Gunicorn

First, you need to install Gunicorn in your virtual environment. If you haven't already created a virtual environment for your Django project, you can do so using the following commands:

```bash
cd /path/to/your/django/project
python3 -m venv venv
source venv/bin/activate
```

Once you're in the virtual environment, you can install Gunicorn using pip:

```bash
pip install gunicorn
```

2. Create a Gunicorn Configuration File

Next, you'll need to create a Gunicorn configuration file to specify the settings for running your Django project. You can create a file named gunicorn_config.py in the root directory of your Django project, and add the following content:

```bash
bind = "127.0.0.1:8000"
workers = 3
timeout = 30
```

In this configuration:

    - bind: Specifies the socket Gunicorn will bind to. 127.0.0.1:8000 means it will bind to localhost on port 8000.
    - workers: The number of worker processes Gunicorn will spawn to handle incoming requests.
    - timeout: The maximum amount of time a worker can take to respond to a request before being killed and restarted.


3. Test Gunicorn

Before setting up Gunicorn as a service, you can test it to ensure that it's working correctly. You can start Gunicorn using the following command:

```bash
/path/to/your/virtualenv/bin/gunicorn -c /path/to/gunicorn_config.py your_project_name.wsgi:application
```

OR

```bash
gunicorn -c /path/to/gunicorn_config.py your_project_name.wsgi:application
```

Replace /path/to/your/virtualenv with the actual path to your virtual environment, /path/to/gunicorn_config.py with the actual path to your Gunicorn configuration file, and your_project_name with the actual name of your Django project.

If Gunicorn starts successfully, you should see output indicating that it's running and listening on the specified socket. You can then access your Django project by visiting the specified address in a web browser.

4. Configure Reverse Proxy (Optional)

If you want to access your Django application through a web server like Nginx or Apache, you need to configure a reverse proxy. Here's an example Nginx configuration:

```perl
    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://127.0.0.1:8000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
```

## Setting Up Gunicorn as a Systemd Service

Using systemd to manage your Gunicorn process ensures that it runs as a service, starting automatically on system boot and restarting if it crashes. Here's how you can set it up:

Create a systemd service unit file:

Create a file ending with .service extension, for example your_project.service, in the /etc/systemd/system directory. You can use any text editor to create and edit this file, such as nano or vim.

```bash
sudo nano /etc/systemd/system/your_project.service
```

Add the following content to the file:

Replace placeholders (/path/to/gunicorn_config.py, your_project_name, etc.) with the appropriate paths and values for your setup.

```bash
[Unit]
Description=Gunicorn instance to serve your_project_name
After=network.target

[Service]
User=<your_server_user>
Group=<your_server_group>
WorkingDirectory=/path/to/your/django/project
ExecStart=/path/to/your/virtualenv/bin/gunicorn -c /path/to/gunicorn_config.py your_project_name.wsgi:application

[Install]
WantedBy=multi-user.target
```

User and Group: Specify the user and group that should run the Gunicorn process. Replace <your_server_user> and <your_server_group> with appropriate values.
WorkingDirectory: Specify the directory where your Django project is located.
ExecStart: Specify the command to start Gunicorn, including the path to your virtual environment and the Gunicorn configuration file.
Reload systemd:

After creating or editing the service file, reload systemd to apply the changes:

```bash
sudo systemctl daemon-reload
```

Start the service:

Start the newly created service:

```bash
sudo systemctl start your_project
```

Enable the service to start on boot:

To ensure that the Gunicorn service starts automatically on system boot:

```bash
sudo systemctl enable your_project
```

Check the status:

You can check the status of the service to ensure it's running without any issues:

```bash
sudo systemctl status your_project
```

This command will show you if the service is active and if there were any errors.

With these steps, your Django project should now be served by Gunicorn as a systemd service on your staging server. You can also use systemctl stop your_project and systemctl restart your_project to stop and restart the service respectively.



## New service file for gunicorn

[Unit]
Description=Gunicorn instance to serve basic card project
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/personal/projects/basic
ExecStart=/home/ubuntu/personal/projects/basic/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 core.wsgi:application

[Install]
WantedBy=multi-user.target

