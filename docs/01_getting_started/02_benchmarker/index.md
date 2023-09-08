
## Benchmarker

Generating new benchmarks can be automated using:

- [Project templates](https://github.com/omnibenchmark/contributed-project-templates), a semi-manual approach;
- [omnibus](https://github.com/omnibenchmark/omnibus), a CLI-like experience using gitlab API calls to terraform new benchmarks.

### Setting up a benchmark with `omnibus`

After installing omnibus, docker and other requirements (such as defining a gitlab token so our user can create new projects via gitlab API), as described in [omnibus](https://github.com/omnibenchmark/omnibus), we need to define the benchmark structure. This is done via config files, and could be wrapped by using omnibus' wrapper:

```
source("R/create_config.R")

create_config(
	benchmark = "Your_benchmark",
	config = "config_Your_benchmark",
	datanames = c("name1", "name2", "name3"),
	methodnames = c("name1", "name2"),
	metricnames = c("name1", "name2"),
	explainer = TRUE, # Adds explanation of each config parameter to the buttom of the file
	... # Add any config parameter setting (does not work with BENCHMARK, REPONAMES,
	# KEYWORDS, or TEMPLATES). Names must match the names from the template.
	)
# If you don't specific the config name, it will be called "config_<benchmark>".

```

Using this file, the command

```
omnibus -o -p -s -c CONFIGFILE

```

triggers the benchmark terraforming.

To avoid storing the API token in clear text, you can pass it to `omnibus` with `--token TOKEN`.
