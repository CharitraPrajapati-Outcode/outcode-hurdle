# Implement Sentry for Error Tracking in DRF

## Description

Sentry provides real-time error tracking, helping you monitor, debug, and resolve issues efficiently. It improves application stability and enhances DevOps workflows by capturing, analyzing, and resolving errors. By implementing Sentry, you can detect and fix issues before they become critical, ensuring a smooth user experience.

## Activity

- Install & Configure Sentry in settings.py
- Enable Error Logging to capture and log issues
- Set Up Alerts for real-time error notifications
- Monitor & Analyze errors via Sentry’s dashboard
- Document Configuration for future reference

## Result

- Gained hands-on experience with Sentry’s tracking and alerting tools
- Gained the ability to quickly identify, monitor, and resolve application errors in real time
- Improved application quality by using error data to prevent recurring issues and optimize performance


# Activity Logs

## 0. Description

Before getting started, make sure to have a Sentry account and a project set up.

If you don't have a Sentry account, you can sign up for free at https://sentry.io/. Or, have client or team members sign up for you.

## 0.1. Setup project in Sentry

- After login on Sentry, navigate to 'Projects' section and click on 'Create Project' button.
- You will be redirected to the project creation page.
- Fill in the required details and click 'Create Project' button.
- Once the project is created, you will be redirected to the project details page.
- Click on 'Integrations' tab and choose 'Python' integration.
- Click on 'Add Integration' button.
- Copy the generated DSN and paste it in your settings.py file. (see below)
- Click on 'Save' button.


## 1. Install & Configure Sentry in settings.py

**Install Sentry SDK in your DRF application using pip**

```python
pip install sentry-sdk
```

Or,

```python
pip install sentry-sdk[django]
```

**Configure Sentry in settings.py**

```python
import sentry_sdk

from decouple import config


sentry_sdk.init(
    dsn=config("SENTRY_DSN", default=""),
    # Add data like request headers and IP for users,
    # see https://docs.sentry.io/platforms/python/data-management/data-collected/ for more info
    send_default_pii=True,
    # Set traces_sample_rate to 1.0 to capture 100%
    # of transactions for tracing.
    traces_sample_rate=1.0,
    _experiments={
        # Set continuous_profiling_auto_start to True
        # to automatically start the profiler on when
        # possible.
        "continuous_profiling_auto_start": True,
    },
)
```

**Verify Sentry installation by adding intentional error in your application**

```python
division_by_zero = 1 / 0
```


## 2. Enable Error Logging to capture and log issues

- In your DRF application's settings.py file, set the `LOGGING` dictionary to enable logging.

```python
# Logging configurations
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'standard': {
            'format': '%(asctime)s [%(levelname)s] %(name)s: %(message)s',
            'datefmt': '%m/%d/%Y %I:%M:%S %p'
        },
    },
    'handlers': {
        'default': {
            'level': 'INFO',
            'class': 'logging.handlers.TimedRotatingFileHandler',
            'filename': 'logs/project.log',
            'when': 'midnight',
            'backupCount': 20,  # max file before override
            'formatter': 'standard',
        },
        'request_handler': {
            'level': 'DEBUG',
            'class': 'logging.handlers.TimedRotatingFileHandler',
            'filename': 'logs/django_request.log',
            'when': 'midnight',
            'backupCount': 20,  # max file before override
            'formatter': 'standard',
        },
    },
    'root': {
        'handlers': ['default'],
        'level': 'DEBUG',
        'propagate': True
    },
    'loggers': {
        '': {  # for default logging setup
            'handlers': ['default'],
            'level': 'DEBUG',
            'propagate': True
        },
        'django.request': {
            'handlers': ['request_handler'],
            'level': 'DEBUG',
            'propagate': True
        },
    }
}
```

## 3. Set Up Alerts for real-time error notifications

- On your sentry dashboard, click on 'Alerts' tab.
- Click on your project and Click on 'Create Alert' button.
- Set conditions for the alert
- Follow guide to set condition:
    - Select an environment and project (select created project)
    - Set conditions are:
        - when new event is created
        - if all filter matches
        - then perform this (generally email to team or members)
    - set intervals
    - adding name and owner
- Then save the alert.

## 4. Monitor & Analyze errors via Sentry’s dashboard

- On your sentry dashboard, click on 'Issues' tab.
- You can filter issues with selected projects, resolved/unresolved, based on different priorities, environments, etc
- Click on issues to get details
- Some important things to note under 'Detail' section:
    - 'Last Seen' is the last time the issue was seen
    - 'First Seen' is the first time the issue was seen
    - 'Times Seen' is the number of times the issue has been seen
    - 'handled' is the status of the issue, whether it is resolved or unresolved
    - 'url' is the link to the original link via which the issue was created
    - 'trace' is the link to the full stack trace of the issue
    - 'assigned to' is the person to whom the issue is assigned
    - 'breadcrumb' is where the SQL query is stored for the issue
    - Under 'tags' and 'context' section, you can find the other important details of the issue
- Some important things to note under 'All Events' section:
    - it shows list of the events that are logged in the project
- Inside issues -> For review section, sometimes you can even see N+1 time complexity issues which is a performance issue

- Under 'Insight' -> Backend section, you can find the insights of the selected project.
    - you can see the most time consuming queries
    - transactions per minute cycle


# References

- [Sentry Documentation](https://docs.sentry.io/)
- [Sentry Documentation Python](https://docs.sentry.io/platforms/python/)
