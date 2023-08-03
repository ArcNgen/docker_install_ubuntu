# Install [Docker](https://docs.docker.com/engine/install/ubuntu/) on ubuntu

## Remove existing docker services

Remove existing docker services that conflict with installing, or updating docker.

```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

## Update software and operating system

Update apt packages

```bash
sudo apt update
sudo apt upgrade
sudo apt autoclean
sudo apt autoremove
```

## Install [GNU Privacy Guard](https://gnupg.org/)

```bash
sudo apt install ca-certificates curl gnupg
```

## Install Docker GPG key

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

## Setup repository

```bash
echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## Run update again

Update apt packages

```bash
sudo apt update
sudo apt upgrade
sudo apt autoclean
sudo apt autoremove
```

## Install Docker engine

```bash
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

docker --version

docker images
```

## Permission denied error

Fix permission denied while trying to connect to the Docker daemon.

```bash
# list groups your user account is associated with
groups $USER

# verify that the docker group has been added to your system
grep -E "docker" /etc/group

# add the docker group to your user account
sudo usermod -aG docker $USER

# verify your user account was added to the docker group
groups $USER

# login to new group
newgrp docker 
```

Test if docker permissions are working properly.

```bash
docker --version       # works

docker ps -a           # still get permission denied

docker run hello-world # still get permission denied

reboot

# change user permissions on docker.sock if still having issues 
sudo chmod 666 /var/run/docker.sock
```

## Run hello world test image

```bash
docker run -rm hello-world
```

## Uninstall docker

```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

sudo rm -rf /var/lib/docker

sudo rm -rf /var/lib/containerd
```
