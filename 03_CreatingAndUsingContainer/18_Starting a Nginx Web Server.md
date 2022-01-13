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