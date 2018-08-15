# How to run Docker containers on Linux Ubuntu 101

## Install Docker

To install Docker on Ubuntu 16.04 it is necessary to add it to the package manager (apt-get).

```shell
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

After adding the repository over HTTPS it is time to install Docker.

```shell
sudo apt-get update
sudo apt-get install docker-ce
sudo docker --version
```

## Instal Docker-compose

This procedure is necessary to install the latest release that supports version 3.

```shell
curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose
chmod +x /usr/bin/docker-compose
docker-compose -v
```

## Add the application code to your machine

Fork all repositories from microservices-demo in the GitHub website.
Install git if necessary.

```shell
sudo apt-get install git
```

Then, clone each repository to your local machine.

```shell
git clone https://github.com/YOUR_USERNAME/REPOSITORY_NAME.git
```

## Build your Docker images from the Dockerfile

Go into the folder root of the project
Run the following command, it will build the new image with the dependencies set in the Dockerfile.

```shell
docker build -t NAME_OF_MY_AWESOME_IMAGE .
```

In order to verify your images use the command:

```shell
docker images
```

## Build your Docker containers after creating the images

Use the following command

```shell
docker run NAME_OF_MY_AWESOME_IMAGE
```


## Work with docker-compose

Docker-compose lets start and/or build multiple containers that are defined in a single file.
It's possible to install via apt'get install.

```shell
apt-get install docker-compose
```

## Imposing restart always on existing container

```shell
docker update --restart=always <container>
```

## Run frontend service

This service is one of the most simple, since it isn't attached to a database.
Clone the project to your prefered location, I chose $HOME/desktop

```shell
git clone https://github.com/DavidMealha/front-end
```

Build the image into your machine (you must be located in the folder of the Dockerfile). The last dot is to define the path of the Dodckerfile.

```shell
docker build -t front-end-v0.1 .

RESULT:

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
front-end-v0.1      latest              a9af6adc25b8        42 hours ago        177MB
```

Next it's time to run the container with the desired image.
This command will expose the front-end service in port 8080, which is acessible through curl or from the web browser (http://localhost:8080).

```shell
docker run -d -p 8080:8079 front-end-v0.1
```

To verify the running containers use the **ps** command. It is possible to list all the containers creating by adding the follow flag (-a).

```shell
docker ps

RESULT:

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
41e9a6f0c85a        front-end-v0.1      "/usr/local/bin/npm …"   41 hours ago        Up 2 seconds        0.0.0.0:8080->8079/tcp   focused_golick

docker ps -a

RESULT:

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS                    NAMES
41e9a6f0c85a        front-end-v0.1      "/usr/local/bin/npm …"   41 hours ago        Up 53 seconds           0.0.0.0:8080->8079/tcp   focused_golick
3f79bef2bf95        ubuntu              "/bin/bash"              4 days ago          Exited (0) 4 days ago                            compassionate_lumiere
```

You can **start or stop** a container whenever you want by defining the container id.

```shell
docker start 41e9a6f0c85a

docker stop 41e9a6f0c85a
```

To free some space you can **remove** Docker images (rmi) or containers (rm). It is not possible to remove an image that has associated containers (either running or stopped).

```shell
docker rm 41e9a6f0c85a

docker rmi a9af6adc25b8
```
