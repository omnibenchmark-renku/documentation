# Data modules

Data modules are modules that define input datasets and bundle them into a [renku dataset](https://renku.readthedocs.io/en/latest/topic-guides/data.html#), that can be imported by other projects. Benchmark specific requirements like file formats, types and prefixes can be checked at the [omnibenchmark webside](https://omnibenchmark.pages.uzh.ch/omni_dash/index.html). Most modules contain 3 main files:

## 1. The config.yaml file

All information about the module are specified in the {ref}`config.yaml file <section-config>`:

```yaml
---
data:
    name: "dataset-name"
    title: "dataset title"
    description: "A new dataset module for omnibenchmark"
    keywords: ["MODULE_KEY"]
script: "path/to/module_script"
outputs:
    files:
        data_file1: 
            end: "FILE1_END"
        data_file2:
            end: "FILE2_END"
        data_file3:
            end: "FILE3_END"
benchmark_name: "OMNIBENCHMARK_TYPE"
```

Entries in capital letters depend on the specifications at the [omnibenchmark webside](https://omnibenchmark.pages.uzh.ch/omni_dash/index.html).

## 2. The run_workflow.py file

This file is to generate, run and update the modules dataset and workflow. A most basic script to do so looks like this:

```python
# Load modules
from omnibenchmark.utils.build_omni_object import get_omni_object_from_yaml
from omnibenchmark.renku_commands.general import renku_save

# Build an OmniObject from the config.yaml file
omni_obj = get_omni_object_from_yaml('src/config.yaml')

# Create the output dataset
omni_obj.create_dataset()
renku_save()

## Run and update the workflow
omni_obj.run_renku()
renku_save()

## Add files to output dataset
omni_obj.update_result_dataset()
renku_save()
```

## 3. The module script

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
