### Persistent Data: Bind Mounting

#### Bind mount
  * it is just a mapping of the host files, or directories, into a container file or directory
  * in the background it's basically just having two locations point to the same file
  * it skips UFS like other volumes do so that it's not going to wipe out your host location when you delete the container
  * you can't use them in Dockerfile, must be at container run
  * cmd: `docker container run -v /Users/np/stuff:/path/container` (mac/linux)
  * cmd: `docker container run -v //c/Users/np/stuff:/path/container` (windows)
  * the way docker tells different between named volume and bind mount is that, bind mount starts with a forward slash (/)

#### Trying bind mount

  * in 48_PersistentData-BindMounting folder there are two files i.e. Dockerfile and index.html
  * the Dockerfile is configured to run nginx server and load custom designed index.html instead of default nginx webpage
  * one way of doing this is by building the Dockerfile as image and running the container
  * __another way is by using bind mounting__
  * cmd:  `docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx`
  * this command runs a nginx container in background, and it maps path of current directory of host to the directory in container
  * any change made on host will also reflect in container and vice versa

__NOTE: to practice Dockerfile and index.html file in this can be used__