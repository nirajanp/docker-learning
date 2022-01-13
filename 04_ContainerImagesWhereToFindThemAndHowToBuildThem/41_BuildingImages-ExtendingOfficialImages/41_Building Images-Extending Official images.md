### Building Images: Extending Official Images

#### Write a Dockerfile to change the default nginx page and change to custom HTML

  1. We want the latest nginx image so in Dockerfile we add this
    __FROM nginx:latest__ 
  2. this WORKDIR is really running cd: change directory. here we are changing to a default nginx directory for its html files
    __WORKDIR /usr/share/nginx/html__
  3. this is the stanza that you would use to copy your source code from your local machine, or your build servers, into your container images
    * in this case we are overriding the index.html file of nginx in its home with custom defined html for 
  __COPY__ index.html index.html

### Build Your own Dockerfile

  1. make changes in your Dockerfile
  2. build the Dockerfile. this is gonna build the docker file with image name testnode in current directory
    * cmd: `docker build -t testnode .`
  3. then run the container 
    * cmd: `docker container run --rm -p 80:3000 testnode` locally 
  
  * Now to push this to Docker Hub, you need to change its name first
    1. cmd: `docker image tag testnode nirajanp/testnode`
    2. push to Docker Hub `docker push nirajanp/testnode`
    3. Now if you remove nirajanp/testnode from local environment, and try to run this container, docker will get that image from Docker Hub Registry and run it locally
    4. cmd: `docker container run --rm -p 80:3000 nirajanp/test-app`