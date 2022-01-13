### The Mighty Hub: Using Docker Hub Registry Images

  * Docker hub has many different images such as nginx, mysql, etc
  [Docker Hub Registry](hub.docker.com)
  * typically use official images
  * official images have just the name without forward slash

#### To pull docker images
  * this will download most latest image from docker hub registry
    * cmd: `docker pull nginx` 

  * this will download image with version specified.
    * alt: `docker pull nginx:1.21.5`