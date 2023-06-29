
# OmniObject

Main class to manage an omnibenchmark module.
It takes the following arguments:

* **`name (str)`**: Module name
* **`keyword (Optional[List[str]], optional)`**: Keyword associated to the modules output dataset.
* **`title (Optional[str], optional)`**: Title of the modules output dataset.
* **`description (Optional[str], optional)`**: Description of the modules output dataset.
* **`script (Optional[PathLike], optional)`**: Script to generate the modules workflow for.
* **`command (Optional[OmniCommand], optional)`**: Workflow command - will be automatically generated if missing.
* **`inputs (Optional[OmniInput], optional)`**: Definitions of the workflow inputs.
* **`parameter (Optional[OmniParameter], optional)`**: Definitions of the workflow parameter.
* **`outputs (Optional[OmniOutput], optional)`**: Definitions of the workflow outputs.
* **`omni_plan (Optional[OmniPlan], optional)`**: The workflow description.
* **`benchmark_name (Optional[str], optional)`**: Name of the benchmark the module is associated to.
* **`orchestrator (Optional[str], optional)`**: Orchestrator url of the benchmark th emodule is associated to. Automatic detection.
* **`wflow_name (Optional[str], optional)`**: Workflow name. Will be set to the module name if none.
* **`dataset_name (Optional[str], optional)`**: Dataset name. Will be set to the module name if none.

The following class methods can be run on an instance of an OmniObject:

* **`create_dataset()`**: Method to create a renku dataset with the in the object specified attributes in the current renku project.
* **`update_object()`**: Method to check for new imports or updates in input and the parameter datasets. Will update object attributes accordingly.
* **`run_renku()`**: Method to generate and update the workflow and all output files as specified in the object.
* **`update_result_dataset()`**: Method to update and add all output datasets to the dataset specified in the object.
