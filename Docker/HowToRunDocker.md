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
docker build -t NAME_OF_MY_AWESOME_IMAGE
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

## Run catalogue service and database

### Using Dockerfiles

Build the image with mysql installed. Go folder catalogue-db, where it's located the dockerfile for that purpose.

```shell
docker build -t catalogue-db-v0.1 .
```

List the built images.

```shell
docker images

RESULT:

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
catalogue-db-v0.1   latest              04334f4bb5e7        30 seconds ago      371MB
mysql               5.7                 5195076672a7        9 hours ago         371MB
```

Next build the service's image (go to the other folder). 
This one has two steps, the first where the go files are compiled, which take a lot of time, and then the compiled application is copied from one container to another.
Run the command on the root of the project.

```shell
docker build -t catalogue-v0.1 -f ./docker/catalogue/Dockerfile .
```

### Runing with docker-compose

The docker-compose of this service is defined to build and run both services.
After a few minutes the containers will be running.

```shell
docker-compose build
docker-compose run
```

## Run user service and database

Get the repository from GitHub, this one has some changes compared to the original version. This is due to the errors caused by the integration of monitoring and tracing tools.

```shell
go get -u github.com/DavidMealha/user
```

### Using docker-compose

First we build the images by installing go and its dependencies on the service image. In the database image the mongodb is installed.

```shell
docker-compose build
```

After building the images are ready to be used. The up command will start the conntainers.

```shell
docker-compose up
```

### Replicate Mongo database with Docker
[Source](https://www.sohamkamani.com/blog/2016/06/30/docker-mongo-replica-set/)

Since the containers are in the same machine it is necessary to create a network inside Docker.

```shell
docker network ls

RESULT:

NETWORK ID          NAME                DRIVER              SCOPE
4a1f0a263ef4        bridge              bridge              local
674b8761dbd5        catalogue_default   bridge              localhost
670b5d3476fd        host                host                local
a36bd5a3356e        my-mongo-cluster    bridge              local
734e97895a00        none                null                local
efd24e46ad50        user_default        bridge              local

docker network create my-network-name

```

Instead of executing the containers with docker-compose, use docker run (I am a newbie and do not understand how to define the network in the .yml file (: ).

```shell
docker run -p 27018:27017 --name user-db-first-replica --net my-mongo-cluster user-db-v0.1 mongod --replSet rs0 --config /etc/mongodb.conf --smallfiles
docker run -p 27019:27017 --name user-db-second-replica --net my-mongo-cluster user-db-v0.1 mongod --replSet rs0 --config /etc/mongodb.conf --smallfiles
```

Choose one of the databases/containers and connect to it through the Mongo shell.
List the existing databases and use the service's one.

```shell
docker exec -it user-db-first-replica mongo
show dbs
use users
```

Setup the replication config.

```shell
config = { "_id" : "rs0", "members" : [{ "_id" : 0, "host" : "user-db-first-replica:27017" }, { "_id" : 1, "host" : "user-db-second-replica:27017" }]}
rs.initiate(config)
```

After this, the mongo instance you chose will be the PRIMARY.

```shell
rs0:PRIMARY>
```

Then, we can connect to another replica that will be surely a SECONDARY replica. This one uses the test database initially (dont know why?!)

```shell
docker exec -it user-db-second-replica mongo

rs0:SECONDARY> show dbs
rs0:SECONDARY> use users
rs0:SECONDARY> rs.slaveOk()
rs0:SECONDARY> db.customers.find({})

```
