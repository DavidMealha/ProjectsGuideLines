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
