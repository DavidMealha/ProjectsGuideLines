# How to run Docker containers on Linux Ubuntu 101

## Install Docker

* To install Docker on Ubuntu 16.04 it is necessary to add it to the package manager (apt-get).

```shell
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

* After adding the repository over HTTPS it is time to install Docker.

```shell
sudo apt-get update
sudo apt-get install docker-ce
sudo docker --version
```

## Add the application code to your machine

* Fork all repositories from microservices-demo in the GitHub website.
* Install git if necessary.

```shell
sudo apt-get install git
```

* Then, clone each repository to your local machine.

```shell
git clone https://github.com/YOUR_USERNAME/REPOSITORY_NAME.git
```

## Build your Docker images from the Dockerfile

* Go into the folder root of the project
* Run the following command, it will build the new image with the dependencies set in the Dockerfile.

```shell
docker build -t NAME_OF_MY_AWESOME_IMAGE
```

* In order to verify your images use the command:

```shell
docker images
```

## Build your Docker containers after creating the images

* Use the following command (it will start the service and it will be acessible through http://localhost:PORT)

```shell
docker run -d -p 8080:8079 NAME_OF_MY_AWESOME_IMAGE
```

* Install docker and docker-compose on linux virtual machine
