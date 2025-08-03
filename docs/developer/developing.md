# Developing

---

## Environments

We have a **production** (prod) and a **development** (dev) environment.

* Each environment has its own APIs and AWS stack.
* The application images, as they are not part of the stack itself, are common to both environments. However, the batch job definitions are separate.
* [not yet implemented] The prod stack follows the `prod` branch in each repository, the dev stack follows the `main` branch in each repository.
* The prod stack is always deployed, the dev stack can be deleted to save resources when not required.

---

## Dependencies handling

We try to use similar structures in all repositories for consistency.
In particular, we use [`poetry`](https://python-poetry.org/) for dependency handling.
Additionally, we use the scripts functionality of poetry to more easily build/run/delete/cleanup docker images and serve the APIs.

---

## Code checks

All repositories use two required checks on PRs:

* Static code checks
    * `ruff` and `ruff format`
    * `poetry` (in strict mode)
* Tests using `pytest`

Some tests require AWS credentials for a test user with the required permissions to test resources.
For this, we use secrets in the GitHub repositories.

The checks can also be ran manually:

* `pre-commit install` to automatically run the static code checks on commit
* `pytest tests/ [-m "aws or not(aws)"]` to automatically run the tests (the `aws` marker tags tests that require AWS credentials)

---

## Releases and deployment
The versions of the applications (user-facing API, worker-facing API, job fetcher) are managed using `poetry`:

* To bump the version in `pyproject.toml`, run `poetry version major|minor|patch`.
* When a commit is merged and the version in `pyproject.toml` was changed:
    * A docker image is published to AWS ECR (public);
    * A github release is created.
* Alternatively, one can manually run the `publish-version` GH action for a docker build that can be manually deployed/tested.

Deployment is done manually (both for dev and for prod) via the AWS_Infrastructure repository using the Python AWS CDK.
Two `yaml` files (`config_prod.yaml` and `config_dev.yaml`) configure the two stacks.
In the config, the version of the APIs and job fetcher are set (so that they can be different for `prod` and `dev`).
To test a new version of the API on dev, one does not necessarily need to run the cloud formation tools;
instead, they can simply update the docker image setting of the corresponding AppRunner Service on the AWS console.

Updating the JobFetcher version for workers is the task of the workers themselves.
