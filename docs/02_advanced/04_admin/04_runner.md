## Install gitlab-runner in your (linux) machine

For CPU (not GPU) computing, machines to run **group** gitlab-runners **with docker executors** need docker and the gitlab-runner software installed. The machine (server, laptop) running the runners does not need a public IP.

### docker

Assuming your system is `apt`-based (debian, ubuntu):

```
sudo apt-get update

sudo apt install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt update

sudo apt-get install -y docker-ce docker-ce-cli containerd.io

systemctl status docker #; start if needed

groupadd docker
usermod -aG docker YOURUSER
```

### gitlab-runner

Mind the `arch`itecture of your machine; amd64 assumed here (Apple M1s: `arm64`)

```
mkdir -p ~/soft/gitlab-runner; cd $_

arch='amd64'
curl -LJO "https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_${arch}.deb"
sudo dpkg -i gitlab-runner_amd64.deb

sudo gitlab-runner start

# create an user too
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

## Register a gitlab runner in your (linux) machine 

On GitLab Community Edition 14.10.5 (might sligthly vary within versions) visit your repository **or group of repositories**, i.e. https://gitlab.renkulab.io/omb_benchmarks, go to `CI/CD` -> `Runners` (caution, not to `Settings -> CI/CD`, click `Register a group runner`, copy to the clipboard the registration token.

Let's assume the token is `94daGGiXCgwthisisnotarealtoken`.

In your gitlab-runner machine, run:

```
REGISTRATION_TOKEN="94daGGiXCgwthisisnotarealtoken"

RUNNER_NAME=examplerunner
EXECUTOR="docker"

sudo gitlab-runner register \
  --non-interactive \
  --url "https://gitlab.renkulab.io" \
  --registration-token "$REGISTRATION_TOKEN" \
  --description "$RUNNER_NAME" \
  -locked=false  \
  --run-untagged=true \
  --executor "$EXECUTOR" \
  --description "$RUNNER_NAME" \
  --docker-image docker:stable \
  --docker-network-mode "host" \
  --docker-volumes /var/run/docker.sock:/var/run/docker.sock

```

To check whether the gitlab runner is running,

```
sudo gitlab-runner list
sudo journalctl -u gitlab-runner
```

## Tips and troubleshooting

To inspect or edit/finetune the concurrency, timeout behaviour, and/or each runners details can be checked at `/etc/gitlab-runner/config.toml`. An example config file is:

```
concurrent = 10
check_interval = 0

[session_server]
  session_timeout = 36000

[[runners]]
  name = "tesuto-robinsonlab-gitlab-docker"
  url = "https://gitlab.renkulab.io"
  token = "YfztssssssssssssssN"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "docker:stable"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
    network_mode = "host"
    shm_size = 0

[[runners]]
  name = "iris-tesuto-robinsonlab-gitlab-docker"
  url = "https://gitlab.renkulab.io"
  token = "xbxxxxxxxxxxxxxKi"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "docker:stable"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
    network_mode = "host"
    shm_size = 0

```

Double check `privileged = false`, needs to be false. Re: the OOM killer, TLS verify etc: up to you.

`concurrent` limits how many **jobs** can run concurrently, across all registered runners; beware gitlab-runner does not know how many cores does each job use. For mostly single-core jobs defining `concurrent` as your machine's `nproc` - 2 should work.

Very useful [advanced config.toml docs](https://docs.gitlab.com/runner/configuration/advanced-configuration.html).

Mind that your gitlab repositories list their registered runners at `Settings -> CI/CD -> Runners`. Beware of the `shared runners`, `group runners` (the ones we're configuring here) and `other available runners`; disable the ones you don't want to use. 

Shell executors are also easy to config. Docker-in-docker is not working well due to subpar caching.

Remember to remove old images and vacuum/clean your machine (with a cron job). A recipe to prune old images, containers, networks and volumes (to be cron-ed at midnight):

```
#!/bin/bash
##
## Prunes images, containers, networks, volumes (ideally daily, at midnight)
## Does some convoluted checks to keep the most up-to-date image
## largely untested
##
## 18 Aug 2022
## Izaskun Mallona
## GPLv3

# prune images older than 24h
## the -a means it removes dangling but also those not used by existing containers
# the until-24h means removes those older than 24h

docker network prune  --filter "until=200h" -f

docker volume prune  -f "until=200h"

for diru in $(docker images -a --format "{{.Repository}}" | sort | uniq)
do
    ## sort by the timestamp, list all except the first/most recent
    for old_thing in $(docker images -a --format \
        "{{.ID}}\t{{.Size}}\t{{.Repository}}\t{{.CreatedAt}}" | \
        grep $diru | \
        sort -k4r | \
        tail -n+2 | \
        cut -f1)
    do
        echo Removing $old_thing from $diru
        docker rmi $old_thing;
    done
done

#remove dangling
docker image prune -f
```
