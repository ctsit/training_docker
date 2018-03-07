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
At this point we have been running containers in a very rudimentary fashion just to get familiar with basic commands. Now we move into building an image with a Dockerfile. You will use the Dockerfile that is already included in this repository under the directory build1 as the starting point.

Change directory or ```cd build1``` into the build1 folder of this repository. Cat or open the Dockerfile to see the contents. Note the structure of the file. The FROM line is always first and says what image to build from, in this case debian:stretch. The first RUN line performs and update and installs curl and pv and the second RUN line runs an echo statement. The CMD line will run the command once the image is run as a container. 

1. Let's build the image by typing the command ```docker build -t spell .```
	* Note the steps of the build process
2. Now run the image as a container by typing the command ```docker run --rm spell```
	* note the --rm command automatically removes the container once it has completed. This saves the step of running ```docker rm spell``` after the container is done.

#### Exercise 4a Creating containers with another Dockerfile
We will now build a second image with another Dockerfile. Note that a file called dockerfile will not work and only the work Dockerfile with a capital D will work to build an image.

Change directory up one level or ```cd ..``` and then change directory again into the build2 folder or ```cd build2```. Cat or open the Dockerfile and see the contents. This time we are pulling an existing nginx image from the Docker public repository. Nginx is a webserver much like Apache.

1. Let's build the image from the Dockerfile by typing ```docker build -t helloworld .``` Note the build process and review the output.
2. Now run the helloworld image as a container by typing ```docker run -ti -p 8080:80 --name web1 helloworld```
	* note that I added the -p option which specifies that to the external world port 8080 is open and maps to the internal port of 80. Also note that we called this container web1 and built it form the helloworld image we created in step 1.
3. Let's go see the results of this container running by opening a browser and typing in the address ```localhost:8080```. You should see the statement Hello World. 
4. Return to the terminal window and Ctrl C to exit the container and type ```docker rm web1``` or keep it around if you like.

#### Exercise 4b Create your own Dockerfile
Now you will create your own Dockerfile. In the root of the training_docker repository create a directory called build3 and inside of that folder create a Dockerfile. You can get as elaborate or simple as you like with this file. At the conclusion of your Dockerfile creation git add and commit this folder and Dockerfile to your forked repository for review later.

### Exercise 5 Working with Volumes
Now that we have created images and containeres let's take a moment to practice using persistent volumes in our containers. 

1. Start a container from an image we have already created but this time mount a volume in the container by typing ```docker run -ti -v /Users/<yourusername>/Desktop:/shared-folder debian:stretch bash```
	* note that the /Users/<yourusername>/Desktop is the desktop of your Mac and the /shared-folder is the name of the folder in the container that can access your Desktop.
2. Touch a file on your desktop from within the container by typing ```touch Hermione_was_here.txt```. Take a moment to look on your desktop to see if the file showed up. Copy the file to your training_docker repository and git add and git commit the file to your forked repository. 

## You have now completed the first online exercise for Docker training. You will now be taken back to the Jump on Board website to begin the next module. Please return to the <a href="https://ctsit.github.io/J.O.B.-Jump-On-Board#dockermodule3" target="_blank">Docker Training Course Website</a> to continue to the next section.
