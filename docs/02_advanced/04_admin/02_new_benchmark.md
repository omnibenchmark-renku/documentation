This guide relies on the `default` omnibenchmark components (gitlab runners, triplestore etc).

GUI click/fill instructions have been tested in [https://gitlab.renkulab.io](https://gitlab.renkulab.io) running GitLab Community Edition 14.10.5 and might change in future releases.

## Foreground

The starting point for a non automated benchmark creation is [renkulab.io](https://renkulab.io).

Interestingly, renkulab.io's gitlab is available at [gitlab.renkulab.io](https://gitlab.renkulab.io). If you log in to renkulab.io you'll be logged in to the gitlab too. To switch easily from renku's GUI to gitlab's GUI please notice the `projects` or `gitlab` components of these URLs: 

- [https://gitlab.renkulab.io/omnibenchmark/iris_example/iris-dataset](https://gitlab.renkulab.io/omnibenchmark/iris_example/iris-dataset) 

vs

- [https://renkulab.io/projects/omnibenchmark/iris_example/iris-dataset](https://renkulab.io/projects/omnibenchmark/iris_example/iris-dataset)

Both refer to the same repository.

### Gitlab group (and subgroups)

Repository groups can be created by browsing https://gitlab.renkulab.io pressing `new group`. Repository subgroups can be created by browsing a group, i.e.  https://gitlab.renkulab.io/omb_benchmarks , and pressing `new subgroup`. . If interested in creating a subgroup below 'known' omnibenchmark groups such as https://gitlab.renkulab.io/omb_benchmarks  or https://gitlab.renkulab.io/omnibenchmark your user needs to be granted rights; contact the omnibecnhmark team if so.

**Tip**: you can add other people to your benchmark group/subgroup by pressing (left panel) `(sub)group information -> Members`.

**Tip**: it's advisable to register a dedicated gitlab runner when generating a group, and use it as a group runner for CI/CD. For that, check [our runner's docs](04_runner.md).

### Benchmark masked variables/tokens

Once you've created the group or subgroup where your benchmark will leave (or at least your orchestrator will), you'll need to create a token with `api_read` scope. For that, visit your **group** `Settings -> CI/CD -> Variables` setting and create a nonprotected, masked variable, for instance `OMB_ACCESS_TOKEN~. And keep its content stored somewhere for future usage. 

### User tokens

A personal gitlab token will allow to automate actions. To generate one:
- log in at [https://gitlab.renkulab.io](https://gitlab.renkulab.io)
- visit [https://gitlab.renkulab.io/-/profile/personal_access_tokens](https://gitlab.renkulab.io/-/profile/personal_access_tokens)
- create one with the adequate scope (i.e. read API)

This token won't be used by omnibenchmark, but can be handy to have.

### Orchestrator

The core component of omnibenchmark is an orchestrator, which stitches together datasets, methods and metrics. Without an orchestrator there is no benchmark. Still, you can set up the orchestrator last.

Orchestrators are unusual omnibenchmark components. They're mainly a gitlab CI/CD yaml. An [example orchestrator](https://gitlab.renkulab.io/omnibenchmark/iris_example/iris-orchestrator/-/blob/master/.gitlab-ci.yml) looks like this:

```
variables:
  GIT_STRATEGY: fetch
  GIT_SSL_NO_VERIFY: "true"
  GIT_LFS_SKIP_SMUDGE: 1
  OMNIBENCHMARK_NAME: iris_example
  TRIPLESTORE_URL: http://imlspenticton.uzh.ch/omni_iris_data
  CI_UPSTREAM_PROJECT: ${CI_PROJECT_PATH}
  OMNI_UPDATE_TOKEN: ${OMNI_UPDATE_TOKEN}
  CI_PUSH_TOKEN: ${CI_PUSH_TOKEN}


stages:
  - build
  - data_run
  - process_run
  - parameter_run
  - method_run
  - metric_run
  - summary_metric_run

image_build:
  stage: build
  image: docker:stable
  rules: 
    - if: '$CI_PIPELINE_SOURCE == "pipeline"'
      when: never
  before_script:
    - docker login -u gitlab-ci-token -p $CI_JOB_TOKEN http://$CI_REGISTRY
  script: |
    CI_COMMIT_SHA_7=$(echo $CI_COMMIT_SHA | cut -c1-7)
    docker build --tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA_7 .
    docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA_7


trigger_iris_dataset:
  stage: data_run 
  only:
    - schedules
  trigger: 
    project: omnibenchmark/iris_example/iris-dataset
    strategy: depend

```

The first stanza (`variables`) defines variables needed for overall git behaviour as well as helping with authentication. The important tokens are:

- OMNI_UPDATE_TOKEN
- CI_PUSH_TOKEN

The second stanza (`image_build`) generates a renku-powered docker image (i.e. for interactive sessions).

The third stanza (`trigger_iris_dataset`) is how most of the orchestrator CI/CD tasks look like: they trigger downstream projects CI/CDs; that is, their `gitlab-ci.yaml`. In the example above, triggers [the iris dataset CI/CD](https://gitlab.renkulab.io/omnibenchmark/iris_example/iris-dataset/-/blob/master/.gitlab-ci.yml).

### Terraforming via templates

[Omnibenchmark's renku templates](https://github.com/omnibenchmark/contributed-project-templates) help to create new benchmark components.

### Terraforming via gitlab API

[omnibus](https://github.com/omnibenchmark/omnibus) helps automating benchmark creation.

### Tips/resources

- [Predefined CI/CD variables](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html)


## Background

Infrastructure-wise, some manual actions need to be done to start a new benchmark.

- register runner(s)
- set up bettr endpoint
- add a triplestore dataset
- add a triplestore apache reverse proxy

### Runners

It's advisable to register a dedicated gitlab runner when generating a benchmark, and use it as a group runner for CI/CD. For that, check [our runner's docs](04_runner).
