# Users

---

## Users handling
Users and workers are both stored in an AWS Cognito user pool.
This pool contains different groups:

* A "users" group, of which confirmed users of DECODE OpenCloud are members.
  Membership in this group is checked when [authenticating in the user-facing API](https://github.com/ries-lab/DECODE_Cloud_UserAPI/blob/main/api/dependencies.py).
* A "workers" group, of which confirmed workers of DECODE OpenCloud are members.
  Membership in this group is checked when [authenticating in the worker-facing API](https://github.com/ries-lab/DECODE_Cloud_WorkerAPI/blob/main/workerfacing_api/dependencies.py).
* A "cloud" group, of which cloud workers of DECODE OpenCloud are members.
  In particular, the [user used by the AWS Batch workers](https://github.com/ries-lab/DECODE_AWS_Infrastructure/blob/main/stack/apis/runtime/cognito_worker_user_trigger/lambda_script.py) is a member of this group.

---

## Registration workflow
##### User view
Potential users and workers can register using the "Registration" page of the [user frontend](https://github.com/ries-lab/DECODE_Cloud_UserFrontend).
They provide their email address, password, and possibly add some details about the registration request (e.g., who they are, why they would like to have access, if they are registering a worker).
They then need to verify the provided email address by using a verification code.
After successful registration, users are in a state of pending confirmation:
At login in the [user frontend](https://github.com/ries-lab/DECODE_Cloud_UserFrontend), they will be informed that their account still has to be confirmed by the administrators.
Once the account is confirmed, they are able to use the [frontend](https://github.com/ries-lab/DECODE_Cloud_UserFrontend) and APIs.

##### Administrators view
Confirmation of new users and workers is controlled by requiring their membership in the "users" and "workers" groups in the AWS Cognito user pool.
As long as this is not the case, their access to the APIs is blocked.
When a new user registers using the [user frontend](https://github.com/ries-lab/DECODE_Cloud_UserFrontend), an email is sent to the administrator email using a [Cognito post-creation trigger](https://github.com/ries-lab/DECODE_AWS_Infrastructure/tree/main/stack/apis/runtime/cognito_post_creation_trigger).
The administrator can see the request details on the AWS CLI and accept the request by adding the Cognito users to the appropriate groups.

---
