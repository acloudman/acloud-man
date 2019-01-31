---
layout: post
title:  "Docker Tutorial"
date:   2019-01-31 01:00:00 +0530
categories: cloud-man blog
---

## Definition of: Docker

The term Docker can refer to

* The Docker project as a whole, which is a platform for developers and sysadmins to develop, ship, and run applications
* The docker daemon process running on the host which manages images and containers (also called Docker Engine)

## Definition of: image

Docker images are the basis of containers. An Image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime. An image typically contains a union of layered filesystems stacked on top of each other. An image does not have state and it never changes.

## Definition of: container

A container is a runtime instance of a docker image.

A Docker container consists of

* A Docker image
* An execution environment
* A standard set of instructions

**Docker is all about speed**

*  Develop faster
*  Build faster
*  Test faster
*  Deploy faster
*  Update faster
*  Recover faster

{% highlight pygments %}
Management Commands:

  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes
{% endhighlight %}

{% highlight pygments %}
Commands:

  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

{% endhighlight %}

Run `docker COMMAND --help` for more information on a command.

## docker info

# Description
Display system-wide information
# Usage

`docker info [OPTIONS]`
    
## Docker old command(still works)
    
docker <command> (options)

Ex: `docker run --name mongo -d mongo`

## Docker new command

Docker <command> <sub-command> (options)

Ex: `docker container run --name mongo -d mongo`

## docker image

# Description

Manage images

# Usage

`docker image COMMAND`

| Command | Description |
|-------|--------|
| docker image build | Build an image from a Dockerfile | 
| docker image history | Show the history of an image | 
|docker image import|	Import the contents from a tarball to create a filesystem image|
|docker image inspect|	Display detailed information on one or more images|
|docker image load|	Load an image from a tar archive or STDIN
|docker image ls|	List images|
|docker image prune|	Remove unused images|
|docker image pull|	Pull an image or a repository from a registry|
|docker image push|	Push an image or a repository to a registry|
|docker image rm|	Remove one or more images|
|docker image save|	Save one or more images to a tar archive (streamed to STDOUT by default)|
|docker image tag|	Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE|

Ex: `docker images ls`

## **Container**

* A container is an instance of the image running as a process.
* You can have many container running off the same image.
* They are just processes.
* Limited to what resources they can access.
* Exit when process stops.

## docker pull

# Description
Pull an image or a repository from a registry(docker hub)

# Usage

`docker pull [OPTIONS] NAME[:TAG|@DIGEST]`

Ex: `docker pull alpine`

## docker container run

# Description
Run a command in a new container

# What happens in `docker container run`?

* Looks for that image locally in image cache, doesn’t find anything.
* Then looks in remote image repository(defaults to docker hub).
* Downloads the latest version(nginx: latest by default).
* create new container based on that image and prepares to start.
* Gives it a virtual IP on a private network inside docker engine.
* Opens up port on host and forward to port 80 in container.
* Starts container by using CMD in the image Dockerfile.

# Usage
`docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]`

Ex: `docker container run --publish 80:80 nginx`

`--publish` we can also use `-p`

* Downloaded image ‘nginx’  from Docker Hub.
* Started a new container from that image.
* Opened port 80 on the host IP.
* Routes that traffic to the container IP, port 80.

Ex 1: `docker container run -d --rm -it centos:7 bash`

`--rm`   Automatically remove the container when it exits

Ex 2: `docker container run --publish 80:80 --detach nginx`

`--detach` or `-d` will run the container in background 

Returns the container ID

## Download Images from Docker Hub

`docker pull alpine`

## List both running and stopped containers

`docker container ls -a`

## Start container with name

`docker container run --publish 80:80 --detach --name webhost  nginx`

## Logs
`Docker container logs <container name/ID>`

## Display the running process of a container
`docker container top webhost`

## Removing containers

`docker container rm 35a09467a331`

We can’t remove container which is running, before removing container it should stop.

There is way to remove running container using `-f` option.

`docker container rm -f 35a09467a331`

We can also remove multiple containers(passing container ids with `,`).

`docker container rm -f 35a09467a331, 35a09467b123`

## Example of changing the defaults
{% highlight pygments %}

docker container run —publish 8080:80 —name webhost -d nginx:1.11 nginx -T

{% endhighlight %}

`8080:80` --> change host listening port
`nginx:1.11` --> change version of image
`nginx -T` --> Change CMD run on start

## Examples

### Important example for run, stop, remove containers mysql, nginx, apache web server(httpd)

{% highlight pygments %}

➜  ~ docker run --name demo-mysql -e MYSQL_ROOT_PASSWORD=cloudman -d mysql:latest

--name demo-mysql —> any name as your convenience
latest -> TAG(version)
-e  —> env settings
-d  —> detach

➜  ~ docker container run --name webserver -d -p 8080:80 httpd

-p 8080:80
    Map TCP port 80 in the container to port 8080 on the Docker host. 
    8080 is local machine port 
    80 is container port

➜  ~ docker container run -d --name proxy -p 80:80 nginx

{% endhighlight %}

### Stop all containers by name or id

`docker container stop proxy demo-mysql webserver`

### Removing all containers

`docker container rm 305d6af8dbb4 7ab459cb9293 71ee9eb1b220 fb0e36775eea 04c9b0606365`

## Whats going on in Containers

1. docker container top —> process list in one container
2. docker container inspect —>details of one container config
3. docker container stats —> performance stats for all containers 

### Examples

{% highlight pygments %}
docker container top nginx
docker container inspect nginx

docker container stats
docker container stats nginx(optional)

{% endhighlight %}

## Getting a Shell inside Containers

`
docker container run -it
`
Start new container interactively.

If run with -it, it will give you a terminal inside the running container.
{% highlight pygments %}
-t, --tty                          Allocate a pseudo-TTY
-i, --interactive                  Keep STDIN open even if not attached
tty -- print the file name of the terminal connected to standard input
{% endhighlight %}

**Open a shell in new container** 
If container not exists, It will Download and run a shell inside container .

{% highlight pygments %}
docker container run -it --name proxy nginx bash 
docker container run -it --name ubuntu ubuntu (default is bash no need to specify)

docker container run -it nginx bash (container name is same as image name, container exists)

{% endhighlight %}

### Open a shell in existing stopped container 

`docker container start -ai ubuntu`

### Open a shell in newly pulled container(not running, docker pull alpine)

docker container run -it alpine bash

docker container run -it alpine sh


### docker container exec -it

Run additional command in existing container (run a command in a running container)

Run a command in a running container.    

`docker container exec -it mysql bash`

## Docker Networks:Concepts

1. Each container connected to a private virtual network “bridge"
2. Each virtual network routes through NAT firewall on host IP
3. All container on a virtual network can talk to each other without -p
4. Batteries included, But removable         
    1.   Default work well in many cases, but easy to swap out parts to customise it.
5. Make new virtual networks
6. Attach containers to more than one virtual network(or none)
7. Skip virtual networks and use host IP(- -net=host)
8. Use different docker network drivers to gain new abilities.


`docker container port webhost(name of the container)`

`docker container inspect --format NetworkSettingsIPAddress webhost`

## Docker Networks:CLI Management

{% highlight pygments %}
➜  ~ docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
1205b22546fa        bridge              bridge              local
9cdc56ab3568        host                host                local
c93e211909f5        none                null                local

{% endhighlight %}

*bridge network bridge*:
> Default docker network virtual network, which is NAT’ed behind the host IP.

*host network*:
> It gains performance by skipping virtual networks but sacrifices security of container model.

*none network*:
> Removes eth0 and only leaves you with localhost interface in container.

## Create new Network

`docker network create my_app-net(network name)`

Spawns a new virtual network for you to attach containers to.

{% highlight pygments %}
docker network ls -> list network 
docker network inspect host -> inspect network i.e host or bridge or none 
docker network inspect bridge -> inspect network i.e host or bridge or none

{% endhighlight %}

### Create new container with specific network
`docker container run -d --name new_nginx --network my_app-net nginx`

### Docker network connect

Dynamically create a NIC in a container on an existing virtual network.

**Example:**

`docker network connect <network-id> <container-id>`

{% highlight pygments %}

"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "1205b22546faba877537cba789c96b33b760217544b8f81ccdbdb2a13dd55b44",
                    "EndpointID": "9613581f65bf1a9c796db8682ef90a54647c86578af1e686ade912cf494606ff",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                },
                "my_app-net": {
                    "IPAMConfig": {},
                    "Links": null,
                    "Aliases": [
                        "fbf0b0bf3406"
                    ],
                    "NetworkID": "46358d45d60ca9ba2d875503f672d531a78862138ea5f4a180ea5d100d29b31c",
                    "EndpointID": "5a95bbf85eba20e81d301c3dbd011966175da5bcbdeea1dfc3d4bb6b99219800",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:03",
                    "DriverOpts": null
                }
            }
        }

{% endhighlight %}

### Docker network disconnect:
Dynamically removes a NIC from a container on a specific virtual network.

**Example:**

`docker network disconnect <network-id> <container-id>`

{% highlight pygments %}

"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "1205b22546faba877537cba789c96b33b760217544b8f81ccdbdb2a13dd55b44",
                    "EndpointID": "9613581f65bf1a9c796db8682ef90a54647c86578af1e686ade912cf494606ff",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }

{% endhighlight %}


## Docker Networks:Default Security

* Create your apps so frontend/backend sit on the same docker container.
* Their inter-communication never leaves host
* All externally exposed ports closed by default.
* You must manually expose via -p, which is better default security!
* This gets even better later with swarm and overlay networks.

## Docker Networks:DNS

**Docker DNS:**
> Docker daemon has a built-in DNS server that containers use by default.
 
**DNS Default names**
> Docker defaults the hostname to the container’s name, but you can also set aliases.
    
            
 `docker container exec -it my_nginx ping new_nginx (vice-versa two containers)`

* Containers shouldn’t rely on IP’s. For inter-communication.
* DNS for friendly names is built-in if you use custom networks.
* Your using custom networks right?
* This gets way easier with docker compose.


## What’s In An Image(And What Isn't)

* App binaries and dependencies
* Metadata about the image data and how to run the image.
* Official definition: “An Image is on ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime."
* Not complete OS.No kernel, kernel modules(e.g. drivers)
* Small as one file(your app binary) like a gaoling binary
* Big as a Ubuntu bistro with apt, and Apache, PHP, and more installed

## Image and Their layers:Review

* Images are made up of file system changes and metadata
* Each layer is uniquely identified and only stored once on a host
* This saves storage space on host and transfer time on push/pull
* A container is just a single/write layer on top of image
* Docker image history and inspect commands can teach us

**CMD:** `docker image history <image name>:tag`

**Old:** `docker history <image name>:tag`

Show the lanes of changes made in image

**CMD:** `docker image inspect <image name>:tag`
        
Returns JSON metadata about the image

## Image Tagging And Pushing to Docker Hub

# Description

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

# Usage

{% highlight pygments %}

➜  ~ docker login/logout
➜  ~ cat .docker/config.json —> credential will store 

Usage:    docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

➜  ~ docker image tag nginx avinash/nginx


{% endhighlight %}

## Pushing Image to Docker Hub:

`docker image push avinashkunuje/nginx`

## Building Images

Sample Docker file

{% highlight pygments %}

FROM node:6-alpine

EXPOSE 3000

RUN apk add --update tini

RUN mkdir -p /usr/src/app
# - Node uses a "package manager", so it needs to copy in package.json file

WORKDIR /usr/src/app

COPY package.json package.json

RUN npm install && npm cache clean --force

# - then it needs to run 'npm install' to install dependencies from that file
# - to keep it clean and small, run 'npm cache clean --force' after above
# - then it needs to copy in all files from current directory

COPY . .

# - then it needs to start container with command '/sbin/tini -- node ./bin/www'

CMD ["tini", "--", "node", "./bin/www"]
{% endhighlight %}

### Running Our Image

`docker build -t testnode .`

`docker container run -p 80:80 --rm testnode`


## Container lifetime & persistent Data

* Containers are usually immutable and ephemeral
* “Immutable infrastructure”: only re-deploy containers, never change
* This is the ideal scenario, but what about databases, or unique data?
* Docker gives us features to ensure these “separation of concerns"
* This is known as “persistent data"
* Two ways: volume and bind Mounts
* Volumes: make special outside of container UFS

Note: You might want to do a `docker volume prune` to cleanup unused volumes and make it easier to see what you’re doing here.

`docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql`

## listing volumes

{% highlight pygments %}

➜  ~ docker volume ls
DRIVER              VOLUME NAME
local               0a330159d2b861b9de736b49dfe293a1064fe37d5758661248bb389306275d51
local               2fab9a24e03b6470626bb12ad50d49ee2f782ef4b6232c3dd5cde87b1363dc86

{% endhighlight %}


## Inspecting volumes


`docker volume inspect b9c654ab35fb8ba2fefa1082e559bbfc4a9f5d3aba8a27e1647816c2ccdd0cdf(volume id)`

{% highlight pygments %}

[
    {
        "CreatedAt": "2019-01-22T08:34:45Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/b9c654ab35fb8ba2fefa1082e559bbfc4a9f5d3aba8a27e1647816c2ccdd0cdf/_data",
        "Name": "b9c654ab35fb8ba2fefa1082e559bbfc4a9f5d3aba8a27e1647816c2ccdd0cdf",
        "Options": null,
        "Scope": "local"
    }
]
{% endhighlight %}

## Named Volumes

Friendly way to assign volumes to containers.

`docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql`

{% highlight pygments %}

➜  ~ docker volume ls
DRIVER              VOLUME NAME
local               mysql-db
➜  ~ docker volume inspect mysql-db
[
    {
        "CreatedAt": "2019-01-22T09:02:09Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/mysql-db/_data",
        "Name": "mysql-db",
        "Options": null,
        "Scope": "local"
    }
]
{% endhighlight %}


## docker volume create

Required to do this before “docker run” to use custom drivers and labels.

`docker volume create`

## Persistant Data: Bind Mounting

* Maps host file or directory to a container file or directory
* Basically just two locations pointing to the same file(s).
* Again, skips UFS, host files overwrite any in container
* Can’t use in docker file, must be at container run.
* … run -v /Users/bret/stuff:/path/container (Mac/linux)

{% highlight pygments %}

Manage volumes

Commands:
  create      Create a volume
  inspect     Display detailed information on one or more volumes
  ls          List volumes
  prune       Remove all unused local volumes
  rm          Remove one or more volumes

{% endhighlight %}

### Basically just two locations pointing to the same file(s).

{% highlight pygments %}

docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx

Host terminal

touch testme.txt
echo 'is it me you're looking for' > testme.txt

Docker terminal

docker container exec -it nginx bash

root@e32d5efcbc4b:/usr/share/nginx/html# ls
Dockerfile  index.html    testme.txt

{% endhighlight %}


## Docker compose

* Why: configure relationship between containers.
* Why: save our docker container run settings in easy-to-read file
* Why: create one-liner developer environment startups
* Comprised of 2 separate but related things 
* 1. YAML-formatted file that describes our solution options for:
    * Containers
    * Networks 
    * volumes
* 2.CLI tool docker-compose used for local dev/test automation with those YAML files


## Docker-compose.yml

* Compose YAML format has its own versions:1,2,2.1,3,3.1
* YAML file can be used with docker-compose command for local docker automation or..
* With docker directly in production with swarm(as of v1.13)
* Docker-compose —help 
* Docker-compose.yml is default filename, but any can be  used with docker-compose -f

## Docker-compose CLI

* CLI tool comes with docker for windows/Mac, but separate download for linux
* Not a production grade tool but ideal for local development and test
* Two most common commands are
    * Docker-compose up #setup volumes/networks and start all containers
    * Docker-compose down # stop all containers and remove cont/vol/net
* If all your projects had a Dockerfile and docker-compose.yml then “new developer Onboarding" would be:
    * Git clone GitHub.com/some/software
    * Docker compose up

`docker-compose up`

`docker-compose up -d —>background`

`docker-compose log   —> logs`

{% highlight pygments %}

➜  docker-compose --help
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file
                              (default: docker-compose.yml)
  -p, --project-name NAME     Specify an alternate project name
                              (default: directory name)
  --verbose                   Show more output
  --log-level LEVEL           Set log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --no-ansi                   Do not print ANSI control characters
  -v, --version               Print version and exit
  -H, --host HOST             Daemon socket to connect to

  --tls                       Use TLS; implied by --tlsverify
  --tlscacert CA_PATH         Trust certs signed only by this CA
  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
  --tlskey TLS_KEY_PATH       Path to TLS key file
  --tlsverify                 Use TLS and verify the remote
  --skip-hostname-check       Don't check the daemon's hostname against the
                              name specified in the client certificate
  --project-directory PATH    Specify an alternate working directory
                              (default: the path of the Compose file)
  --compatibility             If set, Compose will attempt to convert deploy
                              keys in v3 files to their non-Swarm equivalent

Commands:
  build              Build or rebuild services
  bundle             Generate a Docker bundle from the Compose file
  config             Validate and view the Compose file
  create             Create services
  down               Stop and remove containers, networks, images, and volumes
  events             Receive real time events from containers
  exec               Execute a command in a running container
  help               Get help on a command
  images             List images
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pull service images
  push               Push service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  top                Display the running processes
  unpause            Unpause services
  up                 Create and start containers
  version            Show the Docker-Compose version information

{% endhighlight %}


{% highlight pygments %}

➜  docker-compose ps
          Name                   Command          State         Ports
----------------------------------------------------------------------------
compose-sample-2_proxy_1   nginx -g daemon off;   Up      0.0.0.0:80->80/tcp
compose-sample-2_web_1     httpd-foreground       Up      80/tcp

➜  docker-compose top
compose-sample-2_proxy_1
PID    USER   TIME                    COMMAND
---------------------------------------------------------------
3645   root   0:00   nginx: master process nginx -g daemon off;
3944   101    0:00   nginx: worker process

compose-sample-2_web_1
PID    USER   TIME        COMMAND
---------------------------------------
3606   root   0:00   httpd -DFOREGROUND
3839   bin    0:00   httpd -DFOREGROUND
3840   bin    0:00   httpd -DFOREGROUND
3841   bin    0:00   httpd -DFOREGROUND

➜  docker-compose down
Stopping compose-sample-2_web_1   ... done
Stopping compose-sample-2_proxy_1 ... done
Removing compose-sample-2_web_1   ... done
Removing compose-sample-2_proxy_1 ... done
Removing network compose-sample-2_default

➜  docker-compose ps
Name   Command   State   Ports

{% endhighlight %}


{% highlight pygments %}

➜  docker-compose down -v
Removing avinash_drupal_1   ... done
Removing avinash_postgres_1 ... done
Removing network avinash_default
Removing volume avinash_drupal-modules
Removing volume avinash_drupal-profiles
Removing volume avinash_drupal-sites
Removing volume avinash_drupal-themes

{% endhighlight %}

## Swarm Mode: Buil-In Orchestration

* Swarm mode is a clustering solution built inside Docker
* Not related to Swarm “classic” for pre-1.12 versions
* Added in 1.12(summer 2016) via SwarmKit toolkit
* Enhanced in 1.13(January 2017) via stacks and secrets
* Not enabled by default, new commands once enabled
    * docker swarm 
    * docker node
    * docker service
    * docker stack
    * docker secret


{% highlight pygments %}

➜  ~ docker swarm init

docker node ls
ID                            HOSTNAME                STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
l6j4g375xigi6uk46rlyo1bmt *   linuxkit-025000000001   Ready               Active              Leader              18.09.1
{% endhighlight %}


{% highlight pygments %}

➜  ~ docker swarm --help

Usage:    docker swarm COMMAND

Manage Swarm

Commands:
  ca          Display and rotate the root CA
  init        Initialize a swarm
  join        Join a swarm as a node and/or manager
  join-token  Manage join tokens
  leave       Leave the swarm
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm

{% endhighlight %}

{% highlight pygments %}

➜  ~ docker service --help

Usage:    docker service COMMAND

Manage services

Commands:
  create      Create a new service
  inspect     Display detailed information on one or more services
  logs        Fetch the logs of a service or task
  ls          List services
  ps          List the tasks of one or more services
  rm          Remove one or more services
  rollback    Revert changes to a service's configuration
  scale       Scale one or multiple replicated services
  update      Update a service

{% endhighlight %}

{% highlight pygments %}

docker update --help

Usage:    docker update [OPTIONS] CONTAINER [CONTAINER...]

Update configuration of one or more containers

Options:
      --blkio-weight uint16        Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --cpu-period int             Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int              Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int          Limit the CPU real-time period in microseconds
      --cpu-rt-runtime int         Limit the CPU real-time runtime in microseconds
  -c, --cpu-shares int             CPU shares (relative weight)
      --cpus decimal               Number of CPUs
      --cpuset-cpus string         CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string         MEMs in which to allow execution (0-3, 0,1)
      --kernel-memory bytes        Kernel memory limit
  -m, --memory bytes               Memory limit
      --memory-reservation bytes   Memory soft limit
      --memory-swap bytes          Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --restart string             Restart policy to apply when a container exits
➜  ~ docker service update --help

Usage:    docker service update [OPTIONS] SERVICE

Update a service

Options:
      --args command                       Service command args
      --config-add config                  Add or update a config file on a service
      --config-rm list                     Remove a configuration file
      --constraint-add list                Add or update a placement constraint
      --constraint-rm list                 Remove a constraint
      --container-label-add list           Add or update a container label
      --container-label-rm list            Remove a container label by its key
      --credential-spec credential-spec    Credential spec for managed service account (Windows only)
  -d, --detach                             Exit immediately instead of waiting for the service to converge
      --dns-add list                       Add or update a custom DNS server
      --dns-option-add list                Add or update a DNS option
      --dns-option-rm list                 Remove a DNS option
      --dns-rm list                        Remove a custom DNS server
      --dns-search-add list                Add or update a custom DNS search domain
      --dns-search-rm list                 Remove a DNS search domain
      --endpoint-mode string               Endpoint mode (vip or dnsrr)
      --entrypoint command                 Overwrite the default ENTRYPOINT of the image
      --env-add list                       Add or update an environment variable
      --env-rm list                        Remove an environment variable
      --force                              Force update even if no changes require it
      --generic-resource-add list          Add a Generic resource
      --generic-resource-rm list           Remove a Generic resource
      --group-add list                     Add an additional supplementary user group to the container
      --group-rm list                      Remove a previously added supplementary user group from the container
      --health-cmd string                  Command to run to check health
      --health-interval duration           Time between running the check (ms|s|m|h)
      --health-retries int                 Consecutive failures needed to report unhealthy
      --health-start-period duration       Start period for the container to initialize before counting retries towards unstable (ms|s|m|h)
      --health-timeout duration            Maximum time to allow one check to run (ms|s|m|h)
      --host-add list                      Add a custom host-to-IP mapping (host:ip)
      --host-rm list                       Remove a custom host-to-IP mapping (host:ip)
      --hostname string                    Container hostname
      --image string                       Service image tag
      --init                               Use an init inside each service container to forward signals and reap processes
      --isolation string                   Service container isolation mode
      --label-add list                     Add or update a service label
      --label-rm list                      Remove a label by its key
      --limit-cpu decimal                  Limit CPUs
      --limit-memory bytes                 Limit Memory
      --log-driver string                  Logging driver for service
      --log-opt list                       Logging driver options
      --mount-add mount                    Add or update a mount on a service
      --mount-rm list                      Remove a mount by its target path
      --network-add network                Add a network
      --network-rm list                    Remove a network
      --no-healthcheck                     Disable any container-specified HEALTHCHECK
      --no-resolve-image                   Do not query the registry to resolve image digest and supported platforms
      --placement-pref-add pref            Add a placement preference
      --placement-pref-rm pref             Remove a placement preference
      --publish-add port                   Add or update a published port
      --publish-rm port                    Remove a published port by its target port
  -q, --quiet                              Suppress progress output
      --read-only                          Mount the container's root filesystem as read only
      --replicas uint                      Number of tasks
      --reserve-cpu decimal                Reserve CPUs
      --reserve-memory bytes               Reserve Memory
      --restart-condition string           Restart when condition is met ("none"|"on-failure"|"any")
      --restart-delay duration             Delay between restart attempts (ns|us|ms|s|m|h)
      --restart-max-attempts uint          Maximum number of restarts before giving up
      --restart-window duration            Window used to evaluate the restart policy (ns|us|ms|s|m|h)
      --rollback                           Rollback to previous specification
      --rollback-delay duration            Delay between task rollbacks (ns|us|ms|s|m|h)
      --rollback-failure-action string     Action on rollback failure ("pause"|"continue")
      --rollback-max-failure-ratio float   Failure rate to tolerate during a rollback
      --rollback-monitor duration          Duration after each task rollback to monitor for failure (ns|us|ms|s|m|h)
      --rollback-order string              Rollback order ("start-first"|"stop-first")
      --rollback-parallelism uint          Maximum number of tasks rolled back simultaneously (0 to roll back all at once)
      --secret-add secret                  Add or update a secret on a service
      --secret-rm list                     Remove a secret
      --stop-grace-period duration         Time to wait before force killing a container (ns|us|ms|s|m|h)
      --stop-signal string                 Signal to stop the container
  -t, --tty                                Allocate a pseudo-TTY
      --update-delay duration              Delay between updates (ns|us|ms|s|m|h)
      --update-failure-action string       Action on update failure ("pause"|"continue"|"rollback")
      --update-max-failure-ratio float     Failure rate to tolerate during an update
      --update-monitor duration            Duration after each task update to monitor for failure (ns|us|ms|s|m|h)
      --update-order string                Update order ("start-first"|"stop-first")
      --update-parallelism uint            Maximum number of tasks updated simultaneously (0 to update all at once)
  -u, --user string                        Username or UID (format: <name|uid>[:<group|gid>])
      --with-registry-auth                 Send registry authentication details to swarm agents
  -w, --workdir string                     Working directory inside the container

{% endhighlight %}


{% highlight pygments %}

➜  ~ docker service create alpine ping 8.8.8.8
s9lqyzqcdav6kwhlvz2inneg5
overall progress: 1 out of 1 tasks
1/1: running   [==================================================>]
verify: Service converged

{% endhighlight %}

{% highlight pygments %}

➜  ~ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
s9lqyzqcdav6        youthful_jones      replicated          1/1                 alpine:latest

{% endhighlight %}

{% highlight pygments %}

➜  ~ docker service ps youthful_jones
ID                  NAME                IMAGE               NODE                    DESIRED STATE       CURRENT STATE            ERROR               PORTS
7w3idmkhzuc3        youthful_jones.1    alpine:latest       linuxkit-025000000001   Running             Running 56 seconds 

{% endhighlight %}

{% highlight pygments %}

➜  ~ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
96d41ed969c8        alpine:latest       "ping 8.8.8.8"      About a minute ago   Up About a minute                       youthful_jones.1.7w3idmkhzuc3351s19wdknu2y

{% endhighlight %}

{% highlight pygments %}

➜  ~ docker service update s9lqyzqcdav6kwhlvz2inneg5(service id) --replicas 3
s9lqyzqcdav6kwhlvz2inneg5
overall progress: 3 out of 3 tasks
1/3: running   [==================================================>]
2/3: running   [==================================================>]
3/3: running   [==================================================>]
verify: Service converged

{% endhighlight %}

{% highlight pygments %}

➜  ~ docker service ps youthful_jones
ID                  NAME                IMAGE               NODE                    DESIRED STATE       CURRENT STATE            ERROR               PORTS
7w3idmkhzuc3        youthful_jones.1    alpine:latest       linuxkit-025000000001   Running             Running 3 minutes ago
7tbloky1qm2y        youthful_jones.2    alpine:latest       linuxkit-025000000001   Running             Running 17 seconds ago
6zs0sjdynovj        youthful_jones.3    alpine:latest       linuxkit-025000000001   Running             Running 17 seconds ago

{% endhighlight %}


{% highlight pygments %}

➜  ~ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ce61bf7a02ae        alpine:latest       "ping 8.8.8.8"      3 minutes ago       Up 3 minutes                            youthful_jones.2.7tbloky1qm2yr2rmfgnlnx6q1
b015cc8d9db9        alpine:latest       "ping 8.8.8.8"      3 minutes ago       Up 3 minutes                            youthful_jones.3.6zs0sjdynovjhch0u4bujfc70
96d41ed969c8        alpine:latest       "ping 8.8.8.8"      6 minutes ago       Up 6 minutes                            youthful_jones.1.7w3idmkhzuc3351s19wdknu2y

{% endhighlight %}


{% highlight pygments %}

➜  ~ docker container rm -f youthful_jones.1.7w3idmkhzuc3351s19wdknu2y(name.1.id)
youthful_jones.1.7w3idmkhzuc3351s19wdknu2y

{% endhighlight %}


{% highlight pygments %}

➜  ~ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
s9lqyzqcdav6        youthful_jones      replicated          2/3                 alpine:latest

{% endhighlight %}

{% highlight pygments %}

➜  ~ docker service ps youthful_jones
ID                  NAME                   IMAGE               NODE                    DESIRED STATE       CURRENT STATE            ERROR                         PORTS
22q1tjz2d4z4        youthful_jones.1       alpine:latest       linuxkit-025000000001   Running             Running 24 seconds ago
7w3idmkhzuc3         \_ youthful_jones.1   alpine:latest       linuxkit-025000000001   Shutdown            Failed 29 seconds ago    "task: non-zero exit (137)"
7tbloky1qm2y        youthful_jones.2       alpine:latest       linuxkit-025000000001   Running             Running 4 minutes ago
6zs0sjdynovj        youthful_jones.3       alpine:latest       linuxkit-025000000001   Running             Running 4 minutes ago

{% endhighlight %}


{% highlight pygments %}

➜  ~ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ae079f99517e        alpine:latest       "ping 8.8.8.8"      38 seconds ago      Up 32 seconds                           youthful_jones.1.22q1tjz2d4z48lvxw4vgkfxlq
ce61bf7a02ae        alpine:latest       "ping 8.8.8.8"      4 minutes ago       Up 4 minutes                            youthful_jones.2.7tbloky1qm2yr2rmfgnlnx6q1
b015cc8d9db9        alpine:latest       "ping 8.8.8.8"      4 minutes ago       Up 4 minutes                            youthful_jones.3.6zs0sjdynovjhch0u4bujfc70

{% endhighlight %}



{% highlight pygments %}

➜  ~ docker service rm youthful_jones
youthful_jones

{% endhighlight %}


{% highlight pygments %}

➜  ~ docker service ls
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS

{% endhighlight %}


{% highlight pygments %}

➜  ~ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

{% endhighlight %}


