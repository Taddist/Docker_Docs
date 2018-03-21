# How To Install and Use Docker on Ubuntu 16.04

Docker is an open source tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

To follow this tutorial, you will need the following :
- 64-bit Ubuntu 16.04 server
- Non-root user with sudo privileges

All the commands in this tutorial should be run as a non-root user. If root access
is required for the command, it will be preceded by sudo

### Step 1 — Installing Docker

The Docker installation package available in the official Ubuntu 16.04 repository may not
be the latest version. To get the latest and greatest version, install Docker from the official
Docker repository. This section shows you how to do just that.

First, add the GPG key for the official Docker repository to the system :
```
curl -fsSL https ://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add the Docker repository to APT sources :
```
add-apt-repository "deb [arch=amd64] https ://download.docker.com/linux/ubuntu
$(lsb_release -cs) stable"
```
Next, update the package database with the Docker packages from the newly added repo :
```
apt-get update
```
Finally, install Docker :
```
apt-get install -y docker-ce
```
Docker should now be installed, the daemon started, and the process enabled to start on
boot. Check that it’s running :
```
systemctl status docker
```
