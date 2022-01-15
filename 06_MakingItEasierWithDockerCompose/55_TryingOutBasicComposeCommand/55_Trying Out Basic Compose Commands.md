### Trying Out Basic Compose Commands

#### docker-compose CLI

  * docker-compose CLI is actually separate from the Docker tool. It is a separate binary. If you are on Docker for Windows or Docker for Mac, it actually comes bundled with that.
  * if you use toolbox on Windows 7 it bundles with that.
  * on Linux you will have to download it separately and you can get that at GitHub.com/docker/compose
  * it is not designed to be a production-grade tool but ideal for local development and test
  * two most common command are
    * cmd: `docker-compose up` -> setup volumes/networks and start all containers
    * cmd: `docker-compose down` -> stop all containers and remove cont/vol/net
  * if you have a project where you had the typical new developer onboarding process that was all about learning to get the right tools for their system, and the right virtual machines downloaded, and the right add-ons, and all the things that normally they would just have to build a replicated environment for, with __docker-compose__, its as easy as cloning the environment that has your compose file from some repo somewhere and then running the __docker-compose up__ command

#### In compose-sample-2 dir
  * inside compose-sample-2 directory there are two files __docker-compose.yml__ and __nginx.conf__ files
  * __docker-compose.yml__ file is has two containers and __nginx.conf__ has some configuration
  * so in __compose-sample-2__, when i type __docker-compose up__ command, 
    * it will start both containers
    * it should create a private network for the two of them
    * it will automatically bind mount that file
    * it will open up the port, and it will start dumping logs out to my screen
  * in the __docker-compose.yml__ network or volume is not specified because it is not required. it is only necessary if I need to do some custom things in the network, like maybe change the default IP addresses, or change the network drive that you use.
  * or in the volumes cases, named volume could be created, or use a different volume driver

  * cmd: `docker-compose up -d`
  * this will run yml file in the background. so the server will still keep on running on the background
________________________________________________________

  * cmd: `docker-compose logs`
  * to view the logs 
________________________________________________________

  * cmd: `docker-compose --help`
  * displays all the command you could run with docker-compose
________________________________________________________

  * cmd: `docker-compose down`
  * this stops and cleans up stuffs that i started