## Integration test
See the [integration tests repository](https://github.com/ries-lab/DECODE_Cloud_IntegrationTests): can test both local and cloud workers.

## Unit tests
See the respective repositories.

## Testing some parts
#### Testing user workflow without the frontend
This is essentially what is done by the integration test.
To do it manually using the Swagger docs of the user-facing API:  
Post a new user:  
```
{
  "email": "user@example.com",
  "password": "Password1+"
}
```  
On the AWS console, verify this new user.  
Post a token and authenticate.  
Post an example parameter file and an example bead calibration (see for example the files in the ![DECODE tutorial](https://colab.research.google.com/drive/1uQ7w1zaqpy9EIjUdaLyte99FJIhJ6N8E?usp=sharing)).
In the example below, they are posted under `test/param.yaml` and `test/calib.mat`.  
Post a job:  
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

#### Testing lambda functions
Use the AWS CLI.

