# 1.1 What is this section about?

- We will see how to get started using Docker. We will look at Docker flow and how can one work with containers. We will also build some docker images of our own. We will look at networking and in-depth parts of how docker works.

# What should you know before going to Docker ?
- Exposure to a CLI is expected, basic networking knowledge is expected, bash and linux shell scripting experience will be helpful.

# What is Docker?
- Docker fires up a running Linux system into small containers each of which is it's own tightly sealed little world perfect to an app's requirements. Keyword here being **tightly sealed and isolated** from everything else. These containers are designed to be portable so that they can be shipped from one place to another in an attempt to overcome the problem faced by developers from a long time known as **Works on my machine!** Docker does this work of getting these systems to and from your system. Docker also builds these containers for us as well as it is a social platform to help others and find containers with others who may have already built containers for very similar work that you can leverage to your advantage and build on top of.

Let's settle this once and for all: **Docker is NOT VIRTUAL MACHINES.** There is only a single operating system running and that OS is carved up into tiny isolated little spaces.

### What is a container?
- A container is a self-contained sealed unit of software. It has everything in it that is needed to run that code. A container includes **all of the code, config, processes, dependencies needed, part of the OS needed to run the code and networking enabled** to allow one container to talk with other container they are allowed to and nothing else.
![](https://i.imgur.com/Q1lMWXe.png)

- The way docker works is it takes all of the services that make up a linux server, networking, storage, code, IPC (Inter-Process Communication), the entire stuff and makes a copy of that in the Linux Kernel for each container, allowing each container to have its own little isolated world that it can't see out of, and other containers can't see into. As shown below, one can have a container on a system running Red Hat Linux serving a database through a Virtual Network to another container running Ubuntu Linux which is running a web server that talks to that database, and that Ubuntu web server might also be talking to a caching server that runs in a SUSE Linux based container.

- Important part to understand is it doesn't matter which linux each container is running on, it just has to be **a linux.** Docker is the program which manages all of this. Docker is the one which sets it up, monitors it and tears it down properly once work is done.

- Docker is a client program, it is a command you type in the terminal,it is also a server program that listens for messages from the docker command and manages a running Linux System. Docker has a program which builds containers from code. It takes your code, along with dependencies and bundles it up and then seals it into a container. Docker is also a service that distributes these containers across the internet and makes it so that you can find work of other people, and right people can find your work.

![](https://i.imgur.com/r8qhNIw.png)
---

# 1.2 Installing Docker

- Docker's primary job is to manage a linux server and start and stop containers on that server as required. Some people do not work on laptops running linux all the time, so, many people use a VM running on their laptop to act as the Linux Server to run the server side of Docker. 

- Docker provides tools that makes managing this stuff very nearly transparent. To understand this, we will start from your laptop, where you are physically typing, on that physical machine in the terminal we interact with a program named docker, this docker in terminal is the client, that is a connector to a program named Docker that program is the server which controls the Linux VM. 
![](https://i.imgur.com/sSbW9As.png)

- Also on the laptop one might have a Linux VM, being managed by Docker for Windows, Docker for Mac or whatever your platform is. Once it is installed you may click on the whale icon program creates. There isn't much interaction required except when you want to start and stop your containers, it's fairly well automated.
![](https://i.imgur.com/lMLKKSv.png)

- To run docker just type `docker run hello-world` it will fetch the image and run the container.

## Docker Desktop - To Install on Windows and Mac

- There has been several evolutions of Docker Desktop Experience. The oldest being boot2docker, you will have to remove it completely before you follow the further instructions I provide in this README.md. Docker Desktop is better in most regards, so it's good time to get up-to-date. Well you might think why is Docker desktop needed and is available only for Mac and Windows, it is because on Linux Docker runs natively and for Windows and Mac there is this need to create a linux server which will interact with Docker client running on host OS as explained above.

- **NOTE:** If things appear buggy, start breaking or seem just weird, you should try to go to docker desktop context menu and restart the program. Most of the time it fixes about 99% of problems. In extreme cases quit Docker desktop and start again.

# Docker Installation : Linux
- Installing docker on Linux is much more straight forward than other platforms, as it is just another program on Linux. Just do a google search for installing docker on ubuntu and go to the official Docker installation guide which you may find [here](https://docs.docker.com/engine/install/ubuntu/).

- I recommend installing docker-ce as it is much more modern, so follow the command to uninstall previous versions in the link given above.
- Once installed go to [this](https://docs.docker.com/engine/install/linux-postinstall/) post-installation guide to manage docker as non-root user.

