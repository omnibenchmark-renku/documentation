
# Hands-on sessions - omniclustering


### Resources 

- [Downloadable single-cell **datasets**](https://bioconductor.org/packages/devel/data/experiment/manuals/DuoClustering2018/man/DuoClustering2018.pdf)

- [List of wrappers of **methods** that perform single-cell clustering](https://github.com/markrobinsonuzh/scRNAseq_clustering_comparison/tree/master/Rscripts/clustering)

### Projects assignments

To avoid overlaps, please state which projects you are working on: 

[https://docs.google.com/spreadsheets/d/1nnPSALl6up0-yjXiWLPe0R7gSUyveozUQdVmZLu2fno/edit?usp=sharing](https://docs.google.com/spreadsheets/d/1nnPSALl6up0-yjXiWLPe0R7gSUyveozUQdVmZLu2fno/edit?usp=sharing)

### Project creation 

See the [Create a module - Clustering Omnibenchmark](../../01_getting_started/01_module_contr/create_module.md) links to prefilled projects. 

!!! warning
    Please use an explicit name for easier recognition and also to avoid names overlap. E.g. `method_[method name]_clustering`. 

### Project set up

See the dedicated guides for [Data modules](../../01_getting_started/01_module_contr/setup_module/01_data.md), [Methods modules](../../01_getting_started/01_module_contr/setup_module/02_method.md) or [Metric modules](../../01_getting_started/01_module_contr/setup_module/03_metric.md).

### Project population

You can find information about the required metadata (keywords, inputs, outputs, etc) in two ways: 

#### 1. Copy the structure of existing modules

!!! warning
    Please do not fork those to avoid transferring unintended metadata from the original project; rather, copy the module structure manually

- [Example Dataset](https://gitlab.renkulab.io/omb_benchmarks/omniclustering/dataset_Koh_clustering)

- [Example Method](https://gitlab.renkulab.io/omb_benchmarks/omniclustering/method_FlowSOM_clustering)

- [Example Metric](https://gitlab.renkulab.io/omb_benchmarks/omniclustering/metric_ShannonEntropy_clustering)

#### 2. Check the requirements

You can view the output requirements in this centralized repo: 

[https://github.com/omnibenchmark/omni_essentials/tree/main/schemas/omniclustering](https://github.com/omnibenchmark/omni_essentials/tree/main/schemas/omniclustering)

#### 3. Use the `omniValidator` inside your session 

The requirements for a given omnibenchmark and keyword can be displayed by running the following lines: 

```
import omniValidator as ov
ov.display_requirements(
    benchmark='BENCHMARK_NAME', 
    keyword='STEP_KEYWORD')
```

where  `BENCHMARK_NAME` is the name of the benchmark (`omniclustering`) and `STEP_KEYWORD` is the keyword associated to the type of module you are working on (`omniclustering_dataset`, `omniclustering_method` or `omniclustering_metric`).

!!! info
    `omniValidator` is ported with all projects and doesn't need to be installed.

### Output / Omnibenchmark status

An overview of the **Omnibenchmark components** is available on the [Omnibenchmark webpage](http://omnibenchmark.org/p/benchmarks/)

The **output** of the benchmark is available on our [shiny app server](https://bettr.omnibenchmark.org/omniclustering/).
