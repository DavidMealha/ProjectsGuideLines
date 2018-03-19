# Run catalogue service and database

## Using Dockerfiles

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

## Runing with docker-compose

The docker-compose of this service is defined to build and run both services.
After a few minutes the containers will be running.

```shell
docker-compose build
docker-compose run
```
