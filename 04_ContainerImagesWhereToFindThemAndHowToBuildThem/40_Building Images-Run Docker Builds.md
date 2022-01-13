### Building Images: Running Docker Builds

  * go to the directory where there is Dockerfile, this file has instruction on how to build our image

  * FROM cmd: when i build this image it is gonna pull the 'debian:stretch-slim' image in from docker hub down to my local cache and execute line by line each of those stanza(command) inside my docker engine and cache each of those layers
  
  * below command builds Dockerfile in the same directory with the name customnginx specified by using -t
  * cmd: `docker image build -t customnginx .`

  * since Dockerfile runs from top to bottom, if any change is made then it will run from where the change is made. thus it is recommended to have lines that change more at the bottom of the Dockerfile and line that changes less at the beginning.