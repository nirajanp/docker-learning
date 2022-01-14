### Container Lifetime & Persistent Data

  * Containers are usually immutable and ephemeral (i.e. unchanging, temporary)
  * the idea here is we can just throw away a container and create a new one with a image
  * "immutable infrastructure": only re-deploy containers, never change
  * this is the ideal scenario, but what about databases, or unique data?
    * ideally the container should not contain your unique data with application binaries, this is known as separation of concerns. 
  * docker gives us features to ensure these "separation of concerns"
  * the problem of unique data is known as "persistent data"
  * in new world of container and application scaling persistent data causes a unique problem
    * Docker has two solutions to this problem
      1. Volumes or Data Volumes
      2. Bind Mounts
 
 #### Volumes 
 Docker volumes are configuration options for containers that creates a special location outside of that container Union File System(UFS) to store unique data. this preserves it across container removals and allows us to attach it to whatever container we want

 #### Bind Mounts 
 which are simply us sharing or mounting a host directory, or file, into a container. it will just look like a local file path, or a directory path, to the container, it won't actually know it is coming from the host