### Container vs VMs

#### Containers aren't Mini-VM's

  * They are just processes
  * Limited to what resource they can access
  * Exit when process stops
_________________________________________________

  * command: `docker run --name mongo -d mongo`
  * this will start a mongo database with name "mongo" using mongo image and will run in the background
_________________________________________________

  * command: `docker top mongo`
  * this will let me list the processes that are running inside a specific container
_________________________________________________

  * command: `ps aux | grep mongo`
  * this command will show process with name "mongo" running.
  * in macOS by default you won't be able to list the docker processes running in the host because Docker in Mac is actually running a tiny Linux VM in the background. So in order to run `ps aux` you will need to run it from inside tiny Linux VM.
  * This site has information how to run tiny Linux VM in your mac [Getting into Local Docker VM](https://www.bretfisher.com/docker-for-mac-commands-for-getting-into-local-docker-vm/)
_________________________________________________

  * command: `docker image ls` 
  * list the images 