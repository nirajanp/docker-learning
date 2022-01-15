### Adding Image Building to Compose Files

#### Using Compose to Build

  * Compose can build your custom images at runtime
  * It will look for in cache for images, if image has build options then the it will build that image when __docker-compose up__ command is used
  * __docker-compose up__ won't build the image every single time, it will only build if it does not find it
  * you could also use __docker-compose build__ to build the image

#### In compose-sample-3 dir

  * so when i run the __docker-compose up__ cmd in this directory it is gonna
    * it is going to look up build commands inside docker-compose.yml file 
    * build image which is in the nginx.Dockerfile
    * in nginx.Dockerfile it is going to use nginx:1.13 image and copy nginx.conf file to specified directory
    
