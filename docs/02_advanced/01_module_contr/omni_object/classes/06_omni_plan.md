
# OmniPlan

Class to manage the workflow of an omnibenchmark module.
This class has the following attributes:

* **`plan (PlanViewModel)`**: A plan view model as defined in renku
* **`param_mapping (Optional[Mapping[str, str]], optional)`**: A mapping between the component names of the plan and the OmniObject.

The following class methods can be run on an instance of an OmniInput:

* **`predict_mapping_from_file_dict()`**: Method to predict the mapping from the (input-, output-, parameter) file mapping used to generate the command.
