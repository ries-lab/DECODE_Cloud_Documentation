# Data handling

---

## Files

Users and workers download and upload their input/output files to S3 storage using pre-signed URLs. That is, the process is two-step: (1) request to the decode user/worker-facing API to get a pre-signed URL; (2) request to the AWS S3 API using the obtained pre-signed URL.

DECODE Cloud is not meant as a storage solution, so files should be short-lived on the application. The S3 bucket is configured to expire files after two weeks.

---

## Databases

Either AWS RDS storage and local sqlite storage can be configured.
Currently, the stack uses sqlite for cost reasons (simple application, low concurrency, no need for anything fancy).
The sqlite database is backed up periodically and on application shutdown (e.g. when the API docker image is upgraded) to S3, and read on startup.
