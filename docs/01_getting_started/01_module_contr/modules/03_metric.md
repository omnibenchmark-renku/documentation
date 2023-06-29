# Metric modules

A metric module imports method result datasets and runs one evaluation on them. Evaluation results are added to a [renku dataset](https://renku.readthedocs.io/en/latest/topic-guides/data.html#), that can be summarized and explored in an {ref}`omnibenchmark bettr dashboard <section-output>`. Benchmark specific requirements like file formats, types and prefixes can be checked at the [omnibenchmark webside](https://omnibenchmark.pages.uzh.ch/omni_dash/index.html). Usually a metric module contains two workflows, one to evaluate the results and one to generate the **`metric info file`**. Most metric modules contain 5 main files:

## 1. The config.yaml file

All information about the module are specified in the {ref}`config.yaml file <section-config>`:

```yaml
---
data:
    name: "metric name"
    title: "metric title"
    description: "Short description of the metric"
    keywords: ["MODULE_KEY"]
script: "path/to/module_script"
benchmark_name: "OMNIBENCHMARK_TYPE"
inputs:
    keywords: ["INPUT_KEY1", "INPUT_KEY2"]
    files: ["input_file_name1", "input_file_name2"]
    prefix:
        input_file_name1: "_INPUT1_"
        input_file_name2: "_INPUT2_"
outputs:
    files:
        metric_result: 
            end: "FILE1_END"
```

Entries in capital letters depend on the specifications at the [omnibenchmark webside](https://omnibenchmark.pages.uzh.ch/omni_dash/index.html).

## 2. The info_config.yaml file

All information about the module are specified in the {ref}`config.yaml file <section-config>`:

```yaml
---
data:
    name: "metric name"
script: "src/generate_metric_info.py"
benchmark_name: "OMNIBENCHMARK_TYPE"
outputs:
    files:
        metric_info: 
            end: "json"
    file_mapping:
        mapping1:
          output_files:
            metric_info: "path/to/metric_name_info.json"
```

## 3. The run_workflow.py file

This file is to generate, run and update the modules dataset and workflow. A most basic script to do so looks like this:

```python
# Load modules
from omnibenchmark.utils.build_omni_object import get_omni_object_from_yaml
from omnibenchmark.renku_commands.general import renku_save

# Build an OmniObject from the config.yaml file
omni_obj = get_omni_object_from_yaml('src/config.yaml')

# Import and update inputs/parameter and update object accordingly 
omni_obj.update_object()
renku_save()

# Create the output dataset
omni_obj.create_dataset()
renku_save()

## Run and update the workflow on all inputs and parameter combinations
omni_obj.run_renku()
renku_save()

## Add files to output dataset
omni_obj.update_result_dataset()
renku_save()

###################### Generate info file ######################
# Build an OmniObject from the info_config.yaml file
omni_info = get_omni_object_from_yaml('src/info_config.yaml')
omni_info.wflow_name = "metric_info"

## Run and update workflow
omni_info.run_renku()
renku_save()

## Update output dataset 
omni_info.update_result_dataset()
renku_save()
```

## 4. The module script

This is the script to load the dataset and to convert its files into the expected format.
Omnibenchmark accepts any kind of script and its maintenance and content is up to the module author.
Omnibenchmark calls this script from the command line. If you use another language than R, Python, Julia or bash, specify the interpreter to use in the corresponding field of the {ref}`config.yaml file <section-config>` file.

:::note
All input and output files and if applicable parameter need to be parsed from the command line in the format:
`--ARGUMENT_NAME ARGUMENT_VALUE`
:::

In Python [argparse](https://pypi.org/project/argparse/) can be used to parse command arguments like this:

```python
# Load module
import argparse

# Get command line arguments and store them in args
parser=argparse.ArgumentParser()
parser.add_argument('--argument_name', help='Description of the argument')
args=parser.parse_args()

# Call the argument
arg1 = args.argument_name

```

In R we recommend to use the [optparse](https://cran.r-project.org/web/packages/optparse/index.html) package:

```r
# Load package
library(optparse)

# Get list with command line arguments by name
option_list = list(
    make_option(c("--argument_name"), type="character", default=NULL, 
              help="Description of the argument", metavar="character")
); 
 
opt_parser = OptionParser(option_list=option_list);
opt = parse_args(opt_parser);

# An useful error if the argument is missing
if (is.null(opt$argument_name)){
  print_help(opt_parser)
  stop("Argument_name needs to be specified, but is missing.n", call.=FALSE)
}

# Call the argument
arg1 <- opt$argument_name
```

## 5. The generate_metric_info.py file

This file could also be written in R or any other language. It should return a json file with the below fields with the metrics information.

```python
import argparse
import json

parser=argparse.ArgumentParser()
parser.add_argument('--metric_info', help='Path to the metric info json file')
args=parser.parse_args()

metric_info = {
    'flip': False,
    'max': 1,
    'min': 0,
    'group': "METRIC_GROUP",
    'name': "metric_name",
    'input': "metric_input_type"
}

with open(args.metric_info, "w") as fp:
    json.dump(metric_info , fp, indent=3) 
```
