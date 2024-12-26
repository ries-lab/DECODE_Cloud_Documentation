# Welcome to the DECODE OpenCloud Docs

---

DECODE OpenCloud allows running algorithms on attached compute power in the spirit of SaaS.
This documentation is mainly meant for developers.
For users, you can find short high-level guides on how to use DECODE OpenCloud and attach new machine workers below.

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
Follow the usage instructions of the [JobFetcher](https://github.com/ries-lab/DECODE_Cloud_JobFetcher).
You will need:

* A Linux (virtual) machine with:
    * Docker
    * CUDA hardware to run GPU jobs
    * [nvidia-container-toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html) to run GPU jobs
* A registered worker account (you can use the [website frontend](https://ries-lab.github.io/DECODE_Cloud_UserFrontend/#/) to register)

---
