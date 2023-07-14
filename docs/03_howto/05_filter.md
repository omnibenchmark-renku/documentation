
# Filter (out) Inputs and Parameter

Some methods are not applicable for all input datasets or the entire parameter range. Maybe the parameter range to use depends on features of the input dataset. `omnibenchmark` allows filtering (filter out) at different level to flexibly specify input data bundles to exclude, parameter limits and input-parameter combinations to ignore. See below for different types of filter with examples.

!!! Note
    In general `omnibenchmark` allows detailed specification of the accepted inputs and parameter to __include__, e.g., through *keywords* or *explicit definition*. Especially keyword-based specification is limited by it's extend to generalize for new (not yet included inputs/parameter). Filtering inputs and outputs to __exclude__ allows automatic and programmatic specification to extend existing generalizations.     



## Filter explicit input data bundles

While input data bundles with a certain *keyword* should typically fullfill all requirements of downstream modules, there can be module-specifc requirements, which are violated by single upstream data bundles. A possible solution is to explicitly exclude certain data bundles by name as inputs. These data bundles will automatically be **ignored** upon data bundle import and input - output file mapping. Input filtering can be specified within the `config.yaml` or by using attributes of the __OmniObject__ in `run_workflow.py`:

=== "config.yaml"
    ```{ .yaml .annotate hl_lines="12"}
    ---
    data:
	    ...
    script: "src/run_method_XY.R"
    benchmark_name: "omniexample"
    inputs:
        keywords: ["dataset_omniexample"]
        files: ["counts", "meta_file"]
        prefix:
            counts: "_counts"
            meta_file: "_meta"
        filter_names: ["dataset1", "dataset2"] # (1)!
    outputs:
    	...
    ```
    
    1.  Names must correspond to data bundle names (also called *slug* in renku terminology). A single name can be provided as string and multiple names as list of strings.

=== "run_workflow.py"
    ```{ .python .annotate hl_lines="7"}
    import omnibenchmark as omni

    # Generate object from yaml file
    omni_obj = omni.utils.get_omni_object_from_yaml("config.yaml")

    # Explicitly ignore input data bundles by name
    omni_obj.inputs.filter_names = ["dataset1", 'dataset2']
    
    # Update object
    omni_obj.update_object() # (1)!
    renku_save()
    ```

    1.  Add filter attributes __before__ updating the object to prevent data bundles you want to ignore from being imported. 

!!! Note
    Consider using different input data bundle __keywords__, if the number of data bundles to explicitly exclude is large or multiple downstream modules have the same restrictions.


## Filter parameter

Parameter ranges can be explicitly specified within the `config.yaml` file. If the global parameter space (i.e., a data bundle with the benchmark-specific parameter space) is used, specific limits or values to exclude can be specified within the `config.yaml` file or as attributes of the __OmniObject__ in `run_workflow.py`:

### Parameter limits

Limits can only be used to specify ranges of parameter with __numeric__ values. Limits for each parameter are specified separately. 
The keys `upper` and `lower` are fixed terms.

=== "config.yaml"
    ```{ .yaml .annotate hl_lines="13 14 15 16"}
    ---
    data:
	    ...
    script: "src/run_method_XY.R"
    benchmark_name: "omniexample"
    inputs:
        ...
    outputs:
        ...
    parameter:
        names: ["dims", "mode"]
        keywords: ["parameter_omniexample"]
        filter: # (1)!
        	dims:
          		upper: 100
          		lower: 10
    ```
    
    1.  Limits can only be specified for parameter with numeric values. The keys `upper` and `lower` are fixed and required.

=== "run_workflow.py"
    ```{ .python .annotate hl_lines="7 8 9 10 11 12"}
    import omnibenchmark as omni

    # Generate object from yaml file
    omni_obj = omni.utils.get_omni_object_from_yaml("config.yaml")

    # Set parameter limits # (1)!
    omni_obj.parameter.filter = {
        "dims": {
            "lower": 10, 
            "upper": 100
            }
        }
    
    # Update object
    omni_obj.update_object() # (2)!
    renku_save()
    ```

    1.  Limits can only be specified for parameter with numeric values. The keys `upper` and `lower` are fixed and required. 
    2.  Add filter attributes __before__ updating the object to apply filter to the generated outputs. 


### Parameter values to exclude

Specific values can be ignored/excluded for all inputs. Parameter values to exclude are specified explicitly for each parameter. 
The key `exclude` is a fixed term.

=== "config.yaml"
    ```{ .yaml .annotate hl_lines="13 14 15"}
    ---
    data:
	    ...
    script: "src/run_method_XY.R"
    benchmark_name: "omniexample"
    inputs:
        ...
    outputs:
        ...
    parameter:
        names: ["dims", "mode"]
        keywords: ["parameter_omniexample"]
        filter: # (1)!
        	mode:
          		exclude: ["auto", "solo"]
    ```
    
    1.  Values to exclude can be specified for parameter with numeric and character values. The key `exclude` is fixed and required.

=== "run_workflow.py"
    ```{ .python .annotate hl_lines="7 8 9 10 11"}
    import omnibenchmark as omni

    # Generate object from yaml file
    omni_obj = omni.utils.get_omni_object_from_yaml("config.yaml")

    # Set parameter to exclude # (1)!
    omni_obj.parameter.filter = {
        "mode": {
            "exclude": ["auto", "mode"]
            }
        }
    
    # Update object
    omni_obj.update_object() # (2)!
    renku_save()
    ```

    1.  Values to exclude can be specified for parameter with numeric and character values. The key `exclude` is fixed and required. 
    2.  Add filter attributes __before__ updating the object to apply filter to the generated outputs. 


## Filter explicit input-parameter combinations 

Explicit input parameter combinations can be filtered by providing a `.json` file explicitly listing the input - parameter combinations (mappings) to be ignored and not used for output generation. 

```{ .json title="filter_combinations.json"}
[
   {
      "input_files": {
         "in_file1": "data/dataset1/dataset1_in_file1.mtx.gz",
         "in_file2": "data/dataset1/dataset1_in_file2.json",
      },
      "parameter": {
         "dims": 10,
         "mode": "auto"
      }
   }
]
```

The `.json` file with explicit mappings to ignore can be specified in the `config.yaml` file or as attributes of the __OmniObject__ in `run_workflow.py`:


=== "config.yaml"
    ```{ .yaml .annotate hl_lines="12"}
    ---
    data:
	    ...
    script: "src/run_method_XY.R"
    benchmark_name: "omniexample"
    inputs:
        ...
    outputs:
        files:
    	    method_res: 
        	    end: ".json"
        filter_json: "path/to/filter_combinations.json"
    parameter:
        ...
    ```

=== "run_workflow.py"
    ```{ .python .annotate hl_lines="7"}
    import omnibenchmark as omni

    # Generate object from yaml file
    omni_obj = omni.utils.get_omni_object_from_yaml("config.yaml")

    # Path to filter combinations.json 
    omni_obj.outputs.filter_json = "src/filter_comb.json"
    
    # Update object
    omni_obj.update_object() # (1)!
    renku_save()
    ```

    1.  Add filter attributes __before__ updating the object to apply filter to the generated outputs. 



## Programmatic filter 

If possible any filter should be specified programmatically to allow generalization for new (not yet known) inputs or parameter space extension.
We can use the above-mentioned `.json` file from [explicitly filtering input-parameter combinations](#filter-explicit-input-parameter-combinations) to automatically filter input data bundles, parameter values or input-parameter combinations.

!!!Note
    Programmatic filtering can not be done on the `config.yaml` level, but needs to be implemented within `run_workflow.py`.

1.  Define a filter function describing your filter in a general way.

    ???example

        The following example uses the `omni_obj` as input argument, gets the true dimensionality of the data from the input file with the key `meta` and selects values from the `dims` parameter that differ more than 3 from the true dimensionality for filtering.

        ```{ .python .annotate title="filter_function.py"}
        import json
        import pandas as pd

        def get_param_filter_by_ground_truth(omni_obj):
            filter_list = []

            # Loop through all input file groups
            for infile_mapping in omni_obj.inputs.input_files.values():

                # Load meta data from object
                with open(infile_mapping["meta_file"]) as f:  
                        meta = json.load(f)

                # Get true dim
                df = pd.DataFrame.from_dict(meta)
                k = meta.get("true_dim")

                # Get parameter combinations to filter
                param_comb = [
                    {param_nam: param_val for param_nam, param_val in param_map.items()}
                    for param_map in omni_obj.parameter.combinations
                    if param_map["k"] < k - 3 or param_map["k"] > k + 3
                ]

                # Generate filter items
                filter_list_dat = [
                    {"input_files": infile_mapping, "parameter": param_com}
                    for param_com in param_comb
                ]

                # Add to filter_list
                filter_list = filter_list + filter_list_dat

            return filter_list
        ```

2.  Store filtered combinations as `.json` and assign this as output filter `.json` file

    ```{ .python .annotate title="run_workflow.py" hl_lines="14 15 16 17"}
    from omnibenchmark.utils.build_omni_object import get_omni_object_from_yaml
    from omnibenchmark.renku_commands.general import renku_save
    import json
    import filter_function # (1)!

    ## Load config
    omni_obj = get_omni_object_from_yaml('src/config.yaml')

    ## Update object and download input datasets
    omni_obj.update_object()
    renku_save()

    ## Generate json with filter combinations
    filter_comb = filter_function.get_param_filter_by_ground_truth(omni_obj) # (2)!
    with open("filter_comb.json", "w") as fp:
        json.dump(filter_comb, fp, indent=3)
    renku_save()
    ```

    1.  Import filter function from __Step 1__
    2.  See example above (adapt function name and input) 

3.  Update outputs and command to apply filter

    ```{ .python .annotate title="run_workflow.py" hl_lines="2 3 4 5"}
    
    ## update outputs and commands
    omni_obj.outputs.filter_json = "src/filter_comb.json" # (1)!
    omni_obj.outputs.update_outputs()
    omni_obj.command.outputs = omni_obj.outputs
    omni_obj.command.update_command()

    ## Run workflow
    omni_obj.run_renku()
    renku_save()
    ```

    1.  Updating outputs and command __only__ (instead of the entire object) to prevent input import/update.