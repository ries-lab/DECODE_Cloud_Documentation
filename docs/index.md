# Welcome to the DECODE OpenCloud Docs

---

DECODE OpenCloud allows running algorithms on attached compute power in the spirit of SaaS.
Below, you can find short high-level guides on how to use DECODE OpenCloud and attach new machine workers.

---

## Running an implemented algorithm
The easiest way is to use our minimal [website frontend](https://ries-lab.github.io/DECODE_Cloud_UserFrontend/#/).

The frontend contains guides on how to run the different supported algorithms.
On a high level, you will need to:

* Register an account using the frontend (and wait for the account to be approved)
* Upload input data
* Start the job
* Retrieve the results after the job has finished (you will be notified by email)

---

## Attaching compute power
**Requirements**

* Linux (virtual) machine with:
    * CUDA hardware
    * Docker
* Registered worker account (you can use the [website frontend](https://ries-lab.github.io/DECODE_Cloud_UserFrontend/#/) to register)

**Steps**
Follow the usage instructions of the [JobFetcher](https://github.com/ries-lab/DECODE_Cloud_JobFetcher).

---
