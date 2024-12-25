# Design choices

---

## Architecture
#### Pulling architecture of jobs
The workers pull the jobs from the queue, instead of the queue pushing jobs to them.
This way, the worker-facing API does not need a direct connection to the workers.
New workers can be more easily added.
Workers can pull jobs only whenever they have free resources, and do not get constant updates.

#### Database as a queue
Allows filtering by more criteria, including prioritization, resource matching, and so on.
We will not have so many jobs that performance could significantly suffer.

---

## Technical implementation
#### Different job IDs in the user-facing and worker-facing APIs
The worker-facing API queue being a database is only one implementation, and might change in the future.
Having own IDs decouples user- and worker-facing APIs.
In particular, we might have different user-facing APIs using the same worker-facing API.
Jobs can be deleted from the worker-facing API's database when they finished, while coupled IDs would ideally require consistency (1:1 relation) between the two databases.

#### Cognito pool: "users" and "workers" groups
"users" and "workers" groups are a nice way to allow users to self-register in the user pool and verify their email address, while not allowing free access to anyone.

#### Docker with no entrypoint
This is required because we cannot dynamically define a mount point when starting a job on AWS Batch (it would require a new job definition).
This means that we need to mount a path on EFS that is common to all jobs: to separate the filesystem of each job, we create one folder within it per job.
On the other hand, we want the user (and user-facing API) to define paths relative to a fixed path, not to a path dependent on the job ID.
What we do to solve this is to prepend a command that points `/files` to the job-specific directory to the Docker command.
An entrypoint would run before the command, and not have access to the correct filesystem.

#### Pre-signed URLs for data down- and upload
This avoids the data traffic passing through the API.
Instead, the client directly connects to S3.

#### Keep-alive signals
These are implemented to notice silent failures of workers.
A daemon on the worker-facing API periodically checks and requeues jobs that have not received status updates for a while.

---
