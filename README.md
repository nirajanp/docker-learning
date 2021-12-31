# Docker Reference Guide

## Creating and Using Containers

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




