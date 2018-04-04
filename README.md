[![CircleCI](https://circleci.com/gh/bitnami/bitnami-docker-blackbox_exporter/tree/master.svg?style=shield)](https://circleci.com/gh/bitnami/bitnami-docker-blackbox_exporter/tree/master)

# What is Blackbox_exporter?

The blackbox exporter allows blackbox probing of endpoints over HTTP, HTTPS, DNS, TCP and ICMP.

[https://github.com/prometheus/blackbox_exporter](https://github.com/prometheus/blackbox_exporter)

# TL;DR;

```bash
$ docker run --name blackbox_exporter bitnami/blackbox_exporter:latest
```

# Why use Bitnami Images?

* Bitnami closely tracks upstream source changes and promptly publishes new versions of this image using our automated systems.
* With Bitnami images the latest bug fixes and features are available as soon as possible.
* Bitnami containers, virtual machines and cloud images use the same components and configuration approach - making it easy to switch between formats based on your project needs.
* Bitnami images are built on CircleCI and automatically pushed to the Docker Hub.
* All our images are based on [minideb](https://github.com/bitnami/minideb) a minimalist Debian based container image which gives you a small base container image and the familiarity of a leading linux distribution.

# Get this image

The recommended way to get the Bitnami Blackbox_exporter Docker Image is to pull the prebuilt image from the [Docker Hub Registry](https://hub.docker.com/r/bitnami/blackbox_exporter).

```bash
$ docker pull bitnami/blackbox_exporter:latest
```

To use a specific version, you can pull a versioned tag. You can view the [list of available versions](https://hub.docker.com/r/bitnami/blackbox_exporter/tags/) in the Docker Hub Registry.

```bash
$ docker pull bitnami/blackbox_exporter:[TAG]
```

If you wish, you can also build the image yourself.

```bash
$ docker build -t bitnami/blackbox_exporter:latest https://github.com/bitnami/bitnami-docker-blackbox_exporter.git
```

# Connecting to other containers

Using [Docker container networking](https://docs.docker.com/engine/userguide/networking/), a different server running inside a container can easily be accessed by your application containers and vice-versa.

Containers attached to the same network can communicate with each other using the container name as the hostname.

## Using the Command Line

### Step 1: Create a network

```bash
$ docker network create blackbox_exporter-network --driver bridge
```

### Step 2: Launch the Blacbox_exporter container within your network

Use the `--network <NETWORK>` argument to the `docker run` command to attach the container to the `blackbox_exporter-network` network.

```bash
$ docker run --name blackbox_exporter-node1 --network blackbox_exporter-network bitnami/blackbox_exporter:latest
```

### Step 3: Run another containers

We can launch another containers using the same flag (`--network NETWORK`) in the `docker run` command. If you also set a name to your container, you will be able to use it as hostname in your network.


# Configuration

Blackbox exporter is configured via a configuration file and command-line flags (such as what configuration file to load, what port to listen on, and the logging format and level).

The default location for the config file is `/opt/bitnami/blackbox_exporter/conf/config.yml`, you can mount a volume there in order to overwrite it.

The file is written in YAML format, defined by the scheme described below. Brackets indicate that a parameter is optional. For non-list parameters the value is set to the specified default.

Generic placeholders are defined as follows:

``<boolean>``: a boolean that can take the values true or false
`<int>`: a regular integer
`<duration>`: a duration matching the regular expression [0-9]+(ms|[smhdwy])
`<filename>`: a valid path in the current working directory
`<string>`: a regular string
`<secret>`: a regular string that is a secret, such as a password
`<regex>`: a regular expression
The other placeholders are specified separately.

Example config:

```yaml
scrape_configs:
  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]  # Look for a HTTP 200 response.
    static_configs:
      - targets:
        - http://prometheus.io    # Target to probe with http.
        - https://prometheus.io   # Target to probe with https.
        - http://example.com:8080 # Target to probe with http on port 8080.
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115  # The blackbox exporter's real hostname:port.
```

[Further information](https://github.com/prometheus/blackbox_exporter/blob/master/CONFIGURATION.md)

### Mounting a volume

Check the [Persisting your data](#Persisting your application) section to add custom volumes to the Blackbox_exporter container

# Logging

The Bitnami blackbox_exporter Docker image sends the container logs to the `stdout`. To view the logs:

```bash
$ docker logs blackbox_exporter
```

You can configure the containers [logging driver](https://docs.docker.com/engine/admin/logging/overview/) using the `--log-driver` option if you wish to consume the container logs differently. In the default configuration docker uses the `json-file` driver.

# Maintenance

## Upgrade this image

Bitnami provides up-to-date versions of blackbox_exporter, including security patches, soon after they are made upstream. We recommend that you follow these steps to upgrade your container.

### Step 1: Get the updated image

```bash
$ docker pull bitnami/blackbox_exporter:latest
```

### Step 2: Stop and backup the currently running container

Stop the currently running container using the command

```bash
$ docker stop blackbox_exporter
```

Next, take a snapshot of the persistent volume `/path/to/blackbox_exporter-persistence` using:

```bash
$ rsync -a /path/to/blackbox_exporter-persistence /path/to/blackbox_exporter-persistence.bkp.$(date +%Y%m%d-%H.%M.%S)
```

You can use this snapshot to restore the database state should the upgrade fail.

### Step 3: Remove the currently running container

```bash
$ docker rm -v blackbox_exporter
```

### Step 4: Run the new image

Re-create your container from the new image, [restoring your backup](#restoring-a-backup) if necessary.

```bash
$ docker run --name blackbox_exporter bitnami/blackbox_exporter:latest
```

# Contributing

We'd love for you to contribute to this container. You can request new features by creating an [issue](https://github.com/bitnami/bitnami-docker-blackbox_exporter/issues), or submit a [pull request](https://github.com/bitnami/bitnami-docker-blackbox_exporter/pulls) with your contribution.

# Issues

If you encountered a problem running this container, you can file an [issue](https://github.com/bitnami/bitnami-docker-blackbox_exporter/issues). For us to provide better support, be sure to include the following information in your issue:

- Host OS and version
- Docker version (`docker version`)
- Output of `docker info`
- Version of this container
- The command you used to run the container, and any relevant output you saw (masking any sensitive information)

# License
Copyright 2018 Bitnami

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
