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