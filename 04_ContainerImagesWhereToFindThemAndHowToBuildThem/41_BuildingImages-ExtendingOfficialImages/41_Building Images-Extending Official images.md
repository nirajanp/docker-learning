### Building Images: Extending Official Images

#### Write a Dockerfile to change the default nginx page and change to custom HTML

  1. We want the latest nginx image so in Dockerfile we add this
    __FROM nginx:latest__ 
  2. this WORKDIR is really running cd: change directory. here we are changing to a default nginx directory for its html files
    __WORKDIR /usr/share/nginx/html__
  3. this is the stanza that you would use to copy your source code from your local machine, or your build servers, into your container images
    * in this case we are overriding the index.html file of nginx in its home with custom defined html for 
  __COPY__ index.html index.html
