### Images and Their Layers: Discover Image Cache

  * Images are made up of file system changes and metadata
  * each layer is uniquely identified as a SHA, and only stored once on a host
  * this storage space on host and transfer time on push/pull
  * When you start a container it is just a single layer of changes on top of image
  * `docker history:mysql` and `docker image inspect nginx`, teach us what's going on inside a image and how it was made