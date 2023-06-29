
# OmniParameter

Class to manage parameter of an omnibenchmark module.
This class has the following attributes:

* **`names (List[str])`**: Name of all valid parameter
* **`values (Optional[Mapping[str, List]], optional)`**: Parameter values - usually automatically detected.
* **`default (Optional[Mapping[str, str]], optional)`**: Default parameter values.
* **`keyword (Optional[List[str]], optional)`**: Keyword to import the parameter dataset with.
* **`filter (Optional[Mapping[str, str]], optional)`**: Filter to use for the parameter space.
* **`combinations (Optional[List[Mapping[str, str]]], optional)`**: All possible parameter combinations.

The following class methods can be run on an instance of an OmniInput:

* **`update_parameter()`**: Method to import and update parameter datasets and update the object/parameter space accordingly.
