# DECODE_Cloud_Documentation
Documentation for DECODE OpenCloud. 

## Scenarios
### Running one of the implemented algorithms.
Steps
- Register an account
- Use the frontend to upload data (and config)
- Start the job
- Retrieve results

### Attaching Compute Power to DECODE OpenCloud
Requirements:
- Linux machine with CUDA hardware

Steps
- Register an account (can be the same as for usage)
- Install docker
- Follow the instructions of the [JobFetcher](https://github.com/ries-lab/DECODE_Cloud_JobFetcher)

### Adding a new algorithm
tbd

## Contents
1. Overview
   1. [Repositories.md](./1_overview/1_1_Repositories.md): overview of the different repositories.
   2. [Workflow.md](./1_overview/1_2_Workflow.md): jobs workflow.
   3. [Design.md](./1_overview/1_3_Design.md): design choices.
2. Guides
   1. [Applications.md](./2_guides/2_1_Applications.md): supported applications, how to publish a new application.
   2. [Users.md](./2_guides/2_2_Users.md): users and workers management.
   3. [Testing.md](./2_guides/2_3_Testing.md): how to test.
   4. [Debugging.md](./2_guides/2_4_Debugging.md): solutions to possible problems.
