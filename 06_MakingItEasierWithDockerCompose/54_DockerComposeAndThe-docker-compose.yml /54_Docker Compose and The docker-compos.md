### Docker Compose and The docker-compose.yml File

#### Docker Compose
  * __why: configure relationships between containers__
    * few software services are truly stand-alone
    * we are rarely gonna use a single container to solve a problem or provide a service to our customers
    * our container often requires other containers such as sql, key-value and other application that will need to run a container like proxies, web frontend, backend workers, and so on.
  * __Why: save our docker container run settings in easy-to-read file__
    * what if we had a way to connect all those pieces of our solution together, where we didn't need to remember all of our docker run options, and get them into discreet, virtual networks with relationships between them...
  * __why: create one-liner developer environment startups__
    * ...and only expose the public ports, and then spin them all up and tear them down with one command
  #### That's Docker Compose

#### Docker Compose comprised of 2 separate but related things

  1. YAML-formatted file that describes our solution options for:
    this file is where you would specify all the containers you need to run, any volumes you might need, environment variables, images, and all sorts of other configuration
      * containers
      * networks
      * volumes
  
  2. Second part of Docker Compose is CLI tool __docker-compose__, which is used usually for local dev and test using that YAML file we created to simplify our Docker commands

#### docker-compose.yml

  * __compose YAML format has it's own versions: 1,2,2.1,3,3.1__
    * there is actually versions to the things that you can put in the compose file. 
    * when docker-compose was created years ago, it was called 'Fig' and it was just assumed version 1. it did not list a version in the file
    * as it matured and added new features to what the file can have in it, such as networks and volumes, they have created the version statement, which is first line in the file
  * __YAML file can be used with docker-compose command for local docker automation__
    * this file can be used with a Docker composed CLI. this is mainly for local development management and is just making it easier to get around in your local machine
  * __with docker drectly in production with Swarm (as of v 1.13)__
    * starting with the beginning of 2017, we have version 1.13 and anything beyond that, these files can now be used directly with the docker command line in production with swarm
  * __docker-compose --help__
    * docker-compose help command
  * __docker-compose.yml__ is a default filename, but any can be used with __docker-compose -f__

#### docker-compose.yml composition
  * first line of the file is always version.
  * if no version is not mentioned than it assumes version 1 which is not recommended because version 1 does not have lot of features which later versions have
  * other main sections are:
    * service
      * the services are really just containers
      * they are called services because each container that you create in here, you could actually have multiple ones of those containers for redundancy. so they needed to come up with a different word.
    * volumes
    * networks