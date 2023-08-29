# Metric modules

A metric module imports method result datasets and runs one evaluation on them. Evaluation results are added to a [renku dataset](https://renku.readthedocs.io/en/latest/topic-guides/data.html#), that can be summarized and explored in an [omnibenchmark dashboard](https://omnibenchmark.pages.uzh.ch/omb-site/p/results/).  Benchmark specific requirements like file formats, types and prefixes can be checked at the [omnibenchmark website](http://omnibenchmark.org/p/benchmarks/). Usually a metric module contains two workflows, one to evaluate the results and one to generate the **`metric info file`**. Most metric modules contain 5 main files:

## 1. The config.yaml file


The `config.yaml` (in your `src` directory) defines the metadata associated to your method and how it will be tested. Upon creation of a method project, a `src/config.yaml` is already created and some metadata already pre-filled. Let's have a look how you can configure it; 


=== "config.yaml"
    ```{ .yaml }
    ---
    data:
        name: "metric name" # (1)! 
        title: "metric title"
        description: "Short description of the metric"
        keywords: ["MODULE_KEY"] # (2)!
    script: "path/to/module_script" # (3)!
    benchmark_name: "OMNIBENCHMARK_TYPE" # (4)!
    inputs:
        keywords: ["INPUT_KEY"] # (5)!
        files: ["input_file_name1", "input_file_name2"]
        prefix:
            input_file_name1: "_INPUT1_"
            input_file_name2: "_INPUT2_"
    outputs:
        files:
            metric_result: # (6)!
                end: "FILE1_END"
    ```

     1. Specifies the method name. Do not use special characters outside `-` and `_`. 

     2. A keyword that is used for all metrics of this omnibenchmark. By default/ convention: `[omnibenchmark name]_metric_`. If you are not sure, you can check the keywords already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). 

     3. Relative path to the script to execute. More information about the script in the `The module script` section below. 

     4. The name of the omnibenchmark this method belongs to. If you are not sure, you can check the names already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). 

    5. The keyword(s) of the methods results to evaluate. If you are not sure, you can check the keywords already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/).

    6. Name that will be passed to the output of the scripts (e.g. `metric_result`) and their file extension (`end:` field). It must match the arguments of you script (see `The module script` section below).


More information about the `config.yaml` file in the [Setup a config.yaml](../../../03_howto/00_config_yaml.md) section.


## 2. The module script

This is the script to load the method results and evaluate them using metrics.
Omnibenchmark accepts any kind of script and its maintenance and content is up to the module author. By default, an R and Python script are provided in the `src` folder that you can use to add your method's wrapper.
Omnibenchmark calls this script from the command line. If you use another language than R or Python (e.g. Julia or bash) specify the interpreter to use in the corresponding field of the `config.yaml` file (`script:` field of `config.yaml`).

In Python [argparse](https://pypi.org/project/argparse/) can be used to parse command arguments. In R we recommend to use the [optparse](https://cran.r-project.org/web/packages/optparse/index.html) package. A basic example is provided in the two example scripts provided in your project. 

!!!note
    All input and output files will automatically be parsed from the command line in the format:
    `--ARGUMENT_NAME ARGUMENT_VALUE`

Below is a dummy example of how the R/Python scripts can look like given the example `config.yaml` from the previous section;  


=== "metric.py" 
    ```{ .python .annotate hl_lines="6 7 8 20"}
    # Load module
    import argparse

    # Get command line arguments and store them in args
    parser=argparse.ArgumentParser()
    parser.add_argument('--input_file_name1', help='Description of the argument') # (1)!
    parser.add_argument('--input_file_name2', help='Description of the argument')
    parser.add_argument('--metric_result', help='Description of the argument')
    args=parser.parse_args()

    # Call the argument
    input_file_name1 = args.input_file_name1
    input_file_name2 = args.input_file_name2
    metric_result = args.metric_result

    # Run the metric to benchmark the methods results
    # output=...

    # Save as csv using parsed argument
    output.to_csv(metric_result, index=False) # (2)!

    ```

    1.  Argument names that correspond to the  `inputs` and `outputs` fields specified in `config.yaml`. 

    2. No need to specify the file extension here. For outputs, it will automatically be parsed based on the 'end:' that you specified in your config.yaml. 


=== "metric.R" 
    ```{ .R .annotate hl_lines="6 7 8 9 10 11 26"}
    # Load package
    library(optparse)

    # Get list with command line arguments by name
    option_list = list(
        make_option(c("--input_file_name1"), # (1)!
            type="character", default=NULL, help=" "), 
        make_option(c("--input_file_name2"),
            type="character", default=NULL, help=" "), 
        make_option(c("--method_result"),
            type="character", default=NULL, help=" ")
    ); 
    
    opt_parser = OptionParser(option_list=option_list);
    opt = parse_args(opt_parser);

    # Call the argument
    input_file_name1 <- opt$input_file_name1
    input_file_name2 <- opt$input_file_name2
    method_result <- opt$method_result

    # Run the method to be benchmarked and create 2 outputs
    # output <- ...

    # Save as csv using parsed argument
    write.csv(output, file = metric_result) # (2)!
    ```

    1.  Argument names that correspond to the  `inputs` and `outputs` fields specified in `config.yaml`.

    2. No need to specify the file extension here. For outputs, it will automatically be parsed based on the 'end:' that you specified in your config.yaml. 



## 3. The generate_metric_info.py file (optional)

This file could also be written in R or any other language but one is automatically provided in your `src` folder. You can change the metadata of the metric (indicated by a `+`), which will be used for its display into the dashboard showing the metric results. All options can also be left as default. 

=== "generate_metric_info.py"
    ```{ .python }
    import argparse
    import json

    parser=argparse.ArgumentParser()
    parser.add_argument('--metric_info', help='Path to the metric info json file')
    args=parser.parse_args()

    metric_info = {
        'flip': False, # (1)!
        'max': 1, # (2)!
        'min': 0, # (3)!
        'group': "METRIC_GROUP", # (4)!
        'name': "metric_name", # (5)!
        'input': "metric_input_type" # (6)!
    }

    with open(args.metric_info, "w") as fp:
        json.dump(metric_info , fp, indent=3) 
    ```

    1. Logical scalar; whether or not to flip the sign of the metric values on the dashboard app. 

    2. Possible maximum value of the metric. 

    3. Possible minimum value of the metric. 

    4. If different groups of metrics are added, a name can be given for grouping. Can also be left as 'default'. 

    5. Metric name that will be displayed on the dashboard. 

    6. Prefix to name the result files. Can be left as default; 'metric_res'. 




## 4. The info_config.yaml file (optional)

This file contains file to complement the `generate_metric_info.py`. All fields can be left as default. 

## 5. The run_workflow.py file


This file is to run and update the metric project. This is a standard script that will adapt to your `config.yaml` and dataset script and **does not need to be (and should not be!) modified**. Below is a copy of the `run_workflow.py` that you have in your project, with some explanations: 


=== "run_workflow.py"
    ```{ .python }
    from omnibenchmark.utils.build_omni_object import get_omni_object_from_yaml
    from omnibenchmark.renku_commands.general import renku_save
    import omniValidator as ov
    renku_save()

    ## Load config
    omni_obj = get_omni_object_from_yaml('src/config.yaml')  # (1)!

    ## Update object and download input datasets
    omni_obj.update_object(all=True)  # (2)!
    renku_save()

    ## Check object
    print(
        f"Object attributes: \n {omni_obj.__dict__}\n"
    )
    print(
        f"File mapping: \n {omni_obj.outputs.file_mapping}\n"
    )
    print(
        f"Command line: \n {omni_obj.command.command_line}\n"  # (3)!
    )

    ## Create output dataset
    omni_obj.create_dataset() # (4)!

    ## Run workflow
    ov.validate_requirements(omni_obj)
    omni_obj.run_renku(all=False)  # (5)!
    renku_save()

    ## Update Output dataset
    omni_obj.update_result_dataset() # (6)!
    renku_save()

    ###################### Generate info file ###################### # (7)!

    omni_info = get_omni_object_from_yaml('src/info_config.yaml') 
    omni_info.wflow_name = "{{ sanitized_project_name }}_info"

    ## Check object
    print(
        f"Object attributes: \n {omni_info.__dict__}\n"
    )
    print(
        f"File mapping: \n {omni_info.outputs.file_mapping}\n"
    )
    print(
        f"Command line: \n {omni_info.command.command_line}\n"
    )

    ## Run workflow
    omni_info.run_renku()
    renku_save()

    ## Update Output dataset
    omni_info.update_result_dataset()
    renku_save()

    ################################################################

    ```

    1. Load the options of your dataset into an Omnibenchmark object. 

    2. Downloads the datasets specified in `inputs: `. It case there are too many data to download, `all` can be set to False to only download a subset of the input data. 

    3. This will show the command line and the parsed arguments that are going to be called to run the method. Can also be used for debugging. 

    4. Pay attention to any warning messages about names conflicts (it can lead to unpredictable conflicts latter in the benchmark). If it happens, change the 'name' in your config.yaml and rerun all previous command lines.

    5. This will run your script with parsed arguments. 

    6. This will upload the methods results to make it recognizable by further modules. 

    7. Similar as before, but for the metrics metadata. 

Once your module is completed and tested, [you can submit it to the team](04_submit.md). 