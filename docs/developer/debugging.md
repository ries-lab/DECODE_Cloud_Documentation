# Debugging

---

This list is meant to be filled as problems arise and we find fixes.

---

##### Debugging jobs
Note that the IDs in the user-facing and worker-facing APIs are not the same.
Jobs in the worker-facing API contain the job ID in the user-facing API in the field `["meta"]["job_id"]`.

##### Cloud job not starting
Check the following:

* Did any pre- or post-processing Lambda function fail? On the AWS CLI, go to `Lambda > Functions > <function> > Monitor` and see if there were any failures.
* Is the job on AWS Batch stuck in the "runnable" state (`Batch > Job queue overview > <queue>`)? This can happen if the allowed instance types do not have the resources requested by the job. See whether any instance is started by going to `EC2 > Instances (running)`.

##### Frontend not changing after update
You need to refresh the page, and possibly clear the browser history before reloading.

##### After re-deploying, cloud jobs fail
Check whether the problem is with mounting EFS.
If a new filesystem is created, you need to de-register all old AWS Batch job definitions, since they still try to mount the old filesystem.

##### Cloud job stuck on pre- or post-processing
The lambdas might be failing. 
To debug:

* Test the lambda functions manually.
* Add the `BasicExecutionRole` to have them log to CloudWatch, then look at the logs.
    * In particular, the lambdas might have too little memory.

---
