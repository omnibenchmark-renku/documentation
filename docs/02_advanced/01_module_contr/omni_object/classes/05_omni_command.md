# OmniCommand

Class to manage the main workflow command of an omnibenchmark module.
This class has the following attributes:

* **`script (Union[PathLike, str])`**: Path to the script run by the command
* **`interpreter (str, optional)`**: Interpreter to run the script with.
* **`command_line (str, optional)`**: Command line to be run.
* **`outputs (OmniOutput, optional)`**: Object specifying all outputs.
* **`input_val (Optional[Mapping], optional)`**: Input file tyoes and paths to run the command on.
* **`parameter_val (Optional[Mapping], optional)`**: Parameter names and values to run the command with.

The following class methods can be run on an instance of an OmniInput:

* **`update_command()`**: Method to update the command according to the outputs,inputs,parameter.
