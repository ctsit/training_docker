# Docker Training Module 2

These instructions will guide you through the first steps to setup a Docker environment on a Mac. From there you will be able to setup your first "hello world" container. The intent is to get you familiar with the commands used to start a container, check its status and stop the container. Please begin by forking this repository to your GitHub account and then cloning the repository to your Mac. Then follow these steps:

1) Create a Docker ID
- Visit [https://www.docker.com](www.docker.com)
- Click Create Docker ID and follow the prompts

2) Next Install Docker for Mac
- [https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install "Docker for Mac page")
- Click "Get Docker for Mac (Stable)" link and follow the instructions on the website and in the app
- Provide your Docker ID for the app to use while it runs in the background

3) Open a terminal in a new Mac desktop that is full screen and let's have fun with Docker commands

### Exercise 1 Run a Container
1. In your terminal type the command ```docker search debian | more```
	* note that both ubuntu and debian are listed at the top and that there is an [OK] under the official column. This means these images are the offically blessed images by the ubuntu and debian community.
2. Let's choose the debian image for our first exercise and type the command ```docker run -ti debian:stretch bash```
	* note how quickly you built a container and it is running. This action pulled an image from the public Docker registry and ran it.
3. Now let's check the Linux release of the container by typing ```cat /etc/*-release```, the version should say 9 (stretch)
4. Now type ```exit``` to exit the container or press Ctrl D on your keyboard. Either option works.
5. Let's clean up the container we just created by listing it and removing it, type the command ```docker container ps -a```
	* note that ```docker container ps``` just shows running containers and the -a switch says to show all containers running or not
6. Now remove the Docker container by typing ```docker rm "container_id"``` where container_id is the unique identifying number that is given to the Debian container you just ran.
	* note containers have their own unique ids and images have their own set of unique ids
7. Now type the command ```docker run --name voldemort debian:stretch``` then run ```docker container ps -a``` then remove voldemort from your computer by typing ```docker rm voldemort```
	* note wasn't that easier than having to type the container identifier?

### Exercise 2 Working with Commit
In this exercise you will become familiar with the docker commit command which allows you to create a new image from an existing container. This is useful if you have made changes to a container and want to make a new image from it for future use. 

1. In your terminal type the command ```docker run -ti --name ronald debian:stretch bash```
2. Now create a file in this new container by typing ```touch ron_was_here.txt``` and type ```ls```
	* note your file is at the root of your container
3. Now exit your container and type ```docker container ps -a``` to note that the ronald container is there but stopped
4. Now type ```docker start ronald``` which starts up the container
5. Now type ```docker attach ronald``` which puts you back in the bash shell of the container, type ```ls``` to make sure the ron_was_here.txt file is still there then type ```exit```.
6. Let's make a new image from the ronald container by typing ```docker commit ronald harry``` and then type ```docker commit ronald harry:potter```
7. Type ```docker images``` and note that you now see two images one name harry with a tag of latest and another named harry with a tag of potter
8. Type ```docker run -ti harry:potter``` and type ```ls``` to note that the ron_was_here.txt file is in the new container
9. Type exit
10. Type ```docker rm harry:potter```
11. Type ```docker rm harry```

Although this example is simple the idea here is to see how you take a container, add new configurations to it and make and image from it to be used in other containers.

### Exercise 3 Working with commands
In this exercise you will become familiar with running a container that runs a command. 

1. In your terminal type the command ```docker run -ti --name kirk debian apt-get update``` and note that apt-get update ran
2. Type ```docker container ps -a``` and notice the output, you see the kirk container is stopped and that its command was apt-get update
3. Type ```docker rm kirk```

The entire point of this quick exercise is to know that docker containers run a single process and when that is done it stops the container afterward.

### Exercise 4 Creating containers with a Dockerfile
At this point we have been running containers in a very rudimentary fashion just to get familiar with basic commands. Now we move into building an image with a Dockerfile.You will use the Dockerfile that is already included in this repository as the starting point.

1. 



## You have now completed the first online exercise for Docker training. You will now be taken back to the Jump on Board website to begin the next module. Please return to the <a href="https://ctsit.github.io/J.O.B.-Jump-On-Board#dockermodule3" target="_blank">Docker Training Course Website</a> to continue to the next section.
