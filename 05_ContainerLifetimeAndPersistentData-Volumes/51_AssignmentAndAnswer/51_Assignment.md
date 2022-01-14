### Assignment

* Use a Jekyll "Static Site Generator" to start a local server
* this is example of bridging the gap between local file access and apps running in containers
* the same folder in the repo also has source code
* edit files with editor on our host using native tools
* container detects changes with host files and updates web server

#### Answer
* download and cd to bindmount-sample-1 in the repo
* cmd: `docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve`
* this command  forwards port 80 in host to port 4000 in the container, then mount current directory into the container into the site directory and runs a container image 'bretfisher/jekyll-serve'
* Now any changes made in the host will reflect in the container and in the website
