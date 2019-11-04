

```dockerfile
docker version
```

> Verify cli can talk to engine



```
docker info
```

>  Config values of engine



```
docker <command> (options) : old way

docker <command> <sub-command> (options) : new way
```

> Docker command line structure



#### Image vs Container

> **Image** is the application we want to run
>
> **Container** is an instance of that image running as a process



```
docker container run --publish 80:80 nginx
```

> 1. Download image "nginx" from docker hub
> 2. Started a new container from that image
> 3. Open port 80 from the host IP
> 4. Route that traffic to the container IP, port 80
>
> **Note**: You'll get a bind error if the left number (host port) is being used by anything else, even another container. You can use any port on the left, like **8080**:80 or **8888**:80, then use localhost:8080 to test.



#### run vs start

> **docker container run** : start a new container
>
> **docker container start** : start an existing stopped container



```
docker container logs <container name> : show logs for a specific container
docker container logs --help : see all the log options
docker container ls -a : list all the containers
docker container rm <container-id> ... : remove containers (up container can't be removed)
docker container rm -f <container-id> ... : remove containers forcely (up container)
```



#### What happens in 'docker container run'

> 1. Looks for that image locally in image cache, doesn't find anything
> 2. Then looks in remote image repository (defaults to Docker Hub)
> 3. Downloads the latest version (nginx:latest by default)
> 4. Creates a new container based on that image and prepares to start
> 5. Gives it a virtual IP on a private network inside docker engine
> 6. Opens up port 80 on host and forwards to port 80 in container
> 7. Starts container by using the CMD in the image Dockerfile



#### Example of changing the defaults

![image-20190920115821907](/Users/mensopheak/Library/Application Support/typora-user-images/image-20190920115821907.png)



#### Assignment

```
docker container run --detach --name proxy --publish 80:80 nginx

docker container run -d --name webserver -p 8080:80 httpd

docker container run --detach --name db --publish 3306:3306 -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
```



<hr/>
```
docker container inspect <container>
```

> Show metadata about the container (startup config, volumes, networking, etc)



```
docker container stats
```

> Show live performance data for all containers



```
docker container top <container>
```

> Show process list in the container



#### Getting a Shell Inside Containers

```
docker container run -it
```

> Start new container interactively



```
docker container exec -it
```

> Run additional command in existing container



> **-i** (interactive): keep session open to receive terminal input
>
> **-t** (pseudo-tty): simulates a real terminal, like what SSH does



```
docker container run -it --name proxy nginx bash
```

> Will give a terminal inside the running container



Ubuntu image (its default CMD is bash, so we don't have to specify it)

```
docker container run -it --name ubuntu ubuntu
```



```
ls -al
```

> list all directories and files as list view



#### Pull image

```
docker pull alpine
```

> Get the alpine image



```
docker image ls
```



#### Run alpine container

```
docker container run -it alpine sh
```

> By default, alpine has no bash, so we use sh
>
> If we want to install bash inside alpine container, use ```apt```  (package manager) to install programs inside it



#### Docker Networks

```bash
docker container run -p 80:80 --name nginx -d nginx

docker container port nginx
80/tcp -> 0.0.0.0:80

docker container inspect --format '{{ .NetworkSettings.IPAddress }}' nginx
172.17.0.2
```



#### Docker Networks: Default Security

* Create your apps so frontend/backend sit on same Docker network
* Their inter-communication never leaves host
* All externally exposed ports closed by default
* You must manually expose via **-p**, which is better default security
* This gets even better later with Swarm and Overly networks



#### CLI Management

* Show networks ```docker network ls```
* Inspect a network ```docker network inspect```
* Create a network ```docker network create --driver```
* Attach a network to container ```docker network connect```
* Detach a network from container ```docker network disconnect```



> --network bridge: default docker virtual network, which is NAT'ed behind the host IP
>
> --network host: it gains performance by skipping virtual networks but sacrifices security of container model
>
> --network none: removes eth0 and only leaves you with localhost interface in container



```
docker network create my_app_network
```

> Create a new virtual network as bridge by default



> network driver: built-in or 3rd party extensions that give you virtual network features



#### Docker Network: DNS

> **Forget IP's**: static IP's and using IP's for talking to containers is an anti-pattern. avoid it.



> If command ping not found in the container 
>
> ```
> apt-get update
> apt-get install inetutils-ping
> ```



* Containers should not rely on IP's for inter-communication
* DNS for friendly names is built-in if you use custom networks
* You're using custom networks right?
* This gets way easier with Docker Compose



![image-20190922123821410](/Users/mensopheak/Library/Application Support/typora-user-images/image-20190922123821410.png)



<hr/>
#### What is In An Image (And What is not)

* App binaries and dependencies
* Metadata about the image data and how to run the image
* **Official definition**: "**An image is an ordered collection of root file system changes and the corresponding execution parameters for use within a container runtime**"
* Not a complete OS. No kernel, kernel modules (e.g. drivers)
* Small as one file (your app binary) like a golang static binary
* Big as a Ubuntu bistro with apt, and Apache, PHP, and more installed



#### Image and Their Layers

* Images are made up of file system changes and metadata
* Each layer is uniquely identified and only stored once on a host
* This saves storage space on host and transfer time on push/pull
* A container is just a single read/write layer on top of image
* ```docker image history``` and ```inspect``` commands can teach us



#### Image Tagging and Pushing to Docker Hub

> **Official Repositories**, they live at the "root namespace" of the registry, so they don't need account name in front of repo name





```
Usage:	docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Example: docker image tag nginx sopheak1970/nginx
```



```
Usage:	docker image push [OPTIONS] NAME[:TAG]

Push an image or a repository to a registry

docker login
docker image push sopheak1970/nginx
[docker logout]
```



### THE Dockerfile BASIC



#### PACKAGE MANAGER

PM is like **apt** and **yum** are one of the reasons to build containers FROM Debian, Ubuntu, Fedora or CentOS



#### ENVIRONMENT VARIABLES

> One reason they were chosen as preferred way to inject key/value is they work everywhere, on every OS and config.



```
docker build -f some-dockerfile
```

> build Dockerfile with specific filename (by default, **Dockerifle**)



![image-20190927135026351](/Users/mensopheak/Library/Application Support/typora-user-images/image-20190927135026351.png)



### CONTAINER LIFETIME & PERSISTENT DATA

* Containers are usually immutable and ephemeral
* "Immutable infrastructure": only re-deploy containers, never change
* This is the ideal scenario, but what about database, or unique data?
* Docker gives us features to ensure these "separation of concerns"
* This is known as "persistent data"
* Two ways: Volumes and Bind Mounts
* Volumes: make special location outside of container UFS
* Bind Mounts: link container path to host path



#### PERSISTENT DATA (VOLUME)



```
docker volume ls
```

> List all volumes



```
docker volume inspect <volume_name>
```

> Inspect volume



```
docker container run -d --name mysqldb -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql  mysql
```

> Create volume of container (friendly volume name)



```
docker volume prune
```

> To clean up volume in the host machine which are not linked to any container



#### PERSISTENT DATA (BIND MOUNTING)



* Maps a host file or directory to a container file or directory
* Basically just 2 locations pointing to the same files
* Again, skips UFS, and host files overwrite any in container
* Can't use in Dockerfile, must be at **container run**
* ```... run -v /Users/bret/staff:/path/container```(Mac/linux)
* ```... run -v //c/Users/bret/staff:/path/container```(windows)

