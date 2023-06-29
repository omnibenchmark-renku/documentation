# OmniOutput

Class to manage outputs of an omnibenchmark module.
This class has the following attributes:

* **`name (str)`**: Name that is specific for all outputs. Typically the module name/OmniObject name.
* **`out_names (List[str])`**: Names of the output file types
* **`output_end (Optional[Mapping[str, str]], optional)`**: Endings of the output filetypes.
* **`out_template (str, optional)`**: Template to generate output file names.
* **`file_mapping (Optional[List[OutMapping]], optional)`**: Mapping of input files, parameter values and output files.
* **`inputs (Optional[OmniInput], optional)`**: Object specifying all valid inputs.
* **`parameter (Optional[OmniParameter], optional)`**: Object speccifying the parameter space.
* **`default (Optional[Mapping], optional)`**: Default output files.
* **`filter_json(Optional[str], optional)`**: Path to json file with filter combinations.
* **`template_fun (Optional[Callable[..., Mapping]], optional)`**: Function to use to automatically generate output filenames.
* **`template_vars (Optional[Mapping], optional)`**: Variables that are used by template_fun.

The following class methods can be run on an instance of an OmniInput:

* **`update_outputs()`**: Method to update the output definitions according to the objects attributes.
