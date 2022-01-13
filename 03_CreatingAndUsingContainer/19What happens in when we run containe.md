### What happens in when we run container

  * when we run `docker container run` in the background it is going to look for the image that we specified at the end of the command locally in the image cache
  * if nothing is found locally, then it looks remote image repository(defaults to Docker Hub)
  * by default it will look there, download it, and store it in the image cache (nginx: latest by default)
  * creates a new container based on that image and prepares to start
  * it is going to customize the networking, it is going to give it a specific virtual IP address on a private network inside docker image
  * it is going to open up port if we specify in __--publish__, if we did not specify in __--publish__ then it is not going to open any ports
  * since we did the 80:80, that's telling it to take post 80 in the host and forward all that traffic to port 80 in the container
  * then that container will start using command specified in the Dockerfile