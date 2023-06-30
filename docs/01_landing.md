
# Motivation

Benchmarking is a critical step for the development of bioinformatic methods and provides important insights for their application.

The current benchmarking scheme has many limitations:

- It is a snapshot of the available methods at a certain time point and often already outdated when published.

- Comparison of benchmarks is challenging: different procedures, different datasets, different evaluation criteria, etc.

- All of the above can lead to different conclusions among benchmarks made at different time points or at different groups.

## Concept

Omnibenchmark is a modular and extensible framework based on a free open-source analytic platform, [Renku](https://renkulab.io/), to offer a continuous and open community benchmarking system.

The framework consists of data, method and metric repositories (or “modules”) that share data components and are tracked via a knowledge graph from the Renku system. The results can be displayed in an interactive dashboard to be openly explored by anyone looking for recommendations of tools. New data, methods or metrics can be added by the community to extend the benchmark.

Some key features of our benchmarking framework:

- Periodical updates of the benchmark to provide up-to-date results

- Easy extensibility through templates for data, methods or metrics

- Following FAIR principles by using software containers, an integration with Gitlab and full provenance tracking (inputs, workflows and generated files)

- Flexibility to work with different benchmarking structures, topics and programming languages.

## Prototypes

We are currently building multiple prototypes for community-based
benchmarking of single cell batch correction methods. 

The list of current benchmarks and their results can be explored on the official Omnibenchmark website: 

[http://omnibenchmark.org/](http://omnibenchmark.org/)