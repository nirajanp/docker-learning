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
  * you could exit out of this ubuntu server but if you start a new ubuntu then this new server will not have curl installer, you will have to install that again. 
  * to use same ubuntu image you will have to use following command.
  * command: `docker container start -ai ubuntu`
_________________________________________________

  * command: `docker container exec -it`
  * what is I want to see the shell inside a running container that's already running mysql or nginx. we can use above command.
  * Run additional process in running container. 
_________________________________________________

  * Alpine is another small security focused distribution of linux. It is only 5 meg in size
  * Alpine is so small it does not have bash in it, but it does have sh
  * we can install package manager of Alpine which 'apk' if you want to install bash

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

