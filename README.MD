## image
  - a bundle of an app's dependencies (apache, sql, redis, etc) and configuration
  - made up of layers of other images (linux image, then applications (sql image, php image, etc))
  - the artifact that can be moved around (put in repo, deployed) is the image
  - makes development easier
  - makes deployment easier
  - live in a "container repository" (a private one, or a public one like DockerHub)
  - not running
  

## container
  - the running intance of an image. ie. when pull an image to your machine and start it, it creates the container environment
  - when you restart it, everything within it is gone

````

`docker pull apache:8.04`        Download an image of a specific version
`docker images`                  Lists all downloaded images
`docker rmi abc123`              Delete image abc123
`docker run apache`              Download apache image (if dont have it yet), creates a container and runs the container.
                                 -d             run in detached mode (just so dont have to open new terminal)
                                 -p8000:80      use host port 8000 to access port 80 in the container
                                 --name         give it a name so can refer to it by name instead of id
                                 --net my-name  which docker network to use

`docker ps`                      List running containers
`docker ps -a`                   List running and stopped containers

`docker start abc123`            Start container abc123 (does not create it)
`docker stop abc123`             Stop container abc123
`docker rm abc123`               Remove container abc123
`docker exec -it abc123`         Access the `interactive terminal` inside the container (not all terminal commands exist, though)

`docker logs abc123`             Show the logs for container abc123

`docker network create my-name`  Create a docker network (so two apps on different ports in the container can talk to each other natively, i think)
`docker network ls`              List the docker networks


`docker-compose up`              Start all the containers defined in docker-compose.yml (so dont have to run one for each service). It automatically creates the docker network, so we dont have to do it manually.
`docker-compose down`            Stop all the containers defined in docker-compose.yml and the network.
`docker-compose run`             ...

````

## Dockerfile
  - https://stackoverflow.com/a/52496820/1532919
  - optional
  - blueprint for building an image. eg install debian, php, mysql, pdo, imagemagick, copy in these files
  - It defines your app's environment so it can be reproduced anywhere.
  - so Dockerfile installs the dependecies/services, docker-compose.yml configures them and makes them communicate with each other...?
  - useful for deployment
  - i believe Dockerfile creates an image, then you consume it in `docker-compose.yml` with `build: .`. If images already exist in the docker registry, just use it
  - whenever you change, you have to delete and re-create the images and containers

  - example:

```
    FROM apache:8.16.2          // must be based off a single image. this one installs a specific version of apache
    ENV DB_USERNAME=admin \     // can set up environment variables
        DB_PASSWORD=password
    RUN mkdir /my/path          // run any linux command WITHIN the container
    COPY . /my/path             // "COPY" copies from HOST machine into the container
    CMD ["node", "server.js"]   // entrypoint command; only one can exist. runs "node server.js" at command line in container
```

`docker build -t my-app .`       Builds an image called `my-app` from a `Dockerfile` (which is in current directory).  
