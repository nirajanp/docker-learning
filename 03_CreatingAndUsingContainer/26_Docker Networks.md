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