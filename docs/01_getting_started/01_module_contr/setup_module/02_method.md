# Method modules

A method module imports all datasets of a benchmark or all preprocessed inputs and runs one benchmarking method on them. Method outputs are added to a [renku dataset](https://renku.readthedocs.io/en/latest/topic-guides/data.html#), that can be imported by metric projects to evaluate them. Benchmark specific requirements like file formats, types and prefixes can be checked at the [omnibenchmark website](http://omnibenchmark.org/p/benchmarks/). 

To test a method, a **parameter dataset** is imported to test the method on different parameter settings. Usually the parameter space is defined Valid parameters can be specified in the config.yaml file or on a [filtered set of parameters](../../../03_howto/05_filter.md) (e.g. parameter limits, specific values and parameter and input file combinations). Usually a method module contains only one workflow, that is automatically run with all valid parameter and input file combinations. Most modules contain 3 main files:

## 1. The config.yaml file


The `config.yaml` (in your `src` directory) defines the metadata associated to your method and how it will be tested. Upon creation of a method project, a `src/config.yaml` is already created and some metadata already pre-filled. Let's have a look how you can configure it; 

=== "config.yaml"
    ```{ .yaml }
    ---
    data:
        name: "method name" # (1)! 
        title: "method title"
        description: "Short description of the method"
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
            method_result1: # (6)!
                end: "FILE1_END"
            method_result2:
                end: "FILE2_END"
    parameter:
        keywords: ["PARAMETER_KEY"]# (7)!
        names: ["parameter1", "parameter2"] 
    ```

    1. Specifies the method name. Do not use special characters outside `-` and `_`. 

    2. A keyword that is used for all methods of this omnibenchmark. By default/ convention: `[omnibenchmark name]_method`. If you are not sure, you can check the keywords already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). 

    3. Relative path to the script to execute. More information about the script in the `The module script` section below. 

    4. The name of the omnibenchmark this method belongs to. If you are not sure, you can check the names already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). 

    5. The keyword(s) of the input dataset to test the method on. If you are not sure, you can check the keywords already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). 

    6. Name that will be passed to the output of the scripts (e.g. `method_result1`) and their file extension (`end:` field). It must match the arguments of you script (see `The module script` section below). 

    7.  The keyword of the input parameters to test the method on and their names. If you are not sure, you can check the keywords already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). You can also specify define specific parameters, see the dedicated documentation of the [`config.yaml` file](../../../03_howto/00_config_yaml.md).


More information about the `config.yaml` file in the [Setup a config.yaml](../../../03_howto/00_config_yaml.md) section,


## 2. The module script

This is the script to apply a metric on test datasets.
Omnibenchmark accepts any kind of script and its maintenance and content is up to the module author. By default, an R and Python script are provided in the `src` folder that you can use to add your method's wrapper.
Omnibenchmark calls this script from the command line. If you use another language than R or Python (e.g. Julia or bash) specify the interpreter to use in the corresponding field of the `config.yaml` file (`script:` field of `config.yaml`).

In Python [argparse](https://pypi.org/project/argparse/) can be used to parse command arguments. In R we recommend to use the [optparse](https://cran.r-project.org/web/packages/optparse/index.html) package. A basic example is provided in the two example scripts provided in your project. 

!!!note
    All input and output files will automatically be parsed from the command line in the format:
    `--ARGUMENT_NAME ARGUMENT_VALUE`

Below is a dummy example of how the R/Python scripts can look like given the example `config.yaml` from the previous section;  


=== "method.py" 
    ```{ .python .annotate hl_lines="6 7 8 9 10 11 27 28"}
    # Load module
    import argparse

    # Get command line arguments and store them in args
    parser=argparse.ArgumentParser()
    parser.add_argument('--input_file_name1', help='name') # (1)!
    parser.add_argument('--input_file_name2', help='name') 
    parser.add_argument('--method_result1', help='name') 
    parser.add_argument('--method_result2', help='name') 
    parser.add_argument('--parameter1', help='name') 
    parser.add_argument('--parameter2', help='name') 
    args=parser.parse_args()

    # Call the arguments
    input_file_name1 = args.input_file_name1 
    input_file_name2 = args.input_file_name2 
    method_result1 = args.method_result1 
    method_result2 = args.method_result2 
    parameter1 = args.parameter1 
    parameter2 = args.parameter2 

    # Run the method to be benchmarked and create 2 outputs
    # output1=...
    # output2=...

    # Save as csv using parsed argument
    output1.to_csv(method_result1, index=False) # (2)!
    output2.to_csv(method_result2, index=False) 

    ```

    1.  Argument names that correspond to the  `inputs`, `outputs` and `parameter` fields specified in `config.yaml`. 

    2. No need to specify the file extension here. For outputs, it will automatically be parsed based on the 'end:' that you specified in your config.yaml. 

=== "method.R"
    ```{ .R .annotate hl_lines="6 7 8 9 10 11 12 13 14 15 16 17 36 37"}
    # Load package
    library(optparse)

    # Get list with command line arguments by name
    option_list = list(
        make_option(c("--input_file_name1"), # (1)!
            type="character", default=NULL, help=" "), 
        make_option(c("--input_file_name2"),
            type="character", default=NULL, help=" "), 
        make_option(c("--method_result1"),
            type="character", default=NULL, help=" "), 
        make_option(c("--method_result2"),
            type="character", default=NULL, help=" "), 
        make_option(c("--parameter1"),
            type="character", default=NULL, help=" "), 
        make_option(c("--parameter2"),
            type="character", default=NULL, help=" "), 
    ); 
    
    opt_parser = OptionParser(option_list=option_list);
    opt = parse_args(opt_parser);

    # Call the argument
    input_file_name1 <- opt$input_file_name1
    input_file_name2 <- opt$input_file_name2
    method_result1 <- opt$method_result1
    method_result2 <- opt$method_result2
    parameter1 <- opt$parameter1
    parameter2 <- opt$parameter2

    # Run the method to be benchmarked and create 2 outputs
    # output1 <- ...
    # output2 <- ...

    # Save as csv using parsed argument
    write.csv(output1, file = method_result1) # (2)!
    write.csv(output2, file = method_result2) 
    
    ```

    1.  Argument names that correspond to the  `inputs`, `outputs` and `parameter` fields specified in `config.yaml`.

    2. No need to specify the file extension here. For outputs, it will automatically be parsed based on the 'end:' that you specified in your config.yaml. 



## 3. The run_workflow.py file

This file is to run and update the method workflow. This is a standard script that will adapt to your `config.yaml` and dataset script and **does not need to be (and should not be!) modified**. Below is a copy of the `run_workflow.py` that you have in your project, with some explanations: 


=== "run_workflow.py"
    ```{ .python }
    from omnibenchmark.utils.build_omni_object import get_omni_object_from_yaml
    from omnibenchmark.renku_commands.general import renku_save
    import omniValidator as ov
    renku_save()

    ## Load config
    omni_obj = get_omni_object_from_yaml('src/config.yaml')  # (1)!

    ## Update object and download input datasets
    omni_obj.update_object(all=True) # (2)! 
    renku_save()

    ## Check object
    print(
        f"Object attributes: \n {omni_obj.__dict__}\n"
    )
    print(
        f"File mapping: \n {omni_obj.outputs.file_mapping}\n"
    )
    print(
        f"Command line: \n {omni_obj.command.command_line}\n" # (3)!
    )

    ## Create output dataset
    omni_obj.create_dataset() # (4)!

    ## Run workflow
    ov.validate_requirements(omni_obj)
    omni_obj.run_renku(all=False) # (5)!
    renku_save()

    ## Update Output dataset
    omni_obj.update_result_dataset() # (6)!
    renku_save()
    ```

    1. Load the options of your dataset into an Omnibenchmark object. 

    2. Downloads the datasets specified in `inputs: `. It case there are too many data to download, `all` can be set to False to only download a subset of the input data. 

    3. This will show the command line and the parsed arguments that are going to be called to run the method. Can also be used for debugging. 

    4. Pay attention to any warning messages about names conflicts (it can lead to unpredictable conflicts latter in the benchmark). If it happens, change the 'name' in your config.yaml and rerun all previous command lines.

    5. This will run your script with parsed arguments. 

    6. This will upload the methods results to make it recognizable by further modules. 

Once your module is completed and tested, [you can submit it to the team](04_submit.md). 