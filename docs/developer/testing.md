# Testing

---

## Integration test
See the [integration tests repository](https://github.com/ries-lab/DECODE_Cloud_IntegrationTests): it can test both local and cloud workers.

##### Manual integration testing
The [`Run tests`](https://github.com/ries-lab/DECODE_Cloud_IntegrationTests/actions/workflows/run-tests.yaml) GitHub action allows testing a simple job workflow.
You can specify which branches of the different repositories to use to integration-test coordinated changes in multiple repositories before merging them.

The same process can be done locally using the `pytest` tests in the [integration tests repository](https://github.com/ries-lab/DECODE_Cloud_IntegrationTests).
This requires locally cloned repositories, checked out to the changes to test.
On the background, docker images are tested and spin up with `docker-compose`.

If you want to do manual testing of different configurations, you can simply spin up the docker containers (images automatically re-built) using `docker-compose up [-d]`.

##### Automated integration test
Currently, the [`Run tests`](https://github.com/ries-lab/DECODE_Cloud_IntegrationTests/actions/workflows/run-tests.yaml) GitHub action runs once a week testing a DECODE v0.10.1 job on the prod stack with a cloud worker with GPU.

---

## Unit tests
See the respective repositories.
They are mostly required on PRs.

Repository integration tests (and sometimes even unit tests) require a testing AWS account and a user with some permissions on testing S3 buckets/RDS instances.
We store credentials for them in GH's organization secrets, so that they are common across repositories.
However, those tests mostly needed to be ran once manually with a role with more permissions, since we do not want to give GH actions the permission to e.g. create RDS instances (this is also mentioned in the repositories affected).
Currently, the permissions given to the GH runner testing user are (might not be up-to-date):
```
// DecodeCloudTestsEC2Ingress
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ec2:RevokeSecurityGroupIngress",
                "ec2:AuthorizeSecurityGroupIngress"
            ],
            "Resource": "arn:aws:ec2:eu-central-1:<aws-account-id>:security-group/<sg-id>"
        }
    ]
}
// DecodeCloudTestsRDS
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "rds:DescribeDBInstances",
            "Resource": [
                "arn:aws:rds:*:<aws-account-id>:db:decodecloudintegrationtests",
                "arn:aws:rds:*:<aws-account-id>:db:decodecloudqueuetests",
                "arn:aws:rds:eu-central-1:<aws-account-id>:db:decodecloudintegrationtestsuserapi"
            ]
        }
    ]
}
// DecodeCloudTestsS3Buckets
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::decode-cloud-integration-tests",
                "arn:aws:s3:::decode-cloud-integration-tests/*",
                "arn:aws:s3:::decode-cloud-filesystem-tests",
                "arn:aws:s3:::decode-cloud-filesystem-tests/*",
                "arn:aws:s3:::decode-cloud-user-integration-tests",
                "arn:aws:s3:::decode-cloud-user-integration-tests/*",
                "arn:aws:s3:::decode-cloud-user-filesystem-tests",
                "arn:aws:s3:::decode-cloud-user-filesystem-tests/*"
            ]
        }
    ]
}
// DecodeCloudTestsSecrets
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "arn:aws:secretsmanager:eu-central-1:<aws-account-id>:secret:decode-cloud-tests-db-pwd*"
        }
    ]
}
// SQSTestingDecodeCloud
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "sqs:ListQueues",
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "sqs:*",
            "Resource": [
                "arn:aws:sqs:*:<aws-account-id>:cloud_queue.fifo",
                "arn:aws:sqs:*:<aws-account-id>:local_queue.fifo",
                "arn:aws:sqs:*:<aws-account-id>:None_queue.fifo"
            ]
        }
    ]
}
```

---

## Manual tests
##### Testing user workflow without the frontend
This is essentially what is done by the integration test.
To do it manually using the Swagger docs of the user-facing API:  

1. Post a new user:  
  ```
  {
    "email": "user@example.com",
    "password": "Password1+"
  }
  ``` 
2. On the AWS console, verify this new user.  
3. Post a token and authenticate.  
4. Post an example parameter file and an example bead calibration (see for example the files in the [DECODE tutorial](https://colab.research.google.com/drive/1uQ7w1zaqpy9EIjUdaLyte99FJIhJ6N8E?usp=sharing)).
  In the example below, they are posted under `test/param.yaml` and `test/calib.mat`.  
5. Post a job:  
```
{
  "job_name": "decode_test",
  "environment": "cloud",
  "priority": 0,
  "application": {
    "application": "decode",
    "version": "v0_10_1",
    "entrypoint": "train"
  },
  "attributes": {
    "files_down": {
      "config_id": "test",
      "data_ids": ["test"],
      "artifact_ids": [
      ]
    },
    "env_vars": {
    }
  },
  "hardware": {
    "cpu_cores": null,
    "memory": null,
    "gpu_model": null,
    "gpu_archi": null,
    "gpu_mem": null
  }
}
```

You can try pulling the job from the deployed worker-facing API using the Swagger docs as well (worker authentication similar to the user authentication -- you can get the token using the worker-facing API).

##### Testing lambda functions
Use the AWS CLI.

---
