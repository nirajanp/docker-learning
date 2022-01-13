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