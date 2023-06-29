
# OmniInput

Class to manage inputs of an omnibenchmark module.
This class has the following attributes:

* **`names (List[str])`**: Names of the input filetypes
* **`prefix (Optional[Mapping[str, List[str]]], optional)`**: Prefixes (or substrings) of the input filetypes.
* **`input_files (Optional[Mapping[str, Mapping[str, str]]], optional)`**: Input files ordered by file types.
* **`keyword (Optional[List[str]], optional)`**: Keyword to define which datasets are imported as input datasets.
* **`default (Optional[str], optional)`**: Default input name (e.g., dataset).
* **`filter_names (Optional[List[str]], optional)`**: Input dataset names to be ignored.

The following class methods can be run on an instance of an OmniInput:

* **`update_inputs()`**: Method to import new and update existing input datasets and update the object accordingly
