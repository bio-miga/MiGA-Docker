# Installing Docker for Linux

Instructions for installing Docker on a Linux platform can be found at https://docs.docker.com/engine/install/. The following instructions are specific to the Ubuntu distribution.

## Requirements

- A 64-bit version of Ubuntu 18.04 or later. At the time of this writing the current LTS version is Ubuntu Focal 20.04.
- x86_64, amd64, armhf, arm64, or s390x microprocessor architecture.
 
## Uninstall old versions

Enter the following command in your terminal to uninstall any older versions of the Docker engine if they exist. It is okay to enter the command if they do not exist.

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
## Set up the Docker repository

The following commands update `apt-get` and allow it to use `HTTPS` to access repositories.

```
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
## Add Docker's GPG key

Get a key to access the Docker repository with the following command:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
## Install the Docker engine

And finally install the Docker engine with the command:

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

You can then test your installation with the command below. It should print a message and then exit.

```
sudo docker run hello-world
```
