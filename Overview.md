# Repositories overview
The DECODE Cloud project is composed of multiple repositories:
 - [UserFrontend](https://github.com/ries-lab/DECODE_Cloud_UserFrontend): the Vue.js app for the user-facing website.
 - [UserAPI](https://github.com/ries-lab/DECODE_Cloud_UserAPI): the API called by the front-end application.
 - [WorkerAPI](https://github.com/ries-lab/DECODE_Cloud_WorkerAPI): the API called by the workers.
 - [JobFetcher](https://github.com/ries-lab/DECODE_Cloud_JobFetcher): the python interface and Docker recipe for workers to run to fetch and process jobs.
 - [AWS_Infrastructure](https://github.com/ries-lab/DECODE_AWS_Infrastructure): the AWS infrastructure, defined using the Python CDK.

DECODE Cloud runs software in Docker containers with a compatible structure. Currently, it supports:
 - [DECODE](https://github.com/ries-lab/DECODE_Internal/tree/dockerfile_stable): SMLM.
 - [Comet](https://github.com/nolan1999/Comet/tree/docker): drift correction.

For development, the following repositories are additionally used:
 - [IntegrationTests](https://github.com/ries-lab/DECODE_Cloud_IntegrationTests): integration tests.
 - [Documentation](https://github.com/ries-lab/DECODE_Cloud_Documentation): this repository, for documentation.


![](./graphics/Overview/repos_overview.drawio.svg)


