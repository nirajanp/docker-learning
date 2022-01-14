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

