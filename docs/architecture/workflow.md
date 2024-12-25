# Job workflow

---

In the example of the training of a DECODE network, the typical workflow of a job in DECODE OpenCloud looks like this:

![](./graphics/training_sequence_diagram.drawio.svg)


When submitting a job, users (can) select different input folders:

 - **Configuration**: typically, a single file defining the job parameters (e.g., for DECODE v0.10, the folder in which the .yaml parameter file is found; for later versions of DECODE, this directory contains the whole hydra configuration).
 - **Data**: folder(s) containing the input data (e.g., the bead calibration for DECODE training, and the input frames for DECODE fitting).
 - **Artifact**: we call artifacts specific outputs of jobs (e.g., trained DECODE models) that will be used for later jobs (e.g., fitting data with DECODE).

---
