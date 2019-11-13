# node-hello

Use these three files to try your first Docker container.
* `Dockerfile` defines the tasks for building the image.  It includes a step for installing NodeJS and running npm to install modules.
* `pacakge.json` defines the NodeJS modules to install (by npm).  Express is the only module used in this demo.
* `index.js` is the application code.

#### Step 1:  Get a Docker Machine running
Docker has great [documentation](http://docs.docker.com/engine/installation/) for installation, examples and understanding the basics.  On OSX or Windows a Docker Host runs as a VM in [Virtual Box](https://www.virtualbox.org).  Don't worry, the Docker install includes Virtual Box.

If you are impatient and want to run the container, you can skip to Step 5.  It will pull down the image from [Docker Hub](https://hub.docker.com/r/scetes/ubuntu-node-hello/)..  

#### Step 2: Get the sample code
Follow Docker instructions for your OS to start your Docker host and open a terminal window.

Clone the three files in this GitHub repo to a directory on your local filesystem.

#### Step 3: Build the Docker image
Change to the directory, and build your Docker image with this command (noting that `scetes/ubuntu-node-hello` is the image name I used, but you can change that to your own name):

`$ docker build —t scetes/ubuntu-node-hello .`

if all goes well, the terminal output will start with something like this:

```
Sending build context to Docker daemon 81.92 kB
Step 0 : FROM ubuntu:latest
 ---> e9ae3c220b23
Step 1 : RUN apt-get update
 ---> Using cache
 ```
 
and will finish with something like this:

```
Step 7 : CMD nodejs /node-hello/index.js
 ---> Running in d19aebfa895f
 ---> 6aca47b8aecc
Removing intermediate container d19aebfa895f
Successfully built 6aca47b8aecc
```

### Step 4: Run the container
Start a Docker container from the image you built in the previous step (noting that you would replace scetes/ubuntu-node-hello with your own image name):

`docker run -t -p 8088:8088 scetes/ubuntu-node-hello`

### Step 5. Use it
When you started the container, it started a NodeJS app.  The app is listening on port 8088 for HTTP requests with one route `/api`.  In a browser, open `http://[host]:[port]/api`, and you should get response `hello from docker`.

The **host** is the IP of your Docker container.  If you're running directly on Linux, then the host IP is the same as the Docker container IP.    If you're running on OSX or Windows, the container runs in a Virtual Box VM, so you need to find the IP of the Virtual Box VM.  One way to find it is to run this cmd in your Docker CLI: 

`$ docker-machine ls`
 which will show something like this:
 <pre>
 NAME            ACTIVE   DRIVER       STATE     URL                         SWARM
default         *        virtualbox   Running   tcp://<b>192.168.99.100</b>:2376   
defaultwproxy            virtualbox   Stopped                               
</pre>

The **port** is determined by the port mapping as defined by the -p option in the docker run command.  In this example:

`docker run -t -p 8088:8088 scetes/ubuntu-node-hello`

external port 8088 is mapped to internal port 8088 by the `-p` option.  Note, the internal port is defined in the [index.js](https://github.com/scetes/node-hello/blob/master/index.js) file.

Putting it all together using values from my OSX Docker machine:

`http://192.168.99.100:8088/api`

If you wanted to map to an external port of 80:

`docker run -t -p 80:8088 scetes/ubuntu-node-hello`

will expose the app on port 80.

`http://192.168.99.100/api`

Congrats!  You have your first Docker container running.
