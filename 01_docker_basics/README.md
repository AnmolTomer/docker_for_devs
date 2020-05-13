# 1.1 What is this section about?

- We will see how to get started using Docker. We will look at Docker flow and how can one work with containers. We will also build some docker images of our own. We will look at networking and in-depth parts of how docker works.

### What should you know before going to Docker ?
- Exposure to a CLI is expected, basic networking knowledge is expected, bash and linux shell scripting experience will be helpful.

### What is Docker?
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

### Docker Desktop - To Install on Windows and Mac

- There has been several evolutions of Docker Desktop Experience. The oldest being boot2docker, you will have to remove it completely before you follow the further instructions I provide in this README.md. Docker Desktop is better in most regards, so it's good time to get up-to-date. Well you might think why is Docker desktop needed and is available only for Mac and Windows, it is because on Linux Docker runs natively and for Windows and Mac there is this need to create a linux server which will interact with Docker client running on host OS as explained above.

- **NOTE:** If things appear buggy, start breaking or seem just weird, you should try to go to docker desktop context menu and restart the program. Most of the time it fixes about 99% of problems. In extreme cases quit Docker desktop and start again.

### Docker Installation : Linux
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

### Containers to images

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

### Running things in Docker

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

### Managing Containers

- Looking at the container output of a container that is already finished can be really frustrating. You start your container it didn't work, you want to know what went wrong in that case `docker logs` command is the way to go. It keeps the output of the container as long as the container is around. You can use `docker  logs container_name` to look at what the output was.

- `docker run --name example -d ubuntu bash -c "lose /etc/password"`: We run a detached container by the name example, we run ubuntu image and we give it a process bash and we want it to run a command line. So we have a terminal running looking at password file. We want to have less but we have typed lose and it will not work. So later on when we just want to see what was the output on detached container we can do `docker logs example`, now as we see error we can go and fix it. Don't let the output of your docker containers get really really large. This is one of the side effects of convenience. It is very convenient to go back and look at it but if you are writing tons and tons of data to the output of the process in your docker container, you can really push docker to the point that your whole system becomes unresponsive.

- **Stopping and removing containers**: You can kill a running container and on doing this it goes to stop state. And when your work is done with container you can do `docker rm container_name` to remove and `docker kill container_name` to stop a container.

- When we are using containers with fixed names and after killing a container you try to create a new container with same name you will get an error that container already exists, it is because you didn't remove the container only stopped it. Run `docker rm container_name` to remove the container. Stopped containers still exist until explicitly removed

- **Resource Constraints**: One of the big features of Docker is the ability to enforce limits on how many resources a container is going to use. We can limit it to a fixed amount of memory. We can do `docker run --memory maximum-allowed-memory image-name command` We can also limit CPU time by doing `docker run --cpu-shares relative to other containers` or we can also set **hard limits** by doing `docker run --cpu-quota` to limit it in general.

- **Some notes from past experiences:**
1. Don't let your containers fetch dependencies when they start, if you are using things like Node.js and you have your node starts up and then when container starts it fetches its dependencies, then some day, someone will remove some library out from node repos and all of a sudden all your containers just stop, throughout your whole system, and that's awful. Fetch make your containers include their dependencies inside the container themselves, saves a lot of pain in the longer run.
2. Security tip: Don't leave important things in unnamed stopped containers. Don't do a week's worth of work and just leave it sitting in a stopped container on your laptop. As you will reach the point where you go, oh snap, my disk is full and you will be like alright let's clean some stopped containers and guess what, that week's work, you can kiss goodbye to it.

### Exposing Ports

- Docker is used almost universally, to run network services such as web and database servers. To make this easier, docker offers a wide variety of networking options to connect containers together and to connect containers to the internet. For connecting containers together, docker offers private networks, where we can put each container on a network, and they can talk to each other, but still be isolated from rest of the containers.

- For getting data into and out of the system as a whole, Docker offers the option option of exposing a port or publishing a port, that makes a specific port accessible from outside the machine on Docker which is being hosted. Through the combination of these options one can wire up their networks pretty much in any way they please.

- To expose a particular port, we specify the internal port that the program is listening on, and on what port it should be listening on the outside, also what protocol you'd use. There are many options, we will go over the basic ones here.

- Let's set up a interesting but minimal example to show the flow between several containers. We will start a server, server will accept data from one client, and pass it to another. We will have 2 clients. To start up the server we run `docker run --rm -ti -p 45678:45678 -p 45679:45679 --name echo-server ubuntu bash` we use -p to expose the port on inside to the outside. We give docker container a name specifically . After that on this echo-server container we will start a bare minimum server using netcat utility, it is a good way to show networking without having to get concerned with anything else. `nc -lp 45678 | nc -lp 45679` lp for listen port and using pipe we use the output of that to send it to another program, and this another program will be a copy of netcat program `nc -lp 45679` that means data will get passed into our system on one port.

- Before running net cat make sure to install it by running `apt-get update && apt-get install -y netcat` on your container. After that you can do `nc -lp 45678 | nc -lp 45679`. Now on other terminal connect to that netcat instance we ran before this by doing `nc localhost 45678` and `nc localhost 45679` after this we send some data and we see that data went from the computer to a port exposed on the computer. Got passed into the container on one port, passed between two programs within that container and passed out back to a port running on the host computer. If you have netcat on a docker container but not on your system in that case you can do `docker commit container_name image_tag_name` and you can start that image by doing `docker run --rm -ti image_name bash`, it will start the container which has netcat from the image.

- We start the container with image having netcat but who do we connect to ? Localhost of the container refers to itself. We want to refer to the host computer, if you are running Docker Desktop on a Mac or Windows, this is one of those cases where it is different. Containers are not allowed to directly address the container by IP Address, atleast not reliably, so they have added a special host name for containers to refer to the host machine that is hosting them and that name is `host.docker.internal` so from inside the container we do `nc host.docker.internal 45678` on first docker image and `nc host.docker.internal 45679` in another container.

- If you are on linux just do an `ip add` to get your internal ip address of host and pass it to the docker as you can see below:

![](https://i.imgur.com/idJ9ZC9.png)

- Explicitly setting the external port numbers is very reliable as it leads to us knowing where to find the service always. What if you want 12 containers running ? You can leave off the host side of port definition and docker will fill it in from one of the ports available. This allows many containers running the same programs to share the same host. This is often used with some sort of an orchestration, this is often used with some sort of an orchestration or discovery service like Kubernetes or something similar.

- We run the following command same as we used to run echo server with a slight change that we define only the port seen from inside of the container and we will let the docker choose the port on the outside as per availability we run the following command to start the image having netcat `docker run --rm -ti -p 45678 -p 45679 --name echo-server netcat bash`. Once the container is running we do `nc -lp 45678 | nc -lp 45679`. In other terminal we run `docker port echo-server` to look up the port number assigned on the outside. It gives us the following output:

```bash
cosmic@fsociety:~$ docker port echo-server
45678/tcp -> 0.0.0.0:32769
45679/tcp -> 0.0.0.0:32768
```
- Now we insert the dynamically generated port number we can see the output as shown in the image below:

![](https://i.imgur.com/YfoELy1.png)

- In addition to working with TCP as shown in examples above, we will go over now to demonstrate that docker works fine with UDP and other protocols as well.
- docker run -p outside-port:inside-port /protocol (tcp/udp) can be used. E.g. `docker run -p 1234:1234/udp`

- We run the following `docker run --rm -ti -p 45678 --name echo-server netcat bash` >> Look at the port assigned using `docker port echo-server`, we see that port 32770 is assigned. We start something to listen inside the container by doing `nc -ulp 45678` from terminal we send something by doing `nc -u localhost 32770`.

### Connecting Between Containers

- When we expose a container's port in Docker, Docker creates a network path from, essentially from the outside of that machine down through the networking layers and into that container. That's ideal, and other containers can connect to the container by going out to the host, turning around and coming back in along that path via host into Virtual Network.
![](https://i.imgur.com/HJGrh7R.png)

- This is useful but there are more efficient ways, to go about it. Docker offers a lot of networking options so that you can have all the freedom when deciding how the containers connect to each other and making sure it is secure. Let's check some features of Docker's virtual network.

- To take a look at existing networks and see what is there by default we do `docker network ls` it shows us three networks which are:

```bash
cosmic@fsociety:~$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
6bbc569f6333        bridge              bridge              local
d1f1ddfaab06        host                host                local
d3f3a50e52df        none                null                local
```

- **Bridge**: It is the network used by containers that do not specify a preference to be put into any other network. Host is when you do not want container to have any network isolation at all. This does have some security concerns, and none is when a container should have no networking enabled.

- Let's create a new docker network to play around. Run command `docker network create learning`. This will create a network named learning. Let's put some machines on that network. Run `docker run --rm -ti --net learning --name catserver ubuntu bash`. Names are very useful when using private networks in docker, because different containers inside the network can refer to each other by those names. So, it makes it very easy for containers to find each other. Try `ping catserver` and this will show that we can send traffic to ourselves.

- Before you do that make sure to install ping command `apt-get update && apt-get -y install iputils-ping && apt install -y netcat` Let's start a dogserver in a similar way.

- On doing ping from catserver to dogserver and dogserver to catserver following is the output:

```bash
root@9a9b6cd69cc9:/# ping catserver
PING catserver (172.18.0.2) 56(84) bytes of data.
64 bytes from catserver.learning (172.18.0.2): icmp_seq=1 ttl=64 time=0.128 ms
64 bytes from catserver.learning (172.18.0.2): icmp_seq=2 ttl=64 time=0.111 ms
64 bytes from catserver.learning (172.18.0.2): icmp_seq=3 ttl=64 time=0.048 ms
64 bytes from catserver.learning (172.18.0.2): icmp_seq=4 ttl=64 time=0.113 ms
^C
--- catserver ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3066ms
rtt min/avg/max/mdev = 0.048/0.100/0.128/0.030 ms
--------
root@20fe45aea9ea:/# ping dogserver
PING dogserver (172.18.0.3) 56(84) bytes of data.
64 bytes from dogserver.learning (172.18.0.3): icmp_seq=1 ttl=64 time=0.038 ms
64 bytes from dogserver.learning (172.18.0.3): icmp_seq=2 ttl=64 time=0.053 ms
64 bytes from dogserver.learning (172.18.0.3): icmp_seq=3 ttl=64 time=0.073 ms
64 bytes from dogserver.learning (172.18.0.3): icmp_seq=4 ttl=64 time=0.044 ms
^C
--- dogserver ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3066ms
rtt min/avg/max/mdev = 0.038/0.052/0.073/0.013 ms
```

- We can also do a listen on dogserver by doing `nc -lp 1234`, and we can go to cat server send something to dogserver port 1324 `nc dogserver 1234`. These 2 can communicate bidirectionally. But this is just one network with two machines. Let's make another network by running `docker network create catsonly`. Now we put the catserver on catsonly network by doing `docker network connect catsonly catserver`. Now to make things more interesting, we start another container in a new terminal and do a `docker run --rm -ti --net catsonly --name bobcatserver ubuntu bash`, this way now bobcatserver can ping to catserver as both of them are on catsonly server, catserver can ping to dogserver as well as bobcatserver as it is on learning as well as catsonly network and dogserver can ping only to catserver as those two are on learning network.

![](https://i.imgur.com/IasxE6k.png)

- There are many ways to do what we did above, I just tried to show enough of an example to keep things interesting and to cover about 90% of the use cases.

### Legacy Linking

- In addition to exposing ports and connecting containers together with virtual networks, docker also has a legacy way of linking containers together. It is used in kinda private network scenario, but links are only one way, so we can connect from all ports on one machine to all the ports on the other machine. Bidirectional connections aren't allowed. It gives no information the other way. It gives no information the other way. Also, secrets on the machine being linked, say environment variables with database passwords are visible to any machine that later comes along and decides to link to it. However, reverse is not true, server can't see env variables, on the machine's that choose to link to it. Also this leads to things getting dependent on startup order, which machine is running in order for the other one to connect to it. It makes orchestrating things a lot harder. It is recommended to stick to more modern options, though there are cases where we really want the features this offers. So let's dive into this briefly.

- We create a new container and give it a secret using the foll. command: `docker run --rm -ti -e SECRET=cats_are_yuck --name catserver ubuntu bash`, now we create a dogserver and give it ability to be able to connect to catserver so we create it by typing `docker run --rm -ti --link catserver --name dogserver ubuntu bash` and install netcat as we did above. On catserver we do `nc -lp 1234`, we connect to catserver port 1234 on dogserver by doing `nc catserver 1234` and we send some random text to catserver and it appears as expected, catsever can reply with something as well. Remember **here catserver was listening to port 1234 and dogserver was connected and sending to catserver's 1234 port.**

- Now on dogserver we set up listen on port 1234 of dogserver by `nc -lp 1234` and from catserver we try `nc dogserver 1234` we get an error on catserver as we specified while creating dogserver that dog is linked to catserver but the other way around i.e. connection from catserver to dogserver. So now it is established that links are unidirectional in this case only from dogserver to catserver. As dogserver is linked to catserver the env variable of catserver can be viewed from dogserver and you can check by running `env` on dogserver. Thus, legacy linking operates only in one direction, not the band! There exists another form of linking which is really old, which just sets the IP addresses in environment variables corresponding on both side of the link, this kind of linking is totally unused these days and you should probably avoid it in cases where it still comes up.

### Listing and Deleting Images

- We have used lots of images so far in this course, and now let's go over how to manage and keep track of images we are working with in Docker. To list the images that are already downloaded we do `docker images`, it only lists the images that you have downloaded and isn't the tool to find the images to download. Docker is much more efficient and in cases looking at output of docker images is not the right way to estimate the space docker is taking.

- We have already covered tagging images to give them names, we do not have to manually tag images, when we do docker commit docker will tag the images for us, so we can say docker commit container_ID name. We can do `docker ps -l` to look at most recently stopped container. Doing `docker commit container_ID image_12` will create an image with the name but without any tag, to tag an image you should do `docker commit container_ID image_name:tag_name`.

- Images comes from `docker pull` which is run automatically by doing `docker run`, docker pull is better for offline work when you can just pull some images in advanced.

- Opposite of docker pull is `docker push` we will go over this soon.

- **Cleaning Images:** Images  can accumulate quickly, docker rmi i.e. (remove image) takes the image name and tag and can be used in following manner: `docker rmi image_name:tag` and removes the image from your system, or you can do `docker rmi image_ID`

### Listing Volumes

- Now we go over Sharing Data Between Containers. Docker offers this feature called volumes, volumes are sort of shared folders. These are virtual discs inside which we can store data in and share them between the containers, between containers and host or both. There are 2 main varieties of volumes, (virtual discs available within docker)
we have **persistent** ones we can put data there, and it will be available on the host, and when the container goes away, the data will still be there.

- Another one is **Ephemeral Volumes**, these exist as long as container is using them, but when no container is using them, they vanish, POOF! These volumes will be there as long as these are used and are not permanent. Volumes are not part of images, no part of volumes will be included when you download an image, and no part of volumes will be involved if you upload an image. Volumes are for your local data, local to your particular host.

- First we go over sharing data between the host and the container. These are kind of like shared folders you are used to if you have worked with VM systems before, also let's talk about sharing a file into a container as it is very similar. Let's make a test folder to share with container. To share a folder we do `docker run --rm -ti -v ~/test:/shared-folder ubuntu bash` here first we specify the path of folder we want to share on host i.e. ~/test after that using `:` we specify where to find shared folder in the container in our case it is `/shared-folder`, after volume specification we will put the image name i.e. ubuntu and program to run ,i.e. bash.

- Now using touch we create some files inside /shared-folder and when we exit the container and from host when we check if we have data which we made inside container, it shows us that yeah files which we created inside container/shared-folder are present inside host's ~/test folder. This data survived the container deletion as it was shared with host in a volume. To share a single file just pass the path to the file instead of path to whole folder and it shares a file. Also file must exist before you start the container or it will be assumed to be a directory.

- Sharing Data between Containers: For this we will be using **volumes-from** argument to docker run. These are shared discs that exist only as long as they are being used. When these are shared between containers, these will be common among the containers that are using these discs. Now we can create a volume that is not shared with host by doing `docker run --rm -ti -v /shared-data ubuntu bash`. Now we start another docker container, before that check the name of the container just started and run this `docker run --volumes-from name_of_container_to_be_used ubuntu bash`. Now if we look inside the newly created container's shared-data folder we will see that created file in container one will be visible inside this second container. If we exit the first container and see on second container if data is there inside shared-data folder it will still be there, if we start third container by doing `docker run --rm -ti --volumes-from name_of_second_container ubuntu bash`. So we see that we can have a file originated in some container, inherited by some other container and inherited by some other container, even though machine which created it is gone.

- But important part to understand is when **there is not even a single instance having shared-data volume** in that case data is gone. These can be passed from one container to the next, but these aren't saved and will go away eventually.

### Docker Registries

- We have been using a lot of docker images, without talking about where do these images really come from. Docker images are retrieved through registries and published through registries. Registries are pieces of software. Registries manage and distribute images. You can upload and download images from registries. Docker (the company) makes these registries freely available. You can even run your own registry to make sure data stays safe and private.

- Let's go over finding images to use. You can search straight from the command line by doing docker search. E.g. `docker search tensorflow`, while searching look out for STARS on the docker image, whether it is official or not. If you want a much more GUI based option for doing the same, you can go to [hub.docker.com](https://hub.docker.com).

- You can find a lot more info on using the container on docker hub site. It's good to go over README before starting to work on an image.
- Now to pull and push an image, you can do so in the following manner:
```bash
docker login >> Enter username and password.
docker pull debian:sid
docker tag debian:sid anmoltomer/test-image:v1.0
docker push anmoltomer/test-image:v1.0
```
Voila! With that you have pushed your first docker image.

- **Some things to remember:** 
1. Do not push images having passwords, even if those are some layers down. 
2. Also clean up your images regularly. During the process you will discover the images you really need to keep.
3. Be aware of how much you are trusting the containers you fetch, check if those are official and trustworthy. Remember to **Trust, but verify!**

---

# 1.4 Building Docker Images

### What are Dockerfiles ?

- So far, we made a couple of Docker images by hands and ran things on them to make containers. Now let's explore building images with code. This is where dockerfiles come handy. Dockerfiles are small programs designed to describe how to build a docker image. We run these programs with docker build command. It usually goes like this `docker build -t name-of-result .` -t is for tag, it basically says when it's built tag it with this name so that it is easy to find later on. `.` at the end specifies where you can find the dockerfile . in this case means that file to build from is in the current directory. When build command finishes, the result will be in your local docker registry, ready to run with `docker run` command.

- Each step produces a new image. There are a series of steps, start with a image, make a container out of it, run something in it, make a new image. The previous image is unchanged, it just says start from that image as base, make a new one with some changes in it. The state is not carried forward from line to line, if you start a program on one line, it runs only for the duration of that line, as a result if a part of your build process is download a large file, do something with it and delete it, if you do all that in one line, then the resulting image will only have the result of that. If you specify download on one line, it will get saved into an image, next line will have that image saved there, and space occupied by big downloaded file will be carried all the way through and your dockerfile will get pretty big. Be careful about operations on large files span lines in dockerfiles.

- You can learn all about dockerfiles [here](https://docs.docker.com/engine/reference/builder/)

- So each step of running a dockerfile is cached. As told above, later steps don't modify the previous step, that means that next time you run your build, if nothing is changed, docker doesn't have to run that step. **Docker can skip lines that have not changed since the last time.** So, let's say your first line is do download a big file, and save the latest copy, and then 20 minutes later you run the command again the file will already have been downloaded, so that line won't be run. This can save huge amounts of time. Though if you wanted to re-download the big file again you'd have to explicitly make it do so.

- A little tip: Put parts of your code that you changed the most at the end of your Dockerfile, that way parts before those steps won't be done again every time you change a part. **Dockerfiles are not shell scripts.** They were designed with syntax that looks like shell scripts, because that helps dockerfiles to be familiar to people, and it makes it a little easier to learn, but dockerfiles aren't shell scripts. 

1. Processes you start on one line, won't be running on the next line. You run the lines, processes run for the duration of that container, then the container gets shut down, saved into an image, and you have a fresh start. So you can't treat those like shell scripts where you start program on one line and after that you send message to program in other line. If you need to have one program start, and after that another program start, those two operations need to be on the same line so that they run in same container.
2. Environment variables you set will be set on the next line, **if you use the env command**.

### Building Dockerfiles

- Here we create most basic dockerfile. Ref:**./example/**. First line in a Dockerfile specifies what image to start with, where do we begin, we use busybox, it's a very small image with just a shell in it. So starting from busybox > We create a container and in that container we run "building simple docker image" using echo. We get the following output

```bash
(base)  cosmic@fsociety > ~/Desktop/Link to docker_for_devs/01_docker_basics/example >> master ●>> docker build -t hello .
Sending build context to Docker daemon  2.048kB
Step 1/3 : FROM busybox
latest: Pulling from library/busybox
e2334dd9fee4: Pull complete 
Digest: sha256:a8cf7fzzadwdc2afa2a90acd081b484cbded349a704a87bdf37a05279f276bc12
Status: Downloaded newer image for busybox:latest
 ---> be58884596ada # Downloaded the image
Step 2/3 : RUN echo "building simple docker image."
 ---> Running in 5aa45g34c5e94 # Started from container above, created a container and ran command above
building simple docker image.
Removing intermediate container 5aa45g34c5e94 # Removes the container which was intermediate, as no one will use this because it is not referenced elsewhere in Dockerfile
 ---> 947e002eb3ea 
Step 3/3 : CMD echo "hello container."
 ---> Running in a56f88cbfa33
Removing intermediate container a56f88cbfa33 # Remove intermediate container
 ---> 65506a7c3bb5 # Final commit
Successfully built 65506a7c3bb5
Successfully tagged hello:latest # Final result
```
- Now to run the Dockerfile built above we do `docker run --rm hello` and you should see `hello container.` on your terminal.

- Now let's make a slightly more interesting image. Ref:**example2/Dockerfile**

- To run that image we do `docker run --rm -ti debian_notes`

- Now we try to make notes file even more interesting by doing, start with notes file, with some content already in the file, we will start this Dockerfile where example2 left off. Ref:**example3/Dockerfile**.Here we already have a notes.txt file and we try to open this inside the new container we make from notes_debian image. We build this by using `docker build -t notes_v2 .`. We run it using `docker --rm -ti notes_v2`. You will see that inside the container txt file will open in the same state we made changes from host.

### Dockerfile Syntax

- **FROM Statement:** Used to define the image to download and start from. This must always be the first command in Dockerfile. It's fine to put multiple FROM in a Dockerfile, it means that Dockerfile produces more than one image. Might look like `FROM JAVA:8`

- **MAINTAINER STATEMENT:** This is a bit of a documentation that defines who is responsible and to be contacted for this particular image. e.g. `MAINTAINER Firstname Lastname <email@domain.com>`

- **RUN STATEMENT:** Used to say to RUN a command through the shell, wait for it to finish and save the result. `RUN unzip install.zip /opt/install/` `RUN echo hello docker`

- **ADD** Statement: Adds local files. `ADD run.sh /run.sh` can be used for compressed files to add to container using `ADD project.tar.gz /install/` it notices that it is a compressed archive, and it uncompresses all the files in that archive to the directory specified. Works with URLs as well. Works with URLs as well. Pass in a URL and it will download and put the thing in the folder directed. E.g. `ADD https://project.example.com/download/1.0/project.rpm /project/` will put project.rpm in /project/.

- **ENV** Statement: Sets environment variable, both during duration of Dockerfile, so while image is building and those environment variables will still be set in resulting image. For e.g. if you say `ENV DB_HOST=db.production.example.com` `ENV DB_PORT=5432`

- **ENTRYPOINT AND CMD** Statements: We use the CMD statement previously in the examples. ENTRYPOINT is much like CMD, but it specifies the beginning of the expression to use when starting our container. If container has an entry point of ls, then anything we type after saying `docker run my_image name` would be treated as arguments to the ls command.

- **CMD**: Specified the whole command to run. If person while running the container types something after docker run image name that will be ran instead of command. If you have both ENTRYPOINT and CMD they get combined together. If you want to make something like a program and you do not want people to care that it is running inside a docker container, ENTRYPOINT is to make your containers look like normal programs. If you are unsure use CMD.

- **RUN**: This can take commands to run in two different forms: **Shell** form and **Exec** form. Shell form is what you type into a shell i.e. `nano notes.txt` and this will run inside a shell, exec form is `["/bin/nano", "notes.txt"]` This causes nano to run directly, not surrounded by a call to a shell such as bash. So exec is slightly more efficient. You may use whichever form looks better to you.

- **EXPOSE**: Maps ports into a container, does the same thing as -p 1234:1234.

- **Volume**: Defines shared or ephemeral volumes, depending upon whether we give one or two arguments. If you have 2 arguments it mapps a host path, into a container path such as `VOLUME ["/host/path/", "/container/path/"]` , if you want ephemeral volume that can be inherited by later containers you run `VOLUME ["/shared-data"]`. You should generally avoid using shared folders in Dockerfile, as it means that this Dockerfile will only work on your computer and you will probably want to share it around some-day, or atleast run it on a different computer.

- **WORKDIR**: Sets the directory for the remainder of the Dockerfile, and for resulting container when we run it. It is like typing CD at the beginning of the every run expression after that. So, it's a useful expression to know about.  If you say `WORKDIR /install/` then rest of the statements will happen inside install directory.

- **USER**: Says that we want to run the commands as some specific user, say `USER 1000` or `USER cosmic`. This can be useful if you have shared network-directories involved that assume a fixed username or a fixed user number.

### Multi-project Docker files

- When we build docker files that go beyond the very simplest projects, it often runs in two competing desires, on one hand we want Dockerfile to be totally complete, it should have everything required to build this project from scratch, so that years from now we will be absolutely certain, that we can rebuilt it, and that it will have everything. On the other hand we want Dockerfiles to be super small, minimal and really quick to deploy, so people often do things like make two Dockerfile(s) one for the daily life and one for production. These get out of sync and stress follows. Recently a new feature was added to Dockerfile(s) called multi-stage build.

- To show how this works let's start by making a small Dockerfile. Ref:**multi_stage_build/**. We build the dockerfile by doing `docker build -t too_big .` Now when we see size of too_big image it shows that it is like 111 MB and all it does is points out a dozen or so characters on a webpage. Let's see if we can improve that.

- In Dockerfile of multi_stage_build2 we will change the Dockerfile slightly and we call `FROM alpine` and we copy from builder the name we assigned to ubuntu in Dockerfile above and copy google-size. too_big is 111 MB whereas too_big_2 is only 5 MB. Yet all the ability to have reproducable code is completely preserved.

### Golden Images

- Story time folks, long long ago at a tech company right near you, Karen learned about Docker and created a Dockerfile for their new service. It ran fine in production, until one day the site was down and it was an emergency. Alice updated that image, saved it right over the old image, and everyone got on with their day. No one worried about how that image no longer matched the Dockerfile.

- Afterwards Alice and Karen both moved on to new jobs, to bigger and better places, and nobody was ever able to maintain that Dockerfile again. They just kept using it exactly as it was like a golden image, that nobody dares to touch.

- **Some Lessons from that story:**

1. Include installers in your projects, five years from now the installer for version 0.18 if your project will not still be available, include it.
2. If you depend on a piece of software to build your image, check it into your image, gets good for that.
3. Have a canonical build system that builds your images from scratch, i.e. start from base image that has not been touched, by anyone in your organization, running a Dockerfile, build it up to the thing that you actually run in production.
4. Really really avoid the temptation to log in one day, fix the config file, save the image over the same name, and get it back in production. First time you do that, you no longer have a canonical build, you have created the golden image. Docker makes it very very easy to do that and very very difficult to resist.
5. Use small images such as alpine, if you start with a very large OS as base for your system, that large OS takes large maintenances.
6. Build images you share publicly from Dockerfiles, always, say you ship your product as Docker images.
7. Don't ever leave passwords or authentication tokens hidden in deep layers of your docker files.

---

# Docker - Under the Hood

### Docker the Program

- Now a little interlude on what the kernels do, Kernels are either part of a corn, a respectable military rank or decor of every computer you interact with, depending on your perspective. Kernel runs directly on the hardware and it has a bunch of jobs, most of which are pretty simple, and very important.

- **What Kernels Do:**

1. Kernels receives messages from the hardware, a new disk has been attached, a network packet arrived, everything that goes on electrically, bubbles up to the kernel and gets dealt with.
2. It starts and schedules programs, it says what is allowed to run, what and when.
3. It lets your computer do all the things, you are asking it to do at the same time.
4. It controls and it organizes teh storage devices on the computer.
5. When you say write to this file, Kernel goes like ahh, he says write to the file, he actually means this little spot on the disc. Kernel goes there and writes the data. Someone has to make that decision and that's the role of file system inside the kernel.
6. It passes messages between the programs, when two programs in the computer want to communicate, or 2 programs on different computers want to communicate over a network, they ask the kernel to pass a message, the kernel passes the message, gets it ready, sends it over to the kernel on the computer, which receives the message, gets it ready for the program, and sends it to program over there.
7. It allocates memory, CPU, time and bandwidth on network to be given to who, all of that stuff is managed by Kernel.

- **Docker** is a program which manages the Kernel. So, docker is well three things:

1. It is a program written in Go, GO is a nice upcoming systems language and its job is to manage several features of the kernel and use these features to build the concept of containers and images.

2. Manages Kernel Features
   1. Docker primarily uses c-groups or Control Groups to group processes together and give them the idea of being contained within their own little world, that's what keeps one container from interfering with other container.
   2. It use namespaces, which is a feature of Linux Kernel which allows it to split the networking stack so you have one set of addresses for one container and another set of addresses for another container and other addresses for things that are not in containers at all.
   3. It uses copy on write file-systems and various other approached to build the idea of images.

3. Used for years before Docker: Honestly, almost none of what docker does is truly new, docker took things that people were working very hard to do and they made it easy, approachable, and they created a language around it for people to talk about it. And made these things popular.  

- **What docker really does is make scripting distributed systems easy.** And that's the reason it is taking off the way it is. These things used to be something which very large enterprises were able to do with enormous budgets and this is now easily done on anyone's computer, ofcourse the definition of easy is subjective in this case.

- Docker is divided into two programs : **The Client** and **The Server**. These two programs communicate over a socket, that can be a network socket where client runs on one computer, and server runs on a computer somewhere on a cloud provider or they can be running directly on the same hardware or they can be running on the same hardware with the server in a VM which is a common case for people using Docker Desktop.

- In that case client communicates over a network, and sends messages to Docker server to say, make a container, start a container, stop a container etc. etc. When client and server are running on the same computer, they can connect through a special file called a socket. Since client and server can communicate through a file docker can efficiently share files between hosts and containers, it means you can run the client inside Docker itself.

- So traditional Docker scenario with a single host, you have Docker the program, it connects to the socket, sends commands to Docker the program, which is the server side, and that creates containers or deletes containers, all the rest.

![](https://i.imgur.com/1LpYhvQ.png)

- It is pretty easy to run the client inside one of the containers as well and share the socket into that container which allows the same messages, to go through the same socket, get to the server, running on the host and do everything that it would do normally. Let's try that in practice.

-  Idea: **Control docker through its socket.** Docker file will be present at `/var/run/docker.sock` for linux and mac. If you write the proper data into this file in right format, it will cause docker server to do things. We run docker client inside a container and give it access to Docker control socket on our computer using `docker run --rm -ti -v /var/run/docker.sock:/var/run/docker.sock docker sh`, we mount a volume and basically we have given the Docker container a hook for its client to control its own server. After that we use image named docker provided by Docker The Company and we will run just a shell.

-  Once container is ready after image pull, you can check if docker is running by doing `docker info` inside the container. Now we start a new container from a client within container by doing `docker run -ti --rm ubuntu bash`. Now this is not docker inside docker, this is a client within a Docker container, controlling a server that is outside the container. This flexibility in where you control Docker from is one of the key ideas behind Docker, and has been a major contributor to its success and popularity.

### Networking and namespaces

- One of the main things docker does is manage your networking for you to create containers. Let's go over at some of the things docker does for us. First of all, networking is divided into many layers. 
1. Bottom Layer: This is how machines that are near each other, or containers that are near each other actually talk directly to each other, we call this the **ethernet layer**. It moves little frames of data in a local area.

2. Above that layer we have the **internet protocol layer** or IP, and that is how data moves between the networks, and between systems in different parts of the world.

3. **Routing**: This is how packages get into and out of networks. Docker takes care of setting this as well for us.

4. **Ports:** When we talk about ports throughout this course, we are talking about specific programs running on a specific computer, actually listening to traffic. 

- **Bridging:** Docker uses bridges, to create virtual networks inside our computer. When we create a private network in docker, it creates a bridge. These functions are like software switches, it is equivalent to having a little blue box on your desk and plugging a bunch of different wires into it, except it is all within your computer and you are plugging containers in it with virtual network wires. These bridges are used to control the ethernet layer, containers that actually talk directly to each other.

- Let's look at these bridges, inside a running Docker system. To look at this we will need a system with `brctl` or bridge control program installed, we do so by running container with `docker run --rm -ti --net=host ubuntu bash` **--net=host** this gives container full access to the host's networking stack, turns off all the protections, we start up ubuntu image. Once container starts we do `apt-get update && apt-get install -y bridge-utils`. After this is installed we do `brctl show` and this will show us system has a couple of bridges on it, one is called docker0 which is the Virtual Network used by all machines in Docker that doesn't have their own network. Now if we go to other terminal and create a new network by running `docker network create my-new-network`. Now if we see the bridges using `brctl show` we can see that new network will show up. To further verify you can see the network ID, starting few characters of the network you see by brctl show and after creating the network would be same.

- So, we understand now that docker isn't magically moving packets between containers, it is creating bridges by running commands to configure your system, in that demo we turned off the isolation that prevents containers from messing with the host's network by passing the --net=host option. This is very useful to learn and debug and isn't a good idea to have this turned on for production environment container.

- Moving to next layer, we go over how docker moves packets between networks and containers and that's **Routing**. Docker uses the built-in firewall features of the Linux Kernel, namely the iptables command to create firewall rules that controls when packets gets sent between the bridges and thus becomes available to the containers that are attached to those bridges. This whole system is commonly referred to as NAT, or Network Address Translation. That means when a package is on its way out towards the internet, you change the source address, so it will come back to you.  And when package is on its way in you change the destination address so that it looks like it came directly from the machine you were connecting to. We can take a look at these for our docker container by running `sudo iptables -n -L -t nat` -t stands for table.

- To look at how Docker accomplishes port forwarding under the hood, now we can see how docker actually gets the packets into and out of a container, for this we will need some programs, namely the iptables utility to be available, and that is what docker is there for. So, we run `docker run --rm -ti --net=host --privileged=true --cap-add=NET_ADMIN ubuntu bash`, `--net = host` gives the container direct access to networking, then we will need to further turn off the safeties by running `--privileged=true` which will allow the container to have full control over the system that is hosting it.

- First you will have to start your docker image you will have to enable specific capabilities while running container which can be done by running the following commands: More on this [here](https://unix.stackexchange.com/questions/459206/list-ip-tables-in-docker-container)
```bash
docker run --rm -ti --cap-add=NET_ADMIN ubuntu bash
apt update -y
apt-get install iptables sudo -y
sudo iptables -n -L -t nat 
```
- On running iptables command we get the following output:

```bash
root@fsociety:/# iptables -n -L -t nat
Chain PREROUTING (policy ACCEPT)
target     prot opt source               destination         
DOCKER     all  --  0.0.0.0/0            0.0.0.0/0            ADDRTYPE match dst-type LOCAL

Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
DOCKER     all  --  0.0.0.0/0           !127.0.0.0/8          ADDRTYPE match dst-type LOCAL

Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination         
MASQUERADE  all  --  172.18.0.0/16        0.0.0.0/0           
MASQUERADE  all  --  172.17.0.0/16        0.0.0.0/0           

Chain DOCKER (2 references)
target     prot opt source               destination         
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
RETURN     all  --  0.0.0.0/0            0.0.0.0/0           
```
- Now to make things interesting, let's start a container with some ports to forward to, using `docker run -ti --rm -p 8080:8080 ubuntu bash`

- We map port 8080 on host into port 8080 into container. After starting that container, if we go to our privileged container,  and we run iptables command again, now we see that we have a port forward rule, that says forward anything with destination port 8080, to that docker conatiner's IP address on port 8080.

![](https://i.imgur.com/JB3RDmu.png)

- So, it shows that exposing ports in Docker is just port forwarding at the networking layer. Namespaces are a feature in Linux Kernel that allows us to provide complete network isolation to different processes in the system, so it enforces the rule that you are not allowed to mess with the networking of other processes.

- Processes running in containers are attached to virtual network devices, and those virtual network devices are attached to bridges, which lets them talk to any other containers. Containers have virtual network cards. Each container has their own copy of entire linux networking stack, all the pieces that make up the networking are isolated to each container, so that they can't do things like reach in and reconfigure other containers.

- Namespaces enforce the rule of docker and keep containers safe from each other.

### Processes and cgroups:

- One of the Docker's job is to manage the processes in containers, keep them isolated and keep them talking to each other where appropriate.

- **Understanding Linux Processes:** Processes comes from other processes, it is a parent-child relationship, just one parent though. When a process exits it returns a code to its parent, the package to the process that started it. And there is a special process, **process zero called init**, that is the process that starts it all and every other process in a linux system comes from dividing that process into other processes. So in docker our container starts with one process called, the init process. That process can then divide into other processes, and do any number of things, often it starts with a shell and that shell splits off and runs commands and runs other processes. When that init process exits, your container just vanishes and other processes that were in it at the time that the init process exits, gets shut down unceremoniously.

- We start an example container by doing `docker run --rm -ti --name hello ubuntu bash`. Now to find the name of root process inside the container, the convenient way to do that is with **docker inspect** command. We run `docker inspect --format '{{.State.Pid}}' helllo` . This command tells the docker that we only want to find out the process id, Pid of the main process in the container called hello. We run this in another terminal. In our case we get the output `12631` as Pid of main process. Now we start a super container with most of the safety features and network isolation turned off using `docker run --rm -ti --privileged=true --pid=host ubuntu bash`, so that we may use it to kill a process in the underlying host without being stopped by Docker, we pass `--privileged=true` to turn off most of the safety features, and `--pid=host` meaning to turn off even more of the safety features, after that we start an ubuntu image, and run it interactive bash. 

- Now that super container running with lots of privileges, we kill the process using `kill 12631`. As soon as you run kill in super container, the hello container exits on its own as we killed its init process.

![](https://i.imgur.com/OeSknuX.png)

- **Resource Limiting**

- A very important job of Docker is to control access to the limited resources on the machine. That's the amount of time the CPU can spend doing things and the amount of memory that can be allocated between the containers. These limitations are inherited and this is very important. If a container is given a certain amount of memory and CPU time, no matter how many processes it starts, the sum total of all those processes cannot exceed the quota that was given to the initial process. Processes in that container can split the  resources amongst themselves, however they would like but can't escape the hard limits that are set by starting more processes in order to consume more limits.

### Storage in Docker

- Let's go over how Docker accomplishes the idea of images and containers. How does docker do the storage behind the containers? Keeping containers isolated, letting them stack on top of each other, all of that stuff requires a little bit of an introduction to the layers of the Unix file systems method. At the very lowest level we have actual thing, stuff that stores bits, **HDDs, thumb drives, network storage devices** etc, hardware things to which we can set a value of 1 or 0 and it will remember it and you can come back later on and get it. The **kernel manages these**.

- On top of those memory devices, it forms them into **logical groups**, you can say these four groups form a RAID array, or two drives should be treated as one drive, or one drive can be treated as separate 432 drives. So, we have a layer that sort of **lets us partition drives arbitrarily into groups and then partition those groups up**. Docker makes extensive use of this capability.

- On top of logical groups called partitions we have **file systems.** This determines which bits on the drive are part of which file. So say, fourth byte in the file foo.txt would be at a particular spot on a particular drive partition, and it is upto the kernel with the file systems to keep track of where that spot actually is. Now on top of all this, you can take programs, and programs can pretend to be file systems, we call these **FUSE file systems**.

- With that in mind, the **Secret to Docker**! The secret to docker and all of its images is COWs.

![](https://i.imgur.com/epLj3h1.png)

- Well not the animal but principle of **Copy on Write**. We start with our base image, this is a cow, it has no spots, now we want to write some spots, instead of writing directly on the cow, as someone else might be working with this image/cow, there might be another container running off of this cow/image right now. 

![](https://i.imgur.com/fJt2M4s.png)

- So we write our spots on a separate layer, right next to Cow and then when we start the container layer the spot and give that to the container. So container sees the image of cow with spots, even though cow image still exists without spots, and others that are still using spotless cow image can continue to do so, now we want these dark spots.

![](https://i.imgur.com/HA1AJyl.png)

- Now somebody else really has a more artistic approach, they like a cow with more light spots, so we start with same image, literally the same image, not a copy of the image, they make their own layer with their own interpretation of how they want their cow spots. They make their own File System layer, and layer it over that image, giving us another cow with different color spots. Called copy on write. When we write to this cow spots, instead of directly writing on the vanilla cow with no spots, we write on our own layer which gets layered on top of the cow, when we want to look at the cow, we look at the entire thing, vanilla cow with our filesystem all embedded into one.

![](https://i.imgur.com/4kla8Ut.png)

- We read from the original and copy when we write. So we have the original image of the cow, unchanged (just one of it), we have 2 resulting images of that cow, each one has different set of layers on top of it, giving us a total of 3 images here and 2 combinations of how base image was layered.

![](https://i.imgur.com/lvzbV1l.png)

- Docker has many different in kernel mechanisms that it uses for managing the COW layers, the copy on write File System layers, and these depend a lot on what is available, on the system its running on. Sometimes it uses BTRFS, sometimes it uses LVM, Logical Volume manager, sometimes it uses the overlay file system. There are many ways and it doesn't really matter as long as it can accomplish layering on its own sort. We don't have to worry about the format of COW on one machine, a copy on write file system, if you want to import that image to another one because what it does it it takes each layer, splits out the layers and makes them into normal gzip files and ships them over the network to you separately and the receiving end of that network connection, the place where you're actually running the docker server, receives all of those layers separately and then puts them together using the file systems that are available on that computer. 

- That was how COWs can move easily between computers. So, containers are independent of the storage engine, we can send images between machines pretty much freely, there is one catch, depending on the storage layer that is in use on a particular machine, it is possible in some cases to run out of layers. Some of the storage engines have a limited number of layers, and others don't. If you make a very deeply nested image on a machine, that allows a great number of layers, and then you package it and try to download it, on a machine that uses a storage engine with less layers, you can run out of layers. It is worth being vaguely aware of though it doesn't comes up that often.

- **Volumes and Bind Mounting:** Another part of images and storage in docker, is how we share volumes and shared folders with the host. These aren't some rocket science concepts that are happening. These are built right into the linux file system, Linux VFS (Virtual file system). Linux file system starts with an assumed root root directory called /, everything is /something. /home/cosmic, /etc/conf etc. / is the root of the tree where it all starts. You attach devices to various points along that / tree. We can also take a directory from anywhere in the File System and just attach it somewhere else, right on top of what is there. Let's take a look at that.

- Now we will demonstrate bind mounting in action, we need a convenient linux system to show it in. Well that's what Docker is for. So we spin up a docker container to work with some safety turned off using `docker run --rm -ti --privileged=true ubuntu bash`. As we want to manipulate the file system, we will have to exceed the default permissions for a Docker container, and to do that we use `--privileged=true`. We make a new directory using `mkdir example` and inside example we make another directory called work. We make some files inside work directory. Go back to example `>> mkdir other-work >> cd other-work/ touch other-a other-b other-c other-d`.

- Go back up to example directory and do `ls -R` to show contents in all directories. Now we do `mount -o bind other-work work` to say attach one directory on top of another. Now if we do `ls -R` it will show us that ./other-work contains other-a other-b other-c other-d and now work has contents of other-work folder layered on top of it. It didn't copy these 4 files, it just took the directory that contained `other-a other-b other-c other-d` and it attached it to the file system right on top of the work directory. It also didn't delete them, they are still there but are just presently covered up.

- We can uncover those files by doing umount work `umount work`. This will unmount the work. Now if we do `ls -R` it will show us that everything is back the way it was before. The files in work were just temporarily covered by another directory. There is an important side effect of this. Which is that you have to **get the mount order correct**. If you want to mount a folder and then a particular file within that folder, you have to do that in that specific order.

- If you mount the file so it bind mounts that file right onto that position and then you mount the folder on top of it, the file will be underneath that folder, and will be hidden. So it is important to get the order of -v for volume arguments to Docker correctly.

- Also remember that mounting volumes always mounts the host's filesystem over the guest. Docker doesn't gives you a way of saying **mount this guest file system into the host preserving what was on guest.** It always chooses to mount the host file system over the guest file system.