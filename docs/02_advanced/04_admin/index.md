## Administering omnibenchmark

### Components

Omnibenchmark is designed as a SaaS. Omnibenchmark's components are modular, and some you can deploy in your own to have extra control. [omnibenchmark.org](https://omnibenchmark.org) already provides all these components, so this guide only matters if you'd like to replace some of the `default` components.

- renkulab and gitlab deployment
    - `default`: renkulab and gitlab deployment from [renkulab.io](https://renkulab.io).
    - `self`: k8s and following [renku's admin docs](https://renku.readthedocs.io/en/stable/how-to-guides/admin/index.html).
- gitlab runners 
    - `default`: provided by renkulab.io.
    - `self`: registered runners in any architecture (laptops, HPC, GPU-powered machines etc).
- omnibenchmark triplestore
    - `default`: apache jena from robinsonlab (ask us for details).
    - `self`: deploy a jena/fuseki to have full control on your triples.
- centralized benchmark listing/json.
    - `default`: robinsonlab (ask us for details, queried by [omb-py](https://github.com/omnibenchmark/omnibenchmark-py/blob/35e468cb48b814e15deb31498b35d13566f5573d/omnibenchmark/utils/local_cache/sync.py#L21)).
- bettr deployment 
    - `default`: shiny-server by robinsonlab (ask us for details).
    - `self`: set up a shiny-server (perhaps using singularity).
- Web dashboard
    - `default`: [omnibenchmark.org](https://omnibenchmark.org) from robinsonlab (ask us for details, [code](https://renkulab.io/gitlab/omnibenchmark/omni_site)).

Besides, omnibenchmark relies on a set of python modules and R packages, which also need to be cross-compatible and compatible with your renkulab deployment, including:

- [omnibenchmark python](https://pypi.org/project/omnibenchmark/)
    - tip: needs to be compatible with [renku python](https://pypi.org/project/renku/)
- [omni validator](https://github.com/omnibenchmark/omniValidator)
- [omni_cli](https://renkulab.io/gitlab/btraven/omni-cli)
- [bettr](https://github.com/federicomarini/bettr)

## Configuration guides

Mind some of our docs are missing; drafts are listed as &#10003;, ready-to-use docs as 	&#9989;.

1. [Services overview](01_services.md)  &#10003;
2. [Start a new benchmark](02_new_benchmark.md) &#10003;
3. [Set up a triplestore](03_triplestore.md) 
4. [Register a runner](04_runner.md) &#9989;
5. [Serve bettr](05_bettr.md) &#9989;
6. [Serve a dashboard](06_dashboard.md) 
7. [Deploy renkulab](07_renkulab.md) 

## Contact

To get tokens/authentication details: mark.robinson@uzh.ch, izaskun.mallona@uzh.ch
