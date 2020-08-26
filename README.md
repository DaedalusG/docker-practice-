# App Academy Week 19

- [App Academy Week 19](#app-academy-week-19)
  - [Threading](#threading)
    - [Threading in Python](#threading-in-python)
  - [Apline Linux](#apline-linux)
  - [Docker](#docker)
    - [Containers](#containers)
    - [Networking](#networking)
    - [Persistent Data](#persistent-data)
    - [Images](#images)
    - [Dockerfile](#dockerfile)
    - [Docker Compose](#docker-compose)
  - [Microservices](#microservices)

## Threading

- Programs generally boil down ti adding and maniputlating elements in memory
- Variablles are just memory addresses
- A thread is just code running line by line on a specific CPU
  - Uses a program counter to keep track of program execution
- JS has a signle thread of execution
- Concurrency
  - A thread is a separate and complete sequence of execution
  - You have no control which CPU runs the thread
  - Thread Synchronization
    - Uses special features to avoid thread interruption
    - Used by Python
      - CPython has only one thread of execution
    - Lock
      - Blocks interruptions
      - Only one lock can be up at a time
      - Deadlocks can occur when a lock is trying to acquire a lock another thread has
        - Hard to predict when a deadlock can happen
    - Barrier
    - Semaphore
  - Single Thread
    - Avoid interruptions by not making them a possibility
  - No shared mutable memory
    - Two threads cannot access the same piece of memory at once

### Threading in Python

- Import threading
- Instantiate threading.Thread, give it a `target` fuunction, and any args you want passe3d
- Call the thread instance's start method to begin execution
- sleep() will pause execution for a gicen amount of seconds
- daemon or background threads stop when the main thread of execution ends
- threading.Lock() will create a lock that can be used with Python's `with` keyword to create a locked block of code
- Functions that share locks should always acquire and release locks in the same order
- The thread instance's join method will join that thread to the main thread, ensuring the main thread cannot end before the thread instance is finished
  - Can only be called by the thread that created the thread instance

## Apline Linux

- Apline Package Kepper
  - Includes repositories
  - Mirrors, websites that host repositories
  - Releases- versions of a package
    - Edge is the most latest release
  - apk add will install a given package
  - apk search will search for a given package
  - apk del will uninstall a given package
    - Adding -r will also remove packages that depend on it
  - `apk upgrade --available` will install all available updates
  
## Docker

- Standardizes the way code is deployed
- Usually run community version
- `docker info` will give a general overview of the current state of docker
- Images can be uploaded ti and downloaded from DockerHub

### Containers

- Requires an image to run
- The image is an application you want to run
- `docker container run ${image}` starts a container
  - `--name` will specifiy which container to run
  - `-p` specifies the port
    - The port before the : is the port that will be exposed by the computer
    - The port after the : is the port the container is exposing
  - `-d` will run a container in the background
  - Docker will first look at the local computer for the image, then go to it's own repos to find the image
- `docker container rm ${name/id}` will remove a container
- `docker container stop ${name/id}` will stop the container
- `docker container inspect ${name/id}` will give details about a container
  - Hash of the image it's running
  - Where it is currently being hosted
- `docker container top ${id/name}` will show the processes running in a container
- `docker container stats` and `docker container stats` will show information about the all the containers and overall docker environment

### Networking

- Docker handles a lot of the lower level netowrking
- If you do not specify a network, docker will give a container a virtual IP to run off of the bridge network
- `docker network ls` will show all containers that are running on the network
- The Bridge Network
  - Runs off of the bridge driver
  - Containers can talk to each other without exposing a port
- `docker network create --name ${name}` will create a network and return the id of the network
- The bridge driver is the default driver
- Containers do not expose any ports by default, you have to use the `-p` flag
- You can add a container to a network with the run command by adding the `--net` flag and providing a network name
- Containers can be on multiple networks
- You should create a new network for each application

### Persistent Data

- Docker containers are designed to be ephemeral and immutable
- Bind Mounts
  - An address on a local computer that docker will save to
  - `--mount type=bind source="$(pwd/somewhere else on the local computer)", target=${some place on the image}` on docker run will add a bind mount
  - The location on the container will be completely taken over by the host directory
- Docker Volumes
  - A directory docker is in charge of
  - More secure
  - Stores data that a container generates
  -  Look for the volumes key in `docker container inspect`
  -  Adding the `-v` 
  -  You can share volumes across containers
  -  `docker volume ls` and `docker volume inspect` will give information about volumes
  - Volumes can live outside of the container lifecycle

### Images

- Collection of app binaries and dependencies
- Contain layers and metadata
- Has a unique ID
- Created by building a dockerfile
- Images can have tags
  - Versions of the image are common tags
- You can inspect the metadata of the image
- Each layer of an image has a hash id
- Each layer represents a command in the dockerfile
- An image is just a collection of file system changes

### Dockerfile

- From - indicates what image you are building off of
  - alpine is a good choice
- COPY - will copy all of the files from a directory to the image
- WORKDIR - indicates the pwd for building
- RUN - will execute the given command
- EXPOSE - the given port will be exposed on the container
- CMD - The command to run to start the container
- To build from a dockerfile, run `docker build ${dir}`
  - You can add a -t to give the image a name and a tag
- To push to dockerhub
  - `docker login`
  - `docker push ${image}`
  - If the image is deleted. Docker will automatically pull the image from docker hub
- Custom images always have a name before a / followed by the image name, official images just have the name and a badge
  - Check folllows and stars before you pull a user's image

### Docker Compose

## Microservices