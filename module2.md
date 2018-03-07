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
7. Now type the command ```docker run -ti --name voldemort debian:stretch``` then run ```docker container ps -a``` then remove voldemort from your computer by typing ```docker rm voldemort```
	* note wasn't that easier than having to type the container identifier?

### Exercise 2 Present Working Directory
In a terminal window practice the present working directory command which is called pwd.

1. In your terminal type the command pwd
	* note that the terminal displays the current directory that you are working in
2. Document your current directory by typing the command ```pwd > pwd.txt```
3. Change directory to the root of your file system by typing ```cd /``` and then type ```pwd```
4. Change directory to the users directory by typing ```cd /users``` and then type ```pwd```
5. Type the command ```cd $repodir``` to return to your exercise directory

3) Next visit the Docker site and complete Parts 1 - 6 of their getting started with Docker
- Allow yourself 60 minutes to complete this tutorial
- Once you complete the 6 steps of this training please git add, commit and push your files you created from the tutorial to the forked training_docker repository
- [https://docs.docker.com/get-started/](https://docs.docker.com/get-started "Get Started with Docker")

## You have now completed the first online exercise for Docker training. You will now be taken back to the Jump on Board website to begin the next module. Please return to the <a href="https://ctsit.github.io/J.O.B.-Jump-On-Board#dockermodule3" target="_blank">Docker Training Course Website</a> to continue to the next section.
