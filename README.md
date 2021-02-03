# fnndsc/ubuntu-python3

[![Build](https://github.com/FNNDSC/ubuntu-python3/workflows/Build/badge.svg)](https://github.com/FNNDSC/ubuntu-python3/actions)

https://hub.docker.com/r/fnndsc/ubuntu-python3/tags

A base docker image for `python3` in `ubuntu:latest`

- `fnndsc/ubuntu-python3` uses `ENTRYPOINT ["python3"]`
- `fnndsc/python` uses `CMD ["python3"]`


## Supported Platforms

- x86_64 `linux/amd64`
- PowerPC `linux/ppc64le`
- ARM64 `linux/arm64`


## Changelog

03-Aug-2018

* Update for 18.04 (LTS) base image.

13-May-2020

* Update for 20.04 (LTS) base image.

26-May-2020

* Automatic multiarch build on Travis.

09-Jun-2020

* Add DEBIAN_FRONTEND=noninteractive env variable.

03-Feb-2021

* migrate to Github Actions
* automatic tagging of Ubuntu and Python version
* Add ARM support
* Add .dockerignore


## Manifest List

"Manifest list" is the technically correct term for a multi-architectural docker container image.

<details>
<summary>
<b>Background</b>
</summary>

A single tag in a Docker registry (e.g. `docker.io/ubuntu:bionic` or `docker.io/fnndsc/ubuntu-python3:latest`) can refer to multiple images for different architectures. When a local docker engine executes `docker pull` or `FROM` in a Dockerfile, the correct image that matches the host's native architecture is used.

Images that use `FROM fnndsc/ubuntu-python3:latest` in their Dockerfile (most `fnndsc/pl-*` plugins for ChRIS) can also be built for multiple architectures. You can simply build the same Dockerfile on different hosts without modification, or follow the [steps below](#build) to build cross-platform.

Note that [Dockerhub cannot do autobuilds for non-x86_64 images.](https://github.com/docker/hub-feedback/issues/1779#issuecomment-478100972). Autobuilds from Dockerhub (again, most `fnndsc/pl-*`) are only for x86_64. `fnndsc/ubuntu-python3` is built using Github Actions, which is a bit less convenient.

</details>


### Local Build

Linux x86_64 (check with `uname -sm`) can use QEMU emulation to do cross-platform builds for foreign architectures.

First, make sure you have Docker CLI experimetnal features enabled.
https://github.com/docker/buildx#docker-ce

```bash
docker run --rm --privileged aptman/qus -s -- -p ppc64le
docker buildx create --name moc_builder --use
docker buildx build -t fnndsc/ubuntu-python3 --platform linux/amd64,linux/ppc64le .
# clean up
docker buildx rm
docker run --rm --privileged aptman/qus -- -r
```
