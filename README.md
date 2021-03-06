# docker-aeron

[![Travis Status](https://api.travis-ci.org/neomantra/docker-aeron.svg?branch=master)](https://travis-ci.org/github/neomantra/docker-aeron)

Docker tooling for [Aeron Messaging](https://github.com/real-logic/aeron).

----

# Docker Registry

[Travis CI](https://travis-ci.org/neomantra/docker-aeron) builds Aeron images for the the [Docker Registry](https://hub.docker.com/r/neomantra/aeron-cpp-debian).

Our Docker images are available from the Docker Registry at:
  * `neomantra/aeron-<dist-type>:<aeron-version>`

The following image are available:

  * `neomantra/aeron-cpp-debian:latest`
  * `neomantra/aeron-cpp-debian:1.28.2`

----

# GitLab Registry

[GitLab CI](https://gitlab.com/neomantra/oss/aeron/pipelines) builds Aeron images for the the [GitLab Registry](https://gitlab.com/neomantra/oss/aeron/container_registry).

*NOTE:* Our GitLab Registry is not publicly accessible yet.  Use the Docker Registry.

Our Docker images are available from the GitLab.com registry at:
  * `registry.gitlab.com/neomantra/oss/aeron/<dist-type>:<version>`

The following image are available:

  * `registry.gitlab.com/neomantra/oss/aeron/cpp-debian:master`
  * `registry.gitlab.com/neomantra/oss/aeron/cpp-debian:1.28.2`

----

## Running the "Aeron Media Driver"

The Docker image's `ENTRYPOINT` is `/usr/local/bin/aeronmd` so the media driver will be launched by `docker run`:

```
docker run --shm-size=128M --ipc host --network host registry.gitlab.com/neomantra/oss/aeron:cpp-debian-1.28.0
```

**NOTE:** By default, Linux Docker [only allocates 64 MB](https://github.com/docker/docker/issues/2606) to the [shared memory space](https://www.cyberciti.biz/tips/what-is-devshm-and-its-practical-usage.html) at `/dev/shm`.  So `docker run` with `--shm-size` or `docker-compose` with an `shm-size:` parameter.

**NOTE:** The `--ipc` parameter and `--network` parameters control how [the container can share](https://docs.docker.com/engine/reference/run/#ipc-settings---ipc) to the host's shared memory and network subsystems.

## Configuring the Aeron Media Driver"

The C media driver [can be configured](https://github.com/real-logic/aeron/wiki/Configuration-Options#c-media-driver) with arguments like `-D<option>=<value>` or with environment variables like `AERON_PRINT_CONFIGURATION`.

```
docker run --env AERON_PRINT_CONFIGURATION=1 --shm-size=256M --ipc host --network host registry.gitlab.com/neomantra/oss/aeron/cpp-debian:1.28.0

docker run --shm-size=256M --ipc host --network host registry.gitlab.com/neomantra/oss/aeron/cpp-debian:1.28.0 -DAERON_PRINT_CONFIGURATION=1
```

## Building

```
# Build Aeron GitHub master
docker build  -f Dockerfile_cpp_debian -t aeron-cpp-debian .

# Build a specific Aeron GitHub release
docker build  -f Dockerfile_cpp_debian --build-arg AERON_VERSION=1.28.0 -t aeron-cpp-debian:1.28.0 .

```

| Key  | Default | Description |
|:---- | :-----: |:----------- |
| AERON_BUILD_IMAGE_BASE | "debian" | Docker image name to build Aeron from | 
| AERON_BUILD_IMAGE_TAG | "stretch-slim" | Tag of Docker image to build Aeron from | 
| AERON_RUNTIME_IMAGE_BASE | "debian" | Docker image name to install Aeron into | 
| AERON_RUNTIME_IMAGE_TAG | "stretch-slim" | Tag of Docker image to install Aeron into | 
| AERON_VERSION | "master" | GitHub release to build Aeron from |
| AERON_JLEVEL | 1 | `--j` level to build Aeron with |
| AERON_CTEST | "" | Set to non-empty to run `ctest` after build |


### License

Copyright (c) 2017-2020 Neomantra BV

Released under the MIT License, see LICENSE.txt
