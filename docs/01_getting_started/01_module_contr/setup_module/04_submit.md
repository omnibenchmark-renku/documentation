# Submit a module to an Omnibenchmark

An omnibenchmark module will be able to import datasets from other modules and export its output to others. However, it still needs to be integrated in an **Omnibenchmark orchestrator**. An orchestrator is an omnibenchmark project which *orchestrates* the deployment, running and testing of each pieces of an Omnibenchmark. 

## Integrate a module to an orchestrator

The list of current Omnibenchmarks and their related orchestrator can be found on the [Omnibenchmark dashboard](http://omnibenchmark.org/p/benchmarks/): 

- [Omni-batch orchestrator](https://gitlab.renkulab.io/omnibenchmark/omni-batch-py/orchestrator-py)

- [Omniclustering orchestrator](https://gitlab.renkulab.io/omb_benchmarks/omniclustering/orchestrator)

- [Omniclustering (pinned versions) orchestrator](https://gitlab.renkulab.io/omb_benchmarks/omniclustering_pinned/orchestrator)

- [Omni spatial clustering orchestrator](https://gitlab.renkulab.io/omb_benchmarks/omni_srt_clustering/orchestrator)

To integrate your (populated and tested) module: 

- Follow the link to the relevant orchestrator
    
- Open a new issue and describe briefly the aim of your module (data/ method/ metric module ?) and provide a link to it
    
- The development team will check your module and integrate it in the Orchestrator worfkflow. When it is done, you will be able to view the result of your module on the shiny app. 