# Project setup


## Start a renku project

Each module of omnibenchmark is an independent renku project. It can be setup as a new **GitLab** project with the following features:

* A **`Dockerfile`** to specify the computational environment to run your project in.
* A **`gitlab-ci.yaml`** file to continuously build and update the projects Docker container and module script.
* **`Git LFS`** for large file storage.
* Further project organisation files like a **`.gitignore`**, **`.gitattributes`**, **`.renkulfsignore`**, **`.dockerignore`**.

Having independent modules also means that you can test your code and work on your project without being included into an existing benchmark. Only when a project is included into an omnibenchmark orchestrator it becomes part of the benchmark itself (explained latter Omnibenchmark modules) .

To start a new renku project login at the [renku](https://renkulab.io) with your GitHub account, OrchID or SWITCH-eduID or register a renku account.

!!! info

    **Start a new renku project**: [renkulab.io](https://renkulab.io) -> projects -> + New project

Select a name, namespace, description and suitable template and create a new project. There are no specific requirements for omnibenchmark at this stage.
> Read more about renku projects [here](https://renku.readthedocs.io/en/latest/index.html).

## Specify your modules environment

A module environment can be specified by modifying the **`Dockerfile`**.
To run omnibenchmark you need the python modules [renku-python](https://pypi.org/project/renku/) and [omnibenchmark](https://pypi.org/project/omnibenchmark/) installed. There are no further requirements, but it can be useful to follow the [template structure](https://renku.readthedocs.io/en/latest/topic-guides/docker.html#) in the automatically generated **`Dockerfile`**.  
Depending on what template you chose a renku *base image* will be selected at the top. Make sure this base image is reasonable for your module (e.g. choose one with R installed if your module calls R code).
Extra *linux (ubuntu) software requirements* can be specified within the **`Dockerfile`**, while *python modules* are typically defined in the **`requirements.txt`** file or using conda’s environment management system in the **`environment.yaml`** file and installation of R packages is specified in the **`install.R`** file. Detailed instruction can be found [here](https://renku.readthedocs.io/en/latest/how-to-guides/general/install-packages.html#). The automatically generated **`Dockerfile`** already contains commands to install [renku-python](https://pypi.org/project/renku/) at the bottom, but you need to specify [omnibenchmark](https://pypi.org/project/omnibenchmark/) with the version you want to use as software requirement:

!!! info
    **Add this line to requirements.txt**:
    omnibenchmark==VERSION

If you want to call  R code and you chose a R template when creating the renku project the **`install.R`** file is automatically generated upon project creation. Otherwise make sure you switch to a `base image` with R installed and add the **`install.R`** file manually, as well as the following lines to your **`Dockerfile`**:

``` Dockerfile
# install the R dependencies
COPY install.R /tmp/
RUN R -f /tmp/install.R
```

## Run your code

After each commit renku builds automatically a Docker container with your specified requirements. You can start an [interactive session](https://renku.readthedocs.io/en/latest/tutorials/first_steps/02_start_an_interactive_session.html) using the latest container at [renkulab.io](https://renkulab.io) or [work with renku on your own machine](https://renku.readthedocs.io/en/latest/how-to-guides/own_machine/index.html).
