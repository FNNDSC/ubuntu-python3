# ubuntu-python3

Source for a slim Ubuntu-based Python3 image

03-Aug-2018
* Update for 18.04 (LTS) base image.

13-May-2020
* Update for 20.04 (LTS) base image.

# Manifest List

First, make sure you have Docker CLI experimetnal features enabled.

```bash
docker buildx create --name moc_builder --use
docker buildx build -t fnndsc/ubuntu-python3:20.04 --platform linux/amd64,linux/ppc64le --push .
docker buildx rm
```