
# Build an OmniObject from yaml

All relevant information on how to run a specific module are stored as {ref}`OmniObject <section-omniobject>`.
The most convenient way to generate an instance of an `OmniObject` is to build it from a `config.yaml` file
using the `get_omni_object_from_yaml()` function:

``` python
## modules
from omnibenchmark.utils.build_omni_object import get_omni_object_from_yaml

## Load object
omni_obj = get_omni_object_from_yaml('src/config.yaml')

```
