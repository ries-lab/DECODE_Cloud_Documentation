This list is meant to be filled as problems arise and we find fixes.

#### Debugging jobs
Note that the IDs in the user-facing and worker-facing APIs are not the same.
Jobs in the worker-facing API contain the job ID in the user-facing API in the field `["meta"]["job_id"]`.

#### Cloud job not starting
Check the following:
 - Did any pre- or post-processing Lambda function fail? On the AWS CLI, go to `Lambda > Functions > <function> > Monitor` and see if there were any failures.
 - Is the job on AWS Batch stuck in the "runnable" state (`Batch > Job queue overview > <queue>`)? This can happen if the allowed instance types do not have the resources requested by the job. See whether any instance is started by going to `EC2 > Instances (running)`.

#### Frontend not changing after update
You need to refresh the page, and possibly clear the browser history before reloading.