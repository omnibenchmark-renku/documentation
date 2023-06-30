## Omnibenchmark modules


Omnibenchmark uses the **Renku** platform to run open and continuous benchmarks. 
To contribute an independent module to one of the existing benchmarks start by creating a `new renku project <https://omnibenchmark.readthedocs.io/en/latest/start/01_project_setup.html#start-a-renku-project>`_. 
Each module consists of a Docker image, that defines its environment, a dataset to store outputs and metadata, a workflow that describes how to generate outputs and input and parameter datasets with input files and parameter definitions, if they are used. 
Thus each module is an independent benchmark part and can be run, used and modified independently as such. 
Modules are connected by importing (result) datasets from other modules as input datasets and will automatically be updated according to them.
All relevant information on how to run a specific module are stored as `OmniObject <https://omnibenchmark.readthedocs.io/en/latest/topic_guides/classes/01_omni_object.html>`_.
The most convenient way to generate an instance of an **OmniObject** is to build it from a `config.yaml <https://omnibenchmark.readthedocs.io/en/latest/topic_guides/02_config_yaml.html>`_ file <section-config>`.
This structure is universal all modules can be build upon it. 
In the following we describe example setups for different benchmark stages (dataset modules, methods modules, metrics modules). 
Depending on the benchmark these can be flexibly extended and adapted to the benchmark structure.

