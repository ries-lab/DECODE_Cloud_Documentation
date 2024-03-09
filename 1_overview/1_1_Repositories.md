# Repositories overview
The DECODE OpenCloud project is composed of multiple repositories:
 - [UserFrontend](https://github.com/ries-lab/DECODE_Cloud_UserFrontend): the Vue.js app for the user-facing website.
 - [UserAPI](https://github.com/ries-lab/DECODE_Cloud_UserAPI): the API called by the front-end application.
 - [WorkerAPI](https://github.com/ries-lab/DECODE_Cloud_WorkerAPI): the API called by the workers.
 - [JobFetcher](https://github.com/ries-lab/DECODE_Cloud_JobFetcher): the python interface and Docker recipe for workers to run to fetch and process jobs.
 - [AWS_Infrastructure](https://github.com/ries-lab/DECODE_AWS_Infrastructure): the AWS infrastructure, defined using the Python CDK.

DECODE Cloud runs software in Docker containers with a compatible structure. Currently, it supports:
 - [DECODE](https://github.com/ries-lab/DECODE_Internal/tree/dockerfile_stable): SMLM.
 - [Comet](https://github.com/ries-lab/Comet): drift correction.

For development, the following repositories are additionally used:
 - [IntegrationTests](https://github.com/ries-lab/DECODE_Cloud_IntegrationTests): integration tests.
 - [Documentation](https://github.com/ries-lab/DECODE_Cloud_Documentation): this repository, for documentation.

![](./graphics/repos_overview.drawio.svg)


# Repositories interdependencies
The repositories are intertwined in the manners detailed below:

![](./graphics/repos_dependencies.drawio.svg)

 - The user-facing API and worker-facing API are aware of each other's URL, to be able to exchange information about the jobs. This gives rise to a circular dependency that is resolved by ![setting one environment variable in the CDK code directly](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/stack/apis/infrastructure.py) and using a ![triggered function in the CDK code](https://github.com/ries-lab/DECODE_AWS_Infrastructure/tree/main/stack/apis/runtime/api_trigger).
 - The user-facing API and worker-facing API communicate via internal endpoints that should only be accessible to them. This is safeguarded using an API key (![here](https://github.com/ries-lab/DECODE_Cloud_UserAPI/blob/main/api/dependencies.py) and ![here](https://github.com/ries-lab/DECODE_Cloud_WorkerAPI/blob/main/workerfacing_api/dependencies.py)), that is stored as a ![secret in AWS SecretsManager](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/stack/apis/infrastructure.py).
 - The workers and frontend need the URLs of the worker-facing API and the user-facing API respectively, that are passed to them as environment variables (![here](https://github.com/ries-lab/DECODE_Cloud_JobFetcher/blob/main/.env.example) for local workers, ![here](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/stack/worker/infrastructure.py) for cloud workers, and ![here](https://github.com/ries-lab/DECODE_Cloud_UserFrontend/blob/main/src/main.js) for the frontend). Additionally, the frontend requires appropriate CORS middleware both on the ![user-facing API side](https://github.com/ries-lab/DECODE_Cloud_UserAPI/blob/main/api/main.py) and on the ![AWS S3 bucket side](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/stack/data/infrastructure.py).
 - Worker-facing API, user-facing API, user frontend all authenticate in an AWS Cognito user pool, defined in the ![AWS infrastructure repository](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/stack/apis/infrastructure.py). Users are divided into "users" and "workers"; additionally, cloud workers are identified by their membership in the "cloud" group. A user for the cloud workers is created with the CDK stack using a ![trigger](https://github.com/ries-lab/DECODE_AWS_Infrastructure/tree/main/stack/apis/runtime/cognito_worker_user_trigger).
 - We typically store the Docker images of the supported applications in an AWS ECR repository (outside of the CDK stack). The AWS infrastructure repository contains a ![script to push local Docker images to a public ECR repository](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/scripts/push_local_dockerimage.py). Additionally, we provide scripts to ![link the APIs deployed on AWS to custom domains](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/scripts/link_custom_domain.py) and ![setup the email sender for the user-facing API to send notifications to users](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/scripts/link_email_sender.py).
