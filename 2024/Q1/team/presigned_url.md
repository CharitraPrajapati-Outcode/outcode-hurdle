# Learn to securely grant access to files for AWS S3 : Presigned URLs

## Introduction

A user who does not have AWS credentials or permission to access an S3 object can be granted temporary access by using a presigned URL.

In this guide, we will learn how to generate presigned URLs for S3 bucket objects using Django. We will use `boto3` library to interact with AWS S3 bucket.

## Prerequisites

1. AWS account
2. Django project
3. Python 3.6 or later
4. `boto3` library
5. IAM user access keys and secret keys

## Generate presigned URLs for S3 bucket objects

`Before you proceed: Make sure you have valid AWS IAM user access keys and secret keys to generate presigned URLs for S3 bucket objects.`

`Also, make sure you have access to s3 bucket to access the objects.`

Follow the below steps to generate presigned URLs for S3 bucket objects using Django:

1. Install `boto3` library using the following command:

```bash
pip install boto3
```

2. Create configuration settings for boto3 client for S3 bucket.

- Make sure to replace `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` with your AWS IAM user access keys and secret keys.

```python
import boto3

AWS_ACCESS_KEY_ID = env("AWS_ACCESS_KEY_ID")
AWS_SECRET_ACCESS_KEY = env("AWS_SECRET_ACCESS_KEY")

s3 = boto3.client(
   "s3",
   aws_access_key_id=AWS_ACCESS_KEY_ID,
   aws_secret_access_key=AWS_SECRET_ACCESS_KEY
)
```


2. Create a new django url to generate presigned URLs for S3 bucket objects.

```python
path('presigned-url', PresignedView.as_view(), name='presigned-url'),
```

3. Create a new Django view to generate presigned URLs for S3 bucket objects.

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

class PresignedView(APIView):
    def get(self, request, *args, **kwargs):
        file_name = request.query_params.get("file_name")

        try:
            presigned_url = generate_presigned_url(file_name)

            if presigned_url:
                return Response(
                    {"presigned_url": presigned_url},
                    status=status.HTTP_200_OK,
                )
            else:
                return Response(
                    {"message": "Error generating presigned URL"},
                    status=status.HTTP_400_BAD_REQUEST,
                )

        except Exception as e:
            logger.critical(f"Error generating presigned URL: {str(e)}")

            return Response(
                {"message": "Error generating presigned URL"},
                status=status.HTTP_400_BAD_REQUEST,
            )
```


4. Create a new function to generate presigned URLs for S3 bucket objects.

```python
from django.conf import settings
from config.boto3 import s3

def generate_presigned_url(
    file_name,  # File name or key in S3 bucket
    bucket_name=settings.AWS_STORAGE_BUCKET_NAME,   # S3 bucket name
    expires_in=settings.AWS_PRESIGNED_URL_EXPIRATION_TIME,  # Expiration time in seconds
):
    """
    Generate presigned URL
    """
    try:
        presigned_url = s3.generate_presigned_url(
            "get_object",
            Params={
                "Bucket": bucket_name,
                "Key": file_name,
            },
            ExpiresIn=expires_in,
        )

        return presigned_url

    except Exception as e:
        logger.critical(f"Error generating presigned URL: {str(e)}")

        return None
```

5. Example of generated presigned URL for S3 bucket object:

```json
{
    "presigned_url": "https://sample-bucket.s3.amazonaws.com/mediafiles/photos/03515197-7824-4d50-b72e-93e3bd9055f4.png?AWSAccessKeyId=AKIAZI2LDNWMJ6XYYN4W&Signature=%2FEqRXCl%2BUnuS8Fm7mWMNrsycpN4%3D&Expires=1709807702"
}
```

- Here, `sample-bucket` is the S3 bucket name and `mediafiles/photos/03515197-7824-4d50-b72e-93e3bd9055f4.png` is the S3 bucket object key.
- The generated presigned URL can be used to access the S3 bucket object for the specified expiration time.
- The expiration time is set in seconds.
- In presigned URL, `AWSAccessKeyId`, `Signature`, and `Expires` are the query parameters.
- `AWSAccessKeyId` is the AWS access key ID of the IAM user.
- `Signature` is the signature of the presigned URL.
- `Expires` is the expiration time of the presigned URL in seconds.
- These query parameters are used to authenticate and authorize the user to access the S3 bucket object.
- The presigned URL can be used to access the S3 bucket object without AWS credentials or permission.

## References

* [Presigned URLs](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-presigned-urls.html)
