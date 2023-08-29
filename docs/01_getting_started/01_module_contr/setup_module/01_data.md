# Data modules

Data modules are modules that define input datasets and bundle them into a [renku dataset](https://renku.readthedocs.io/en/latest/topic-guides/data.html#), that can be imported by other projects. This page goes into details of some important elements to configure data. Most modules contain 3 main files:

## 1. The config.yaml file

The `config.yaml` (in your `src` directory) defines the metadata associated to your dataset and how it will be run. Upon creation of a dataset project, a `src/config.yaml` is already created and some metadata already pre-filled. Let's have a look how you can configure it; 

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

- `data: name:` (**mandatory**) Specifies the dataset name. Do not use special characters outside `-` and `_`. 

!!! warning
    Make the name as explicit as possible. If it (even partially) overlaps with other projects names, it might create some conflicts. For example, prefer `iris_dataset_clustering` instead of `iris`. 

- `data: keywords:` (**mandatory**) A keyword that is used for all datasets of this omnibenchmark. By default/ convention: `[omnibenchmark name]_data`. If you are not sure, you can check the keywords already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). 

- `script:` (**mandatory**) Relative path to the script to execute. More information about the script in the `The module script` section below. 

- `outputs:files:` (**Mandatory**) Name that will be passed to the output of the scripts (e.g. `data_file1`) and their file extension (`end:` field). It must match the arguments of you script (see `The module script` section below).  

- `benchmark_name:` (**Mandatory**) The name of the omnibenchmark this dataset belongs to. If you are not sure, you can check the names already in use in [existing omnibenchmarks](http://omnibenchmark.org/p/benchmarks/). 

- `data: title:` (Optional) A human readable title for this dataset. Can be the same as `data: name:`. 

- `data: description:` (Optional) Some description about this dataset. 


More information about the `config.yaml` file in the [Setup a config.yaml](../../../03_howto/00_config_yaml.md) section.


## 2. The module script

This is the script to load the dataset and to convert its files into the expected format.
Omnibenchmark accepts any kind of script and its maintenance and content is up to the module author.
By default, an R and Python script are provided in the `src` folder that you can use to import or create your dataset.
Omnibenchmark calls the script (specified in the `script:` field of `config.yaml`) from the command line. If you use another language than R or Python (e.g. Julia or bash) specify the interpreter to use in the corresponding field of the `config.yaml` file (`script:` field of `config.yaml`).

!!!note
    All input and output files will automatically be parsed from the command line in the format:
    `--ARGUMENT_NAME ARGUMENT_VALUE`

You can find below a dummy example of how a `config.yaml` and its corresponding script can look like (in Python and R); 


=== "config.yaml"
    ```{ .yaml .annotate hl_lines="7 11 12"}
    ---
    data:
        name: "some-dataset"
        title: "some dataset"
        description: " "
        keywords: ["omnibenchmark-X_data"] 
    script: "src/dataset.R" # (1)!
    outputs:
        template: "data/${name}/${name}_${out_name}.${out_end}"
        files:
            dataset_output: # (2)!
                end: "csv" # (3)!
    benchmark_name: "omnibenchmark-X" 

    
    ```

    1. Can be replaced by another script or extension. 

    2. The argument name that will be parsed when running the script. Has to match the argument name of your script! (showed in the next script)

    3. Specifies the file extension (attached to the argument name). 


The corresponding script that creates a `dataset_output.csv` (following our example `config.yaml`) can thus look like this; 

=== "dataset.py" 
    ```{ .python .annotate hl_lines="6 16"}
    # Load module
    import argparse

    # Get command line arguments and store them in args
    parser=argparse.ArgumentParser()
    parser.add_argument('--dataset_output', help='name') # (1)!
    args=parser.parse_args()

    # Call the argument
    arg1 = args.argument_name 

    # Import or create a dataset
    # df=...

    # Save as csv using parsed argument
    df.to_csv(arg1, index=False) # (2)!
    
    ```

    1.  Argument name should correspond to the one in config, here 'dataset_output'. 

    2. No need to specify the file extension here. For outputs, it will automatically be parsed based on the 'end:' that you specified in your config.yaml. 

=== "dataset.R"
    ```{ .R .annotate hl_lines="6 20"}
    # Load package
    library(optparse)

    # Get list with command line arguments by name
    option_list = list(
        make_option(c("--dataset_output"), # (1)!
                type="character", default=NULL, 
                help=" ", metavar="character")
    ); 
    
    opt_parser = OptionParser(option_list=option_list);
    opt = parse_args(opt_parser);

    # Call the argument
    arg1 <- opt$argument_name

    # Import of create a dataset 
    # df <- ...

    write.csv(df, file = arg1) # (2)!
    
    ```

    1.  Argument name should correspond to the one in config, here 'dataset_output'. 

    2. No need to specify the file extension here. For outputs, it will automatically be parsed based on the 'end:' that you specified in your config.yaml. 

For multiple outputs, simply add more fields to the `outputs: files:` of your `config.yaml` and the same names to argument parser of your script. 


??? note "Special case: input data as files"

    In some cases the data have to be **uploaded manually** to the project instead of being fetched/ downloaded by a script. (Please note however that downloading the data from a script is more reproducible and allows to keep the data, and thus your benchmark, always up-to-date!)
    
    For example, say that we have uploaded 2 data files in an `input` folder. The corresponding `config.yaml` and R/Python script would be; 

    === "config.yaml"
        ```{ .yaml .annotate hl_lines="8 9 10 11 12"}
        ---
        data:
            name: "some-dataset"
            title: "some dataset"
            description: " "
            keywords: ["omnibenchmark-X_data"] 
        script: "src/dataset.R" 
        inputs: # (1)!
            input_files: 
                import_this_dataset:
                    input_data: "input/input_dataset.txt" # (2)!
                    input_metadata: "input/input_metadata.txt"
        outputs:
            template: "data/${name}/${name}_${out_name}.${out_end}"
            files:
                dataset_output:
                    end: "csv" 
        benchmark_name: "omnibenchmark-X" 

        
        ```

        1. Section to add to your config.yaml

        2. Two new arguments, 'input_data' and 'input_metadata' will now be sent to your script, containing the path to your input files. 

        And the corresponding script; 

        === "dataset.py" 
            ```{ .python .annotate hl_lines="7 8"}
            # Load module
            import argparse

            # Get command line arguments and store them in args
            parser=argparse.ArgumentParser()
            parser.add_argument('--dataset_output', help='name')
            parser.add_argument('--input_data', help='name') # (1)!
            parser.add_argument('--input_metadata', help='name') 
            args=parser.parse_args()

            # Call the argument
            arg1 = args.argument_name 

            # Import or create a dataset
            # df=...

            # Save as csv using parsed argument
            df.to_csv(arg1, index=False) 
            
            ```

            1.  Argument names that correspond to the 2 'input:' fields of the config.yaml. 

        === "dataset.R"
            ```{ .R .annotate hl_lines="9 10 11 12 13 14"}
            # Load package
            library(optparse)

            # Get list with command line arguments by name
            option_list = list(
                make_option(c("--dataset_output"),
                        type="character", default=NULL, 
                        help=" ", metavar="character"),
                make_option(c("--input_data"), # (1)!
                        type="character", default=NULL, 
                        help=" ", metavar="character"), 
                make_option(c("--input_metadata"),
                        type="character", default=NULL, 
                        help=" ", metavar="character")
            ); 
            
            opt_parser = OptionParser(option_list=option_list);
            opt = parse_args(opt_parser);

            # Call the argument
            arg1 <- opt$argument_name

            # Import of create a dataset 
            # df <- ...

            write.csv(df, file = arg1) 
            
            ```

            1.  Argument name should correspond to the one in config, here 'dataset_output'. 

        

## 3. The run_workflow.py file

This file is to generate, run and update the modules dataset and workflow. This is a standard script that will adapt to your `config.yaml` and dataset script and **does not need to be (and should not be!) modified**. Below is a copy of the `run_workflow.py` that you have in your project, with some explanations: 

=== "run_workflow.py"
    ```{ .python }

    # Load modules
    from omnibenchmark.utils.build_omni_object import get_omni_object_from_yaml
    from omnibenchmark.renku_commands.general import renku_save

    # Build an OmniObject from the config.yaml file
    omni_obj = get_omni_object_from_yaml('src/config.yaml') # (1)!

    # Create the output dataset
    omni_obj.create_dataset() # (2)!
    renku_save()

    ## Run and update the workflow
    omni_obj.run_renku()  # (3)!
    renku_save()

    ## Add files to output dataset
    omni_obj.update_result_dataset() # (4)!
    renku_save()

    ```

    1. Load the options of your dataset into an Omnibenchmark object. 

    2. Pay attention to any warning messages about names conflicts (it can lead to unpredictable conflicts latter in the benchmark). If it happens, change the 'name' in your config.yaml and rerun all previous command lines. 

    3. This will run your script with parsed arguments. 

    4. This will upload the created dataset to make it recognizable by further modules. 

Once your module is completed and tested, [you can submit it to the team](04_submit.md). 