# RHEL8 docker installation

_These instructions are adapted from the article found [here](https://linuxconfig.org/how-to-install-docker-in-rhel-8)_.

### Instructions

First, we need to add and enable the docker-ce repo.
 ```
$ sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
```
Then we want to go ahead and install docker. Unfortunately, the most recent version has at least one compatibility issue
with RHEL8 so, we will use the `--nobest` flag to get the most recent compatible version.
```
$ sudo dnf install --nobest docker-ce
```
In order for DNS resolution to work inside docker containers we must also disable `firewalld`.
```
$ sudo systemctl disable firewalld
```
Finally, enable the docker daemon.
```
$ sudo systemctl enable --now docker
```
Ensure its working by running the following.
```
$ systemctl is-active docker
active
```
