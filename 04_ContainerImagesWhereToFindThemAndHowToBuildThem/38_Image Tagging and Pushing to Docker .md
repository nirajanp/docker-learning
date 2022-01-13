### Image Tagging and Pushing to Docker Hub

  * All about image tags
    * There is lot to tag because they're not as intuitive as you might think when you first start using them
  
  * How to upload to Docker Hub
    * They need to be in a specific format in order to work with a registry
  
  * Image ID vs Tag
    * Image ID is a unique id
    * if multiple tags belong to a image then all tags will have same image ID
    
  * this will add new tag to the existing image. it will have same image ID with name as nirajanp/nginx in repositories
    * cmd: `docker image tag  nginx nirajanp/nginx`
      `docker image tag <source_image:[tag]> <target_image:[tag]>`
  
  * to upload this into Docker Hub you need to login from CLI
    * cmd: `docker login`
  
  * to logout
    * cmd: `docker logout`
  
  * to push image in Docker Hub
    * cmd: `docker image push nirajanp/nginx`
  
  * to give a tag name use this command
    * cmd: `docker image tag nirajanp/nginx nirajanp/nginx:testing`