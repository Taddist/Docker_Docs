# How To Install and Use Docker on Ubuntu 16.04

Docker is an open source tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

To follow this tutorial, you will need the following :
- 64-bit Ubuntu 16.04 server
- Non-root user with sudo privileges

>All the commands in this tutorial should be run as a non-root user. If root access
is required for the command, it will be preceded by sudo.

### Installing Docker

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
### Using the Docker Command
Using docker consists of passing it a chain of options and commands followed by arguments. The syntax takes this form:
```
docker [option] [command] [arguments]
```
To view all available subcommands, type:
```
docker 
```
To view system-wide information about Docker, use:
```
docker info
```
### Step 3 — Working with Docker Images
Docker containers are run from Docker images. By default, it pulls these images from Docker Hub, a Docker registry managed by Docker, the company behind the Docker project. Anybody can build and host their Docker images on Docker Hub, so most applications and Linux distributions you'll need to run Docker containers have images that are hosted on Docker Hub.

You can search for images available on Docker Hub by using the docker command with the search subcommand. For example, to search for the Ubuntu image, type:
```
docker search ubuntu
```
The script will crawl Docker Hub and return a listing of all images whose name match the search string.

In the **OFFICIAL** column, **OK** indicates an image built and supported by the company behind the project. Once you've identified the image that you would like to use, you can download it to your computer using the pull subcommand, like so:

```
docker pull ubuntu
```
After an image has been downloaded, you may then run a container using the downloaded image with the run subcommand. If an image has not been downloaded when docker is executed with the run subcommand, the Docker client will first download the image, then run a container using it:
```
docker run ubuntu
```
To see the images that have been downloaded to your computer, type:
```
docker images
```
### Running a Docker Container
let's run a container using the latest image of Ubuntu. The combination of the -i and -t switches gives you interactive shell access into the container:
```
docker run -it ubuntu
```
Your command prompt should change to reflect the fact that you're now working inside the container.Now you may run any command inside the container. For example, let's update the package database inside the container. No need to prefix any command with sudo, because you're operating inside the container with root privileges:
```
apt-get update
```
Then install any application in it. Let's install Django, for example. 
```
pip install django
```
### Committing Changes in a Container to a Docker Image
After installing Django inside the Ubuntu container, you now have a container running off an image, but the container is different from the image you used to create it.

To save the state of the container as a new image, first exit from it:
```
exit
```
Then commit the changes to a new Docker image instance using the following command. The -m switch is for the commit message that helps you and others know what changes you made, while -a is used to specify the author. The container ID is the one you noted earlier in the tutorial when you started the interactive docker session. Unless you created additional repositories on Docker Hub, the repository is usually your Docker Hub username:
```
docker commit -m "What did you do to the image" -a "Author Name" container-id repository/new_image_name
```
After that operation has completed, listing the Docker images now on your computer should show the new image, as well as the old one that it was derived from:
```
docker images
```
### Listing Docker Containers
After using Docker for a while, you'll have many active (running) and inactive containers on your computer. To view the active ones, use:
```
docker ps
```
To view all containers — active and inactive, pass it the -a switch:
```
docker ps -a
```
To view the latest container you created, pass it the -l switch:
```
docker ps -l
```
Stopping a running or active container is as simple as typing:
```
docker stop container-id
```
The **container-id** can be found in the output from the **docker ps** command.

## Supported platforms
Docker’s native platform is Linux, as it’s based on features provided by Linux kernel. However, you can still run it on macOS and Windows. The only difference is that on macOS and Windows Docker is encapsulated into a tiny virtual machine. At the moment Docker for macOS and Windows has reached a significant level of usability and feels more like a native app.

Moreover, there a lot of supplementary apps such as
**Kitematic** or **Docker Machine** which help to install and operate Docker on non Linux platforms. 

## Terminology
**Container** :running instance that encapsulates required software. Containers are always created from images.
Container can expose ports and volumes to interact with other containers or/and outer world.
Container can be easily killed / removed and re-created again in a very short time.

**Image** :basic element for every container. When you create an image every step is cached and can be reused (Copy On Write model). Depending on the image it can take some time to build it. Containers, on the other hand can be started from images right away.

**Registry** :he server that stores Docker images. It can be compared to Github — you can pull an image from the registry to deploy it locally, and you can push locally built images to the registry. 

**Docker Daemon** :The background service running on the host that manages building, running and distributing Docker containers. The daemon is the process that runs in the operating system to which clients talk to.
**Docker hub** :a registry with web-interface provided by Docker Inc. It stores a lot of Docker images with different software. Docker hub is a source of the “official” Docker images made by Docker team or made in cooperation with the original software manufacturer (it doesn’t necessary mean that these “original” images are from official software manufacturers). Official images list their potential vulnerabilities. This information is available for any logged in user. There are both free and paid accounts available. You can have one private image per account and an infinite amount of public images for free. 

## Create your own image 
To build a Docker image you need to follow these steps:

* Create a directory

A separate directory is useful to organise docker applications. For this Python Example, create a directory somewhere with name of your choice. We shall use the name **Myimage-docker**

* Create Python Application

Create a simple Python File, in the directory **Myimage-docker**, with name **python_docker.py** containing the following content :
```
import requests

print ("It's your first docker image")

r = requests.get('http://www.google.com')
print (r.status_code)
```

* Create Dockerfile 

Create a file with name Dockerfile. Dockerfile contains instructions to prepare Docker image with our Python Application.
Following is the content of Dockerfile :
```
# Use an official Python runtime as a parent image
FROM python:3.5

#Copy python script into the container at /
ADD python_docker.py /

# Install app dependencies
RUN pip3 install requests

# Run python_docker.py when the container launches
CMD [ "python3", "./python_docker.py" ]
```
>Dockerfile is a text file that contains all the commands, in order, needed to build a given image automatically. In this case python.Here is the description of the instructions we’re going to use in our next example:
>FROM — set base image
>RUN — execute command in container
>ENV — set environment variable
>WORKDIR — set working directory
>VOLUME — create mount-point for a volume
>CMD — set executable for container

* Build Docker Image
Run the following command in Terminal, from Myimage-docker directory, to create Docker Image with Python Application.

* Build the image using our directory's Dockerfile with the following command :
```
docker build -t my-docker-image .

```
**Now we have the new image and we can see it in the list of existing images:by typing docker images**
* Run Docker Image with Python Application
```
docker run my-docker-image 

```
* Save Docker Image to a tar file
Save the Docker Image file to a tar file, so that the image file could be copied to other machines through disk storage devices like pen-drive, etc.

Run the following command to save Docker image as a tar file.
```
docker save -o directory/file_name.tar my-docker-image

```
**Saving might take few seconds. Wait for the command to complete.**

Now you may copy or ship the Docker Image file that is having Python Application.
