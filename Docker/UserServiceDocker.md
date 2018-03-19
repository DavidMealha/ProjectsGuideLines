# Run user service and database

Get the repository from GitHub, this one has some changes compared to the original version. This is due to the errors caused by the integration of monitoring and tracing tools.

```shell
go get -u github.com/DavidMealha/user
```

## Using docker-compose

First we build the images by installing go and its dependencies on the service image. In the database image the mongodb is installed.

```shell
docker-compose build
```

After building the images are ready to be used. The up command will start the conntainers.

```shell
docker-compose up
```

## Replicate Mongo database with Docker
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
