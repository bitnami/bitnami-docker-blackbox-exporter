[![CircleCI](https://circleci.com/gh/bitnami/bitnami-docker-blackbox-exporter/tree/master.svg?style=shield)](https://circleci.com/gh/bitnami/bitnami-docker-blackbox-exporter/tree/master)

# What is Blackbox Exporter?

The blackbox exporter allows blackbox probing of endpoints over HTTP, HTTPS, DNS, TCP and ICMP.

[https://github.com/prometheus/blackbox-exporter](https://github.com/prometheus/blackbox-exporter)

# TL;DR;

```bash
$ docker run --name blackbox-exporter bitnami/blackbox-exporter:latest
```

# Why use Bitnami Images?

* Bitnami closely tracks upstream source changes and promptly publishes new versions of this image using our automated systems.
* With Bitnami images the latest bug fixes and features are available as soon as possible.
* Bitnami containers, virtual machines and cloud images use the same components and configuration approach - making it easy to switch between formats based on your project needs.
* Bitnami images are built on CircleCI and automatically pushed to the Docker Hub.
* All our images are based on [minideb](https://github.com/bitnami/minideb) a minimalist Debian based container image which gives you a small base container image and the familiarity of a leading linux distribution.

# Supported tags and respective `Dockerfile` links

* [`0-ol-7`, `0.12.0-ol-7-r25` (0/ol-7/Dockerfile)](https://github.com/bitnami/bitnami-docker-blackbox-exporter/blob/0.12.0-ol-7-r25/0/ol-7/Dockerfile)
* [`0-debian-9`, `0.12.0-debian-9-r10`, `0`, `0.12.0`, `0.12.0-r10`, `latest` (0/Dockerfile)](https://github.com/bitnami/bitnami-docker-blackbox-exporter/blob/0.12.0-debian-9-r10/0/Dockerfile)
* [`0-debian-8`, `0.12.0-debian-8-r78` (0/debian-8/Dockerfile)](https://github.com/bitnami/bitnami-docker-blackbox-exporter/blob/0.12.0-debian-8-r78/0/debian-8/Dockerfile)

Subscribe to project updates by watching the [bitnami/blackbox-exporter GitHub repo](https://github.com/bitnami/bitnami-docker-blackbox-exporter).

# Get this image

The recommended way to get the Bitnami Blackbox Exporter Docker Image is to pull the prebuilt image from the [Docker Hub Registry](https://hub.docker.com/r/bitnami/blackbox-exporter).

```bash
$ docker pull bitnami/blackbox-exporter:latest
```

To use a specific version, you can pull a versioned tag. You can view the [list of available versions](https://hub.docker.com/r/bitnami/blackbox-exporter/tags/) in the Docker Hub Registry.

```bash
$ docker pull bitnami/blackbox-exporter:[TAG]
```

If you wish, you can also build the image yourself.

```bash
$ docker build -t bitnami/blackbox-exporter:latest https://github.com/bitnami/bitnami-docker-blackbox-exporter.git
```

# Connecting to other containers

Using [Docker container networking](https://docs.docker.com/engine/userguide/networking/), a different server running inside a container can easily be accessed by your application containers and vice-versa.

Containers attached to the same network can communicate with each other using the container name as the hostname.

## Using the Command Line

### Step 1: Create a network

```bash
$ docker network create blackbox-exporter-network --driver bridge
```

### Step 2: Launch the Blacbox_exporter container within your network

Use the `--network <NETWORK>` argument to the `docker run` command to attach the container to the `blackbox-exporter-network` network.

```bash
$ docker run --name blackbox-exporter-node1 --network blackbox-exporter-network bitnami/blackbox-exporter:latest
```

### Step 3: Run another containers

We can launch another containers using the same flag (`--network NETWORK`) in the `docker run` command. If you also set a name to your container, you will be able to use it as hostname in your network.


# Configuration

Blackbox exporter is configured via a configuration file and command-line flags (such as what configuration file to load, what port to listen on, and the logging format and level).

The default location for the config file is `/opt/bitnami/blackbox-exporter/conf/config.yml`, you can mount a volume there in order to overwrite it.

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

[Further information](https://github.com/prometheus/blackbox-exporter/blob/master/CONFIGURATION.md)

# Logging

The Bitnami blackbox-exporter Docker image sends the container logs to the `stdout`. To view the logs:

```bash
$ docker logs blackbox-exporter
```

You can configure the containers [logging driver](https://docs.docker.com/engine/admin/logging/overview/) using the `--log-driver` option if you wish to consume the container logs differently. In the default configuration docker uses the `json-file` driver.

# Maintenance

## Upgrade this image

Bitnami provides up-to-date versions of blackbox-exporter, including security patches, soon after they are made upstream. We recommend that you follow these steps to upgrade your container.

### Step 1: Get the updated image

```bash
$ docker pull bitnami/blackbox-exporter:latest
```

### Step 2: Stop and backup the currently running container

Stop the currently running container using the command

```bash
$ docker stop blackbox-exporter
```

Next, take a snapshot of the persistent volume `/path/to/blackbox-exporter-persistence` using:

```bash
$ rsync -a /path/to/blackbox-exporter-persistence /path/to/blackbox-exporter-persistence.bkp.$(date +%Y%m%d-%H.%M.%S)
```

You can use this snapshot to restore the database state should the upgrade fail.

### Step 3: Remove the currently running container

```bash
$ docker rm -v blackbox-exporter
```

### Step 4: Run the new image

Re-create your container from the new image, [restoring your backup](#restoring-a-backup) if necessary.

```bash
$ docker run --name blackbox-exporter bitnami/blackbox-exporter:latest
```

# Contributing

We'd love for you to contribute to this container. You can request new features by creating an [issue](https://github.com/bitnami/bitnami-docker-blackbox-exporter/issues), or submit a [pull request](https://github.com/bitnami/bitnami-docker-blackbox-exporter/pulls) with your contribution.

# Issues

If you encountered a problem running this container, you can file an [issue](https://github.com/bitnami/bitnami-docker-blackbox-exporter/issues). For us to provide better support, be sure to include the following information in your issue:

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
