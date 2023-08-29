
# The config.yaml file

Usually all specific information about a benchmark project can be specified in a `config.yaml` file. Below we show an example with all standard fields and explanations to them. Many fields are optional and do not apply to all modules. All unneccessary fields can be skipped. There are further optional fields for specfic edge cases, that are described in an extra `config.yaml` file. In general the `config.yaml` file consists of a data, an input, an output and a parameter section as well as a few extra fields to define the main benchmark script and benchmark type. Except for the data section the other sections are optional. Multiple values can be parsed as lists.

```yaml
# Data section to describe the object and the associated (result) dataset
data:
    # Name of the dataset
    name: "out_dataset"
    # Title of the dataset (Optional)
    title: "Output of an example OmniObject"
    # Description of the dataset (Optional)
    description: "This dataset is supposed to store the output files from the example omniobject"
    # Dataset keyword(s) to make this dataset reachable for other projects/benhcmark components
    keywords: ["example_dataset"]
# Script to be run by the workflow associated to the project
script: "path/to/method/dataset/metric/script.py"
# Interpreter to run the script (Optional, automatic detection)
interpreter: "python"
# Benchmark that the object is associated to.
benchmark_name: "omni_celltype"
# Orchestrator url of the benchmark (Optional, automatic detection)
orchestrator: "https://www.orchestrator_url.com"
# Input section to describe output file types. (Optional)
inputs:
    # Keyword to find input datasets, that shall be imported 
    keywords: ["import_this", "import_that"]
    # Input file types
    files: ["count_file", "dim_red_file"]
    # Prefix (part of the filename is sufficient) to automatically detect file types by their names
    prefix:
        count_file: "counts"
        dim_red_file: ["features", "genes"]
# Output section to describe output file types. (Optional)
outputs:
    # Output filetypes and their endings
    files:
        corrected_counts: 
            end: ".mtx.gz"
        meta:
            end: ".json"
# Parameter section to describe the parameter dataset, values and filter. (Optional)
parameter:
    # Names of the parameter to use
    names: ["param1", "param2"]
    # Keyword(s) used to import the parameter dataset
    keywords: ["param_dataset"]
    # Filter that specify limits, values or combinations to exclude
    filter:
        param1:
            upper: 50
            lower: 3
            exclude: 12
    param2:
        "path/to/file/with/parameter/combinations/to/exclude.json"
```

Specific fields, that are only relevant for edge cases. These fields have their counterparts in the generated {ref}`OmniObject <section-classes>`.
Changes of the attributes of the OmniObject instance have the same effects, but come with the flexibility of python code. 

```yaml
# Command to generate the workflow with (Optional, automatic detection)
command_line: "python path/to/method/dataset/metric/script.py --count_file data/import_this_dataset/...mtx.gz"
inputs:
    # Datasets and manual file type specifications (automatic detection!)
    input_files:
        import_this_dataset:
            count_file: "data/import_this_dataset/import_this_dataset__counts.mtx.gz"
            dim_red_file: "data/import_this_dataset/import_this_dataset__dim_red_file.json"
    # (Dataset) name that default input files belong to (Optional, automatic detection)
    default: "import_this_dataset"
    # Input dataset names that should be ignored (even if they have one of the specified input keywords assciated)
    filter_names: ["data1", "data2"]
outputs:
    # Template to automatically generate output filenames (Optional - recommended for advanced user only)
    template: "data/${name}/${name}_${unique_values}_${out_name}.${out_end}"
    # Variables used for automatic output filename generation (Optional - recommended for advanced user only)
    template_vars:
        vars1: "random"
        vars2: "variable"
    # Manual specification of mapping for output files and their corresponding input files and parameter values (automatic detection!)
    file_mapping:
        mapping1: 
            output_files:
                corrected_counts: "data/out_dataset/out_dataset_import_this__param1_10__param2_test_corrected_counts.mtx.gz"
                meta: "data/out_dataset/out_dataset_import_this__param1_10__param2_test_meta.json"
        input_files:
            count_file: "data/import_this_dataset/import_this_dataset__counts.mtx.gz"
            dim_red_file: "data/import_this_dataset/import_this_dataset__dim_red_file.json"
        parameter:
            param1: 10
            param2: "test"
    # Default output files (Optional, automatic detection)
    default:
        corrected_counts: "data/out_dataset/out_dataset_import_this__param1_10__param2_test_corrected_counts.mtx.gz"
        meta: "data/out_dataset/out_dataset_import_this__param1_10__param2_test_meta.json"
parameter:
    default:
        param1: 10
        param2: "test"
```
