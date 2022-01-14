# Docker Reference Guide

## Section 3: Creating and Using Containers

### Check Docker Install and Config

  * command: `docker version`
    * to verify CLI can talk to docker engine
  * command: `docker info`
    * most config values of engine
  * docker command line structure
    * old way(still functional): `docker <command> (options)`
    ex: `docker run`
    * new way: `docker <command> <sub-command> (options)`
    ex: `docker container run`

### Starting a Nginx Web Server

#### Image vs. Container
__Image__
  * an image is a binaries and libraries that all make up your application
  * in this docker learning experience series our image will be open-source Nginx web server
  * we get all of our images from "registry", and default one for docker is Docker Hub (hub.docker.com)
  * "registry" are kind of like what gitHub is for source code. Image registry are for container images.
__Container__
  * a container is a running instance of that image
  * you can have many container running off the same image

#### Start a new container and use some docker commands to manage that container

  * command: `docker container run --publish 80:80 nginx`
  * docker engine looked for an image 'nginx' and pulled down latest image of 'nginx' from Docker Hub
  * then it started it as a new process in a new container for us to use.
  * the __publish__ part of the command exposes my local port 80 on my local machine and sends all traffic from it to the executable running inside that container on port 80.
_________________________________________________

  * command: `run --publish 80:80 --detach nginx`
  * since we don't want the server to be running all the time, we use __--detach__; it tells Docker to run it in the background. This command returns with a unique container ID of your container. Every time you run a new container you get unique id.

  * command: `docker container run --publish 80:80 --detach --name webhost nginx`
  * alt command: `docker container run -d -p 80:80 --name webhost nginx`
  * This will create a docker container with name webhost
_________________________________________________

  * command: `docker container ls`
  * command to list our containers

_________________________________________________

  * command: `docker container stop <id>` 
  * command to stop the docker container.
_________________________________________________

  * command: `docker container logs webhost`
  * this will spit out latest logs in webhost
_________________________________________________

  * command: `docker container rm <id> <id>`
  * this will remove multiple docker container with specific id at once
  * command: `docker container ls -a`
  * you can view the id of docker container by listing all the docker container 
_________________________________________________

  * command: `docker container rm -f d30`
  * for force remove

### What happens in `docker container run`

  * when we run `docker container run` in the background it is going to look for the image that we specified at the end of the command locally in the image cache
  * if nothing is found locally, then it looks remote image repository(defaults to Docker Hub)
  * by default it will look there, download it, and store it in the image cache (nginx: latest by default)
  * creates a new container based on that image and prepares to start
  * it is going to customize the networking, it is going to give it a specific virtual IP address on a private network inside docker image
  * it is going to open up port if we specify in __--publish__, if we did not specify in __--publish__ then it is not going to open any ports
  * since we did the 80:80, that's telling it to take post 80 in the host and forward all that traffic to port 80 in the container
  * then that container will start using command specified in the Dockerfile

### Container vs VMs

#### Containers aren't Mini-VM's

  * They are just processes
  * Limited to what resource they can access
  * Exit when process stops
_________________________________________________

  * command: `docker run --name mongo -d mongo`
  * this will start a mongo database with name "mongo" using mongo image and will run in the background
_________________________________________________

  * command: `docker top mongo`
  * this will let me list the processes that are running inside a specific container
_________________________________________________

  * command: `ps aux | grep mongo`
  * this command will show process with name "mongo" running.
  * in macOS by default you won't be able to list the docker processes running in the host because Docker in Mac is actually running a tiny Linux VM in the background. So in order to run `ps aux` you will need to run it from inside tiny Linux VM.
  * This site has information how to run tiny Linux VM in your mac [Getting into Local Docker VM](https://www.bretfisher.com/docker-for-mac-commands-for-getting-into-local-docker-vm/)
_________________________________________________

  * command: `docker image ls` 
  * list the images 

### What's going on in Containers

  * command: `docker container top`
  * process list in one container
  _________________________________________________

  * command: `docker container inspect`
  * to look for how the container started and how it is configured
  _________________________________________________

  * command: `docker container stats`
  * this gives performance stats for all containers

### Getting a Shell Inside Container
  How to get into a container and do things from inside of a container. 

  * command: `docker container run -it`
  * in above command `-it` is two separate options. 
  * t gives us pseudo-TTY, or a prompt that simulates a real terminal, like what SSH does.
  * -i interactive, keep session open to receive terminal input
_________________________________________________

  * command: `docker container run -it --name proxy nginx bash`
  * after the image you want to run you can add arguments to run inside that image from command. 
  * In above case you will go inside a nginx image with name "proxy" and start a bash shell.
_________________________________________________

  * command: `docker container run -it --name ubuntu ubuntu`
  * this will download the image of full distribution of linux, ubuntu image. 
    * command: `apt-get update` 
    * inside this container you could use ubuntu package manager
    * command: `apt-get install -y curl`
    * installs curl inside ubuntu image.
  * you could exit out of this ubuntu server but if you start a new ubuntu then this new server will not have curl installed, you will have to install that again. 
  * to use same ubuntu image you will have to use following command.
  * command: `docker container start -ai ubuntu`
_________________________________________________

  * command: `docker container exec -it`
  * cmd: `docker container exec -it <containerName> command`
  * cmd: `docker container exec -it elasticsearch:2 bash`
  * what if I want to see the shell inside a running container that's already running mysql or nginx. we can use above command.
  * Run additional process in running container. 
_________________________________________________

  * Alpine is another small security focused distribution of linux.
  * It is only 5 meg in size
  * Alpine is so small it does not have bash in it, but it does have sh
  * we can install using package manager of Alpine which 'apk' if you want to install bash

### Docker Networks: Concepts

  * command: `docker container run -p` 
  * command: `docker container run -p 80:80 --name webhost -d nginx`
  * -p (publish) command exposes the port on your machine, but there is a lot more going on in the background.
  * command: `docker container port <container>`
  * quick port check to see what ports are open in a container

#### Docker Network Defaults

  * When you start a container in the background you are connecting to a docker network, by default that is a "bridge" network
  * each one of the those networks that you would connect to route out through NAT firewall, which is actually Docker daemon configuring the host IP address on your default interface, so that your container can get out to the internet or the rest of the network and then get back.
  * we don't need to use -p when all containers on a virtual network want to talk to each other.
  
  * Batteries included, but removable
    * Defaults work well in many cases, but easy to swap out parts to customize it
  * some of the things you could change is like creating multiple virtual networks. 
  * attach containers to more than one virtual network (or none)
  * Skip virtual networks and use host IP (--net=host), you will loose some containerization benefits - it might be required
  * use different Docker network drivers to gain new abilities
________________________________________________________

  * command: `docker container port webhost`
  * which ports are forwarding traffic to that container in the host and to the container itself
________________________________________________________

  * you might assume that container is using same IP as the host but by default that is not true.
________________________________________________________

  * command: `docker container inspect --format "{{.NetworkSettings.IPAddress}}" webhost`
  * `--format` is a common option for formatting the output of commands using "Go templates" 
  * above command is used to look up ip address of container host. 
________________________________________________________

  * command: `ifconfig en0`
  * to look up ip address of macOS

### Docker Networks: CLI Management

  * command: `docker network ls`
  * show networks
________________________________________________________

  * command: `docker network inspect <network name>`
  * cmd: `docker network inspect bridge`
  * this will show us details about a specific network, what containers are attached and more.
________________________________________________________

  * command: `docker network create --driver`
  * this command has optional `--driver` that we can specify using builtin and optional third party driver to create a new virtual network.
  * cmd: `docker network create my_app_net`
  * creates new virtual network with driver of bridge because that's the default driver
________________________________________________________

  * command: `docker network connect`
  * attach a network to container
  * cmd: `docker network connect db8e3de9acd8 c6389434ac6d`
    * `docker network connect <network> <container>`
  * this command will connect a container to a specific network. a container is already connected to default bridge network, after using above command it will be connected to both bridge and another specified network
________________________________________________________

  * command: `docker network disconnect`
  * cmd: `docker network disconnect db8e3de9acd8 7c78656274b0`
    * `docker network disconnect <network> <container>`
  * detach a network from container 
  * these connect and disconnect command is for changing a live running container, so that a new NIC is created on a virtual network for that container. It is kind of like a sticking a network card on a computer while it is running.
  * you could connect more than one network to a container
________________________________________________________

  * network name: bridge
  * bridge is a default network that bridges through the NAT firewall to the physical network that your host is connected to 
  * up until now we did not have to worry about it because all of our containers were just attached to that network and it worked
________________________________________________________

  * network name: host
  * it is a special networking that skips the virtual networking of docker and attaches the container directly to the host interface. 
  * It prevents the security boundaries of the containerization from protecting the interface of that container.
  * in some situation it can improve the performance of high throughput networking and get around a few other issues with specific special software. 
  ________________________________________________________

  * network name: none
  * removes eth0 and only leaves you with localhost interface in container
  ________________________________________________________

  * cmd: `docker container run -d --name new_nginx --network my_app_net nginx`
  * to run a nginx container in background and attach a network my_app_net
  
### Docker Networks: DNS
You are gonna learn about DNS, and how it affects containers in custom networks and default networks. 

* Having good DNS is important because we can't rely on IP addresses to inside containers since things are so dynamic.

* DNS is the standard for how we do intercommunication between containers on the same host and across host.

* It is recommended to create custom networks rather than using `--link` all the time

### Assignment - 2

#### Start two linux distribution
  cmd: `docker container run --rm -it centos:7 bash`
  * gets centos:7 image and runs bash inside it 
  * --rm removes the centos:7 after exiting

    * curl command comes with centos image
    * package manager for centos is yum
________________________________________________________   
  cmd: `docker container run --rm -it ubuntu:14.04 bash`
  * gets ubuntu:14.04 image and runs bash inside
  * --rm removes the centos:7 after exiting

    * cmd: `apt-get update && apt-get install -y curl`
    * installs curl from apt-get package manager

### Assignment: DNS Round Robin Test - 3


  1. create a network
    * cmd: `docker network create sup`
  
  2. create two container, run them in the background and attach network sup to the container with elasticsearch:2 image
    * cmd:`docker container run -d --network sup --network-alias search elasticsearch:2` 
    * alt cmd: `docker container run -d --net sup --net-alias search elasticsearch:2`
    * --network-alias <name>, so I could find them with the DNS name search
  * since name is not specified you could use same above command to create another container

  3. this should run nslookup command in the 'search' DNS entry and immediately exit and remove alpine image because of --rm switch
    * cmd: `docker container run --rm --net dude alpine nslookup search.`
  
  4. this will give back elasticsearch server name, although you are running same DNS search there will be different elastic search server
    * cmd: `docker container run --rm --net dude centos curl -s search:9200`

## Section 4: Container Images, Where to Find Them and How to Build Them

### What's in an image (And what isn't)

  * App binaries and dependencies for you app
  * Metadata about the image data and how to run the image

  * Inside a image there is not a complete OS. No kernel, kernel modules(e.g. drivers), it is just the binaries that your application needs because host provides the kernel
  * this is one the distinct characteristics around container that makes it different from virtual machine, it that it is not booting up a full OS it is really just starting an application.

### The Mighty Hub: Using Docker Hub Registry Images

  * Docker hub has many different images such as nginx, mysql, etc
  [Docker Hub Registry](hub.docker.com)
  * typically use official images
  * official images have just the name without forward slash

#### To pull docker images
  * this will download most latest image from docker hub registry
    * cmd: `docker pull nginx` 

  * this will download image with version specified.
    * alt: `docker pull nginx:1.21.5`

### Images and Their Layers: Discover Image Cache

  * Images are made up of file system changes and metadata
  * each layer is uniquely identified as a SHA, and only stored once on a host
  * this storage space on host and transfer time on push/pull
  * When you start a container it is just a single layer of changes on top of image
  * `docker history:mysql` and `docker image inspect nginx`, teach us what's going on inside a image and how it was made

### Image Tagging and Pushing to Docker Hub

  * All about image tags
    * There is lot to tag because they're not as intuitive as you might think when you first start using them
  
  * How to upload to Docker Hub
    * They need to be in a specific format in order to work with a registry
  
  * Image ID vs Tag
    * Image ID is a unique id
    * if multiple tags belong to a image then all tags will have same image ID
    
  * this will add new tag to the existing image. it will have same image ID with name as nirajanp/nginx in repositories
    * cmd: `docker image tag  nginx nirajanp/nginx`
      `docker image tag <source_image:[tag]> <target_image:[tag]>`
  
  * to upload this into Docker Hub you need to login from CLI
    * cmd: `docker login`
  
  * to logout
    * cmd: `docker logout`
  
  * to push image in Docker Hub
    * cmd: `docker image push nirajanp/nginx`
  
  * to give a tag name use this command
    * cmd: `docker image tag nirajanp/nginx nirajanp/nginx:testing`
  
### Building Images: The Dockerfile Basics

  * Dockerfile is a recipe of creating images
  * all of the images that is created are created by using Dockerfile
  * a Dockerfile looks like a shell script but it is not a shell script, neither batch file
  * it is totally different language of a file that is unique to docker
  * its default name is Dockerfile with capital D
  * with the command line whenever you need to deal with the Dockerfile using docker command you could use -f to specify a different file than default
  * e.g cmd: `docker build -f some-dockerfile`

#### First Step in Dockerfile

  * first Step in Dockerfile is a __FROM__ command
  * it is required to be in every docker file
  * it specifies a minimal distribution like debian or alpine
  * the reason you would use these is to save time and pain because these minimal distribution are much smaller than CD's you would use to install a virtual machine from them 
  * one of the main benefits of using them in container is to use their package distribution system to install whatever software you need in your package

#### Next we have ENV 

  * __ENV__ is for environment variables, it is a way to set environment variables
  * it is important because they are the main way we set key and value for container building and for running container

__NOTE: Order of command matters because it works Top-Down__

#### RUN command

  * you will usually see __RUN__ commands when you install software with package repository, or need to do some unzipping, or some file edits inside container itself

#### EXPOSE command

  * by default no TCP or UDP ports are open inside a container
  * it does not expose anything from container to a virtual network unless we specify in __EXPOSE__ command
  * this __EXPOSE__ command does not mean that ports are going to be opened up in the host, that's what -p command is for when we use docker run

#### CMD command

  * this is a required command
  * it is the final command it will run every time you launch a new container from a image
  * or every time you restart stopped container

### Building Images: Running Docker Builds

  * go to the directory where there is Dockerfile, this file has instruction on how to build our image

  * FROM cmd: when i build this image it is gonna pull the 'debian:stretch-slim' image in from docker hub down to my local cache and execute line by line each of those stanza(command) inside my docker engine and cache each of those layers
  
  * below command builds Dockerfile in the same directory with the name customnginx specified by using -t
  * cmd: `docker image build -t customnginx .`

  * since Dockerfile runs from top to bottom, if any change is made then it will run from where the change is made. thus it is recommended to have lines that change more at the bottom of the Dockerfile and line that changes less at the beginning. 

### Building Images: Extending Official Images

#### Write a Dockerfile to change the default nginx page and change to custom HTML

  1. We want the latest nginx image so in Dockerfile we add this
    __FROM nginx:latest__ 
  2. this WORKDIR is really running cd: change directory. here we are changing to a default nginx directory for its html files
    __WORKDIR /usr/share/nginx/html__
  3. this is the stanza that you would use to copy your source code from your local machine, or your build servers, into your container images
    * in this case we are overriding the index.html file of nginx in its home with custom defined html for 
  __COPY__ index.html index.html

### Build Your own Dockerfile

  1. make changes in your Dockerfile
  2. build the Dockerfile. this is gonna build the docker file with image name testnode in current directory
    * cmd: `docker build -t testnode .`
  3. then run the container 
    * cmd: `docker container run --rm -p 80:3000 testnode` locally 
  
  * Now to push this to Docker Hub, you need to change its name first
    1. cmd: `docker image tag testnode nirajanp/testnode`
    2. push to Docker Hub `docker push nirajanp/testnode`
    3. Now if you remove nirajanp/testnode from local environment, and try to run this container, docker will get that image from Docker Hub Registry and run it locally
    4. cmd: `docker container run --rm -p 80:3000 nirajanp/test-app`

## Section 5: Container Lifetime & Persistent Data: Volumes

### Container Lifetime & Persistent Data

  * Containers are usually immutable and ephemeral (i.e. unchanging, temporary)
  * the idea here is we can just throw away a container and create a new one with a image
  * "immutable infrastructure": only re-deploy containers, never change
  * this is the ideal scenario, but what about databases, or unique data?
    * ideally the container should not contain your unique data with application binaries, this is known as separation of concerns. 
  * docker gives us features to ensure these "separation of concerns"
  * the problem of unique data is known as "persistent data"
  * in new world of container and application scaling persistent data causes a unique problem
    * Docker has two solutions to this problem
      1. Volumes or Data Volumes
      2. Bind Mounts
 
 #### Volumes 
 Docker volumes are configuration options for containers that creates a special location outside of that container Union File System(UFS) to store unique data. this preserves it across container removals and allows us to attach it to whatever container we want

 #### Bind Mounts 
 which are simply us sharing or mounting a host directory, or file, into a container. it will just look like a local file path, or a directory path, to the container, it won't actually know it is coming from the host

### Persistent Data: Data Volumes

  * The first way you can tell a container that it needs to worry about a __Volume__ is in a __Dockerfile__
  * __Volume__ need manual deletion, it won't be deleted by removing a container
  
  * to list volumes in your docker
  * cmd: `docker volume ls`
________________________________________________________
  * all the volumes are have a long code which are not user friendly and thus it is difficult to distinguish which container a certain volume belongs to

  * there are also __named volumes__ which are friendly way to assign volumes to containers by using -v while running a container 
  * cmd: ` docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql`
________________________________________________________

  * why would you want to `docker volume create`?
    we can create them from a docker container run command at runtime, and we can create them by specifying them in the Dockerfile, there is only a few cases where you'd want to create it ahead of time. 
  * By using this option we could specify a different driver for volume

### Persistent Data: Bind Mounting

#### Bind mount
  * it is just a mapping of the host files, or directories, into a container file or directory
  * in the background it's basically just having two locations point to the same file
  * it skips UFS like other volumes do so that it's not going to wipe out your host location when you delete the container
  * you can't use them in Dockerfile, must be at container run
  * cmd: `docker container run -v /Users/np/stuff:/path/container` (mac/linux)
  * cmd: `docker container run -v //c/Users/np/stuff:/path/container` (windows)
  * the way docker tells different between named volume and bind mount is that, bind mount starts with a forward slash (/)

#### Trying bind mount

  * in 48_PersistentData-BindMounting folder there are two files i.e. Dockerfile and index.html
  * the Dockerfile is configured to run nginx server and load custom designed index.html instead of default nginx webpage
  * one way of doing this is by building the Dockerfile as image and running the container
  * __another way is by using bind mounting__
  * cmd:  `docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx`
  * this command runs a nginx container in background, and it maps path of current directory of host to the directory in container
  * any change made on host will also reflect in container and vice versa
  


