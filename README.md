# ubuntu-python3

[![Build Status](https://travis-ci.org/FNNDSC/ubuntu-python3.svg?branch=master)](https://travis-ci.org/FNNDSC/ubuntu-python3)

Source for a slim Ubuntu-based Python3 image

03-Aug-2018
* Update for 18.04 (LTS) base image.

13-May-2020
* Update for 20.04 (LTS) base image.

26-May-2020
* Automatic multiarch build on Travis.

09-Jun-2020
* Add DEBIAN_FRONTEND=noninteractive env variable.

## Manifest List

Create a multi-architecture manifest list.

<details>
<summary>
<b>Background</b>
</summary>

A single tag in a Docker registry (e.g. `docker.io/ubuntu:bionic` or `docker.io/fnndsc/ubuntu-python3:latest`) can refer to multiple images for different architectures. When a local docker engine executes `docker pull` or `FROM` in a Dockerfile, the correct image that matches the host's native architecture is used.

Images that use `FROM fnndsc/ubuntu-python3:latest` in their Dockerfile (most `fnndsc/pl-*` plugins for ChRIS) can also be built for multiple architectures. You can simply build the same Dockerfile on different hosts without modification, or follow the [steps below](#build) to build cross-platform.

Note that [Dockerhub cannot do autobuilds for non-x86_64 images.](https://github.com/docker/hub-feedback/issues/1779#issuecomment-478100972). Autobuilds from Dockerhub (again, most `fnndsc/pl-*`) are only for x86_64. `fnndsc/ubuntu-python3` is built using [Travis](#Travis-Automatic-Builds), which is a bit less convenient.

</details>


### Build

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

# Travis Automatic Builds

[![Travis Badge](https://travis-ci.org/FNNDSC/ubuntu-python3.svg?branch=master)](https://travis-ci.org/github/FNNDSC/ubuntu-python3)

Travis automatically builds and pushes [`master`](https://github.com/FNNDSC/ubuntu-python3/tree/master) branch to [Dockerhub](https://hub.docker.com/r/fnndsc/ubuntu-python3/tags) as `fnndsc/ubuntu-python3:latest`. This manifest list will contain images for x86_64 and PowerPC.

If the latest commit is tagged (`git tag <tag_name>`), then the automatic build will also have that tag in addition (`fnndsc/ubuntu-python3:latest` and `fnndsc/ubuntu-python3:<tag_name>`). In that case we should update https://github.com/FNNDSC/cookiecutter-chrisapp Dockerfile to use the new `fnndsc/ubuntu-python3:<tag_name>`.
