## Welcome to Omnibenchmark documentation

**Omnibenchmark** is a project that aims to provide
**community-driven**, **modular**, **extensible** and **continuously-updating** benchmarks. This page gives documentation for all the various facets of Omnibenchmark benchmarking. You may also want to visit the [Omnibenchmark project website](https://omnibenchmark.org), e.g., to see our [current continuous benchmarks](https://omnibenchmark.org/p/benchmarks/).

The Omnibenchmark framework is based on [Renku](https://renku.readthedocs.io/en/latest/introduction/index.html#renku-introduction) (and in particular, [Renku workflows](https://renku.readthedocs.io/en/latest/topic-guides/workflows/workflow-concepts.html)), an open and
collaborative data analysis platform. The Figure below gives a schematic for how Omnibenchmark builds on top of Renku (and [Gitlab](https://gitlab.com)).  Overall, the framework connects the various **ground-truth dataset**, **method** and **metric** repositories (a.k.a. *modules*) via a knowledge graph; all components of the system can be scrutinized and flexibly extended by members of the community.

![image](https://raw.githubusercontent.com/omnibenchmark/documentation/master/docs/images/omnibench_overview.png)

If you are interested in contributing a new **method**, **dataset** or **metric**, follow the documentation from the
[`Getting started`](01_getting_started/index.md) section, where you can also learn more about how Omnibenchmark works and how to extend it with a new module. [TODO: add a sentence for each tab and link them here].

