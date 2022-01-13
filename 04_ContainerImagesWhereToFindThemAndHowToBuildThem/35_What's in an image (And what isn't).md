### What's in an image (And what isn't)

  * App binaries and dependencies for you app
  * Metadata about the image data and how to run the image

  * Inside a image there is not a complete OS. No kernel, kernel modules(e.g. drivers), it is just the binaries that your application needs because host provides the kernel
  * this is one the distinct characteristics around container that makes it different from virtual machine, it that it is not booting up a full OS it is really just starting an application.