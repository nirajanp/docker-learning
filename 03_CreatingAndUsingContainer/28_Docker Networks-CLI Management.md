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