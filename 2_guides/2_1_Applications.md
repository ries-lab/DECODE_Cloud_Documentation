# Supported applications
 - [DECODE](https://github.com/ries-lab/DECODE_Internal/tree/dockerfile_stable): SMLM.
 - [Comet](https://github.com/nolan1999/Comet/tree/docker): drift correction.

# Publishing a new application
### Step 1: Dockerize the application.
The image should:
 - not have an ENTRYPOINT
 - have no free parameters, except input files (parameters can then be set in the input files)
 - either save its outputs in a single, specific directory, or allow setting the output directory
