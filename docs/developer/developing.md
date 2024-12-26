# Developing

---

## Environments

**This is WIP (i.e., not yet implemented).**

We would like to have a **production** (prod) and a **development** (dev) environment.

* Each environment has its own APIs and AWS stack.
* The application images, as they are not part of the stack itself, are common to both environments.
* The prod stack follows the `prod` branch in each repository, the dev stack follows the `main` branch in each repository.
* The prod stack is always deployed, the dev stack can be deleted to save resources when not required.

---

## Dependencies handling

We try to use similar structures in all repositories for consistency.
In particular, we use [`poetry`](https://python-poetry.org/) for dependency handling.
Additionally, we use the scripts functionality of poetry to more easily build/run/delete/cleanup docker images and serve the APIs.

---

## Code checks

Ideally, all repositories will use two required checks on PRs:

* Static code checks
    * `ruff` and `ruff format`
    * `poetry` (in strict mode)
* Tests using `pytest`

Some tests require AWS credentials for a test user with the required permissions.
For this, we use secrets in the GitHub repositories.
<!--- TODO: Describe required permissions. -->

The checks can also be ran automatically:

* `pre-commit install` to automatically run the static code checks on commit
* `pytest tests/ [-m "aws or not(aws)"]` to automatically run the tests (the `aws` marker tags tests that require AWS credentials)
---
