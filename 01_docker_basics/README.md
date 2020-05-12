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
- Once installed go to [this](https://docs.docker.com/engine/install/linux-postinstall/) post-installation guide to manage docker as non-root user. You might see something similar on running `docker run hello-world` as non root user after following the post-install instructions.

![](https://i.imgur.com/3cOzZTS.png)
---

# 1.3 Using Docker

- Now we have docker installed on our system let's understand what Docker does. The **Docker Flow** is the fundamental concept. In Docker everything begins with an image. An image is every file that makes up just enough of the OS (minimalistic) to do what we want to do. Traditionally devs installed a whole operating system, with everything for each application that they needed to do. With docker we pair it way down so that we have a little container with just enough of the OS to do what you need to do and we can have lots and lots of these efficiently on a computer.

- To look into your docker images, any guesses what the command might be? Duh!! It's `docker images` no rocket science! It isn't as hard as you thought it would be so far, right :D. I hope so. On running `docker images` we can see from where the image came in form of Repository, there is a tag, which is basically a version number, IMAGE ID is the internal representation of this image. So, one could always refer to a particular image by combination of its name and tag hello-world:latest for example in any command.

- We get the following output on running **docker images**:

```bash
(base)  cosmic@fsociety ~ $ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              1d622ef86b13        2 weeks ago         73.9MB
hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
```

- Or we can refer to it by its number. It is better this way, as some images don't have a name as it is not necessary. `docker run` command takes an image and turns it into a living running container, with a process in it that's doing something. Let's look at running an image to make a container.

- `docker run -ti ubuntu:latest bash` **ti** stands for terminal interactive, it causes the container shell to have a full terminal within the image so that we can run that shell and get things like tab completion and formatting and things like that. When you are running commands by typing on a keyboard inside an image ti is a very useful flag to put in, after that we specify which image we would like to run by passing `ubuntu:latest` after that we run the bash shell in image.

- `cat /etc/lsb-release` inside our image gives us the following output:

```bash
root@ea5ced9babe4:/# cat /etc/lsb-release 
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=20.04
DISTRIB_CODENAME=focal
DISTRIB_DESCRIPTION="Ubuntu 20.04 LTS"
```
- Type `exit` to exit or just hit Ctrl+D.

- `docker ps` : To see our running images, we get the following output:

```bash
(base)  cosmic@fsociety > ~ > docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ea5ced9babe4        ubuntu:latest       "bash"              3 minutes ago       Up 3 minutes                            condescending_lamport
```

- `docker ps` gives us a container ID, and do not confuse this with image ID, these aren't the same things. Images have one set of IDs, containers have their own IDs, these IDs do not overlap and there is no place in docker where you can interchange the image and container IDs. It tells us the image container was created from, command it is running in it, duration since created, Status fo the container and name of the container. As we didn't specify the name when we started this container, docker made a creative name for us. It's similar to what GitHub does with random repo names. Now that's the what's what behind running a container.

- Now this is part is **important crux** as it is what makes Docker different from Virtual machines or running things on a real computer. When we are inside a container we start from an image and that image is fixed, it doesn't change. When we make a container from an image, we do not change the image, so in our container that's running which we made from Ubuntu Image we will create a file, we are at root of file system and we will make a file. `touch TEST_FILE`. Now if we open terminal or a new terminal tab and run the same container again using `docker run -ti ubuntu:latest bash` and if we do a **ls**  we won't be seeing TEST_FILE in container created from same image, if we exit the container one where we created the file and start it up again, we will see file is not there.

- Files go into containers but that does not put them back into the image that the container came from, files stay in container as long as it is running.

# Containers to images

- We went from images to a running container, on running the container again, we got the same thing as we got on running it on first time. That's the whole point of images in Docker, i.e. they are fixed points where you know everything's good and you can always start from there. Now when we have a running container, we make changes to that container, we put files there, it is very useful when we want to be able to actually save those files.

- Next step, in docker flow is a stopped container.
![](https://i.imgur.com/HJcrvXR.png)

- So the container that is running is alive and has a process in it, when that process exits, the container is still there. So the file we created TEST_FILE, when we exited the container that file is still there, we can go back and find it, it didn't get deleted, it is just that currently that file is in a stopped container and on doing docker run... you start a new container. We can look at most recently exited container using `docker ps` command. Though docker ps shows running containers and stopped containers are not shown by default, so to show the stopped containers we can specify the **-a** argument to see all containers. `docker ps -l` to see last exited container.

- Doing `docker ps -l` you can see that you will get ID, IMAGE, COMMAND and other stuff about the container that was last exited. Looking at container exit codes can be a good clue to figure out why a container died, while you expected that container to be running and you find it to be stopped for some reason. As our containers right now do not have any networking going on that is the reason why we do not have anything under the PORTS section.

- So, now we have a stopped container that has a file in it, which we want to use for the future, next step is `docker commit` command, what it does is takes the containers and makes images out of those images. It doesn't deletes the container, container is still there, now we have an image with the same content which was inside that container.

- So, `docker run` and `docker commit` are complementary to each other. Docker run takes images to containers, and docker commit takes containers back to images. Docker commit doesn't overwrites the image from which container was made. Now we can make a new image.

![](https://i.imgur.com/44pfmE7.png)

- We create a dummy file using `touch DUMMY_IMPORTANT_FILE`, exit the container using Ctrl+D or exit. See the most recently exited container using `docker ps -l`. We get the following :

```bash
CONTAINER ID        IMAGE               COMMAND       CREATED             STATUS        PORTS               NAMES

cfa719603e99        ubuntu:latest       "bash"   31 minutes ago      Exited (0) 3 seconds ago        funny_ishizaka
```

- Next we take the container ID and do a `docker commit cfa719603e99`. Now, we have a new image, and origin ubuntu image is unchanged. Last step in docker flow is tag command to give images name. For that we grab the sha256 image ID. and pass it to docker tag command. `docker tag < sha 256> my-image`.

- Now on doing `docker images` we see that my-image is listed in list of images.

```bash
(base)  cosmic@fsociety~ $ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
my-image            latest              e86e6e296cea        About a minute ago   73.9MB
ubuntu              latest              1d622ef86b13        2 weeks ago          73.9MB
hello-world         latest              bf756fb1ae65        4 months ago         13.3kB
```

- You can verify if the created file is still there on that image by running `docker run -ti my-image bash` >> `ls`

- Committing images and then tagging them is such a common pattern that actually it is built right into the docker commit command. We can skip the steps about copying image name over to a `docker tag` command and just run `docker commit < name_of_container> < name you want to be tagged as>` e.g. would be `docker commit optimistic_jepsen my-image-2`.

- That's all there is to **The Docker Flow.**

# Running things in Docker

- Now that we know docker flow, let's talk about running things in Docker. Let's start off with what we have been using a lot: `docker run`.

- Docker run starts a container by giving an image name and a process to run inside that container. This is the main process to the container. When main process exits the container is done. Container keeps running till the main process is running. If we start other processes in the container later, which we will cover in that case also container still exits when that process finishes.

- Docker containers have **one main process.** Containers can have names. If you don't give a name to container it will make a random one up on its own. If you just want to run a container but you don't want to keep the container afterwards we run it by passing --rm argument. It tells docker to delete container upon exit. Otherwise you do docker run >> exit >> docker rm < container name>

`docker run --rm -ti ubuntu sleep 5`: This will start a container from ubuntu image it will be on for 5 seconds and then exit. See the idea there? We start a process, container existed for the duration of that process, and then poof...vanishes!

- Let's run a fancy container that starts from ubuntu image and starts the bash shell with a few programs to run.
`docker run --rm --ti ubuntu bash -c "sleep 3; echo all done"`: Here we have one process bash which starts. The bash process starts the sleep command, when sleep is finished it runs the echo. This is very common when we want to do one thing in container and do another thing after that.

- **Leaving Things Running in a Container:** Docker just like tmux (if you have used it) has the idea of detached containers, what this means is you can start a container running and just let it go. That's accomplished by doing `docker run -d -ti ubuntu bash`, it will run it and put it in the background. You can run the `docker ps` command to see the container that is running. `-d` starts the container detached. You can attach to a container by doing `docker attach < container name>`

- If you start a container attached and you want to jump away from it, there's a special sequence to type in that case, and it is `Ctrl+P,Ctrl+Q`, it will exit us out of the container by detaching us from it, but leaves the container running in the background.

- **Running more things in a container:** We started a container, and we want to add another process to a running container, this is really good for debugging, `docker exec`, it won't allow you to add extra fancy stuff with this, you can just start a process and aren't allowed to add more ports or volumes using this method or any stuff you do with docker run.

- To execute a process in a container using docker exec we do `docker exec -ti < container name> bash`. If you exit by Ctrl+D in any of the container the attached one also dies and entire container stops.

# Managing Containers

- Looking at the container output of a container that is already finished can be really frustrating. You start your container it didn't work, you want to know what went wrong in that case `docker logs` command is the way to go. It keeps the output of the container as long as the container is around. You can use `docker  logs container_name` to look at what the output was.

- `docker run --name example -d ubuntu bash -c "lose /etc/password"`: We run a detached container by the name example, we run ubuntu image and we give it a process bash and we want it to run a command line. So we have a terminal running looking at password file. We want to have less but we have typed lose and it will not work. So later on when we just want to see what was the output on detached container we can do `docker logs example`, now as we see error we can go and fix it. Don't let the output of your docker containers get really really large. This is one of the side effects of convenience. It is very convenient to go back and look at it but if you are writing tons and tons of data to the output of the process in your docker container, you can really push docker to the point that your whole system becomes unresponsive.

- **Stopping and removing containers**: You can kill a running container and on doing this it goes to stop state. And when your work is done with container you can do `docker rm container_name` to remove and `docker kill container_name` to stop a container.

- When we are using containers with fixed names and after killing a container you try to create a new container with same name you will get an error that container already exists, it is because you didn't remove the container only stopped it. Run `docker rm container_name` to remove the container. Stopped containers still exist until explicitly removed

- **Resource Constraints**: One of the big features of Docker is the ability to enforce limits on how many resources a container is going to use. We can limit it to a fixed amount of memory. We can do `docker run --memory maximum-allowed-memory image-name command` We can also limit CPU time by doing `docker run --cpu-shares relative to other containers` or we can also set **hard limits** by doing `docker run --cpu-quota` to limit it in general.

- **Some notes from past experiences:**
1. Don't let your containers fetch dependencies when they start, if you are using things like Node.js and you have your node starts up and then when container starts it fetches its dependencies, then some day, someone will remove some library out from node repos and all of a sudden all your containers just stop, throughout your whole system, and that's awful. Fetch make your containers include their dependencies inside the container themselves, saves a lot of pain in the longer run.
2. Security tip: Don't leave important things in unnamed stopped containers. Don't do a week's worth of work and just leave it sitting in a stopped container on your laptop. As you will reach the point where you go, oh snap, my disk is full and you will be like alright let's clean some stopped containers and guess what, that week's work, you can kiss goodbye to it.