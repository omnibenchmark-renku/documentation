
# 2. Set up a module

The basic steps required to set up a module are described in the README of your project. Depending on your project, you may also have to modify the following files of your project: 

- `Dockerfile`; extra linux (ubuntu) software requirements can be specified here (`R` and `Python` installed by default).

- `install.R`; to manage extra R dependencies

- `requirements.txt`; to manage extra Python dependencies

Detailed instruction can be found [in the Reku documentation](https://renku.readthedocs.io/en/latest/how-to-guides/general/install-packages.html#).

Upon any commit, Renku will automatically build a new Docker container based on these files. 

!!! info

    Having independent modules also means that you can test your code and work on your project without affecting the omnibenchmark until you submit it. Only when a project is included into an omnibenchmark 'orchestrator', it becomes part of the benchmark itself.

## Detailed instructions for 

- [**Input data**](01_data.md); Dataset with known ground-truth to run methods on.

- [**Methods**](02_method.md); Methods that will be applied on the (processed) datasets and have their output evaluated. Also use their 

- [**Metrics**](03_metric.md); Performance metrics used to evaluate methods outputs. 
