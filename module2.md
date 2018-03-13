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
8. Type ```docker run -ti --name harry harry:potter``` and type ```ls``` to note that the ron_was_here.txt file is in the new container
9. Type exit
10. Type ```docker rmi harry:potter```
11. Type ```docker rm harry```

Although this example is simple the idea here is to see how you take a container, add new configurations to it and make and image from it to be used in other containers. However, as useful as docker commit is ultimately you will most likely use it rarely and instead will favor the Dockerfile process to build your images. Using docker commit is typically a one time process where you have to create the first container to make the next image. A Dockerfile is a more consistent way to make repeatable images or new images with changes to the Dockerfile options.

### Exercise 3 Working with Commands in a Container
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

#### Exercise 4a Creating more containers with another Dockerfile
We will now build a second image with another Dockerfile. Remember that the captial D in the word Dockerfile is a required.

Change directory up one level or ```cd ..``` and then change directory again into the build2 folder or ```cd build2```. Cat or open the Dockerfile and see the contents. This time we are pulling an existing nginx image from the Docker public repository. Nginx is a webserver much like Apache.

1. Let's build the image from the Dockerfile by typing ```docker build -t helloworld .``` Note the build process and review the output.
2. Now run the helloworld image as a container by typing ```docker run -ti -p 8080:80 --name web1 helloworld```
	* note that I added the -p option which specifies to the external world port 8080 is open and maps to the container's port of 80.  Also note that we called this container web1 and built it form the helloworld image we created in step 1.
3. Let's go see the results of this container running by opening a browser and typing in the address ```localhost:8080```. You should see the statement Hello World. 
4. Return to the terminal window and Ctrl D to exit the container and type ```docker rm web1``` or keep it around if you like.

#### Exercise 4b Create your own Dockerfile
Now you will create your own Dockerfile. In the root of the training_docker repository create a directory called build3 and inside of that folder create a Dockerfile. You can get as elaborate or simple as you like with this file. At the conclusion of your Dockerfile creation git add and commit the build3 folder and Dockerfile to your forked repository for review later by the instructor.

### Exercise 5 Working with Persistent Volumes
Now that we have created images and containeres let's take a moment to practice using persistent volumes in our containers. In this first exercise you are setting up a container that creates a database that is shared with the host and the container.

1. Start a container from an image we have already created but this time mount a volume in the container by typing ```docker run -ti -v /Users/<yourusername>/Desktop:/shared-folder debian:stretch bash```
	* note that the /Users/<yourusername>/Desktop is the desktop of your Mac and the /shared-folder is the name of the folder in the container that can access your Desktop.
2. Touch a file on your desktop from within the container by typing ```touch sqlite.db```. 
3. Now exit the container by pressing Ctrl D and take a moment to look on your desktop to see if the file showed up. Note that the file survived the container exit and persisted on your desktop. Copy the file to your training_docker repository and git add and git commit the file to your forked repository. 

#### Exercise 5a Working with Ephemeral Volumes
In this exercise you are setting up a volume that will share data amoung two containers using an ephermeral volume.

1. Start a container from an image we have already created and setup a shared volume in the container by typing ```docker run -ti --name db01 -v /shared-data debian:stretch bash```
2. Now in the commandline of the new container add data to the shared-data folder by typing ```cd shared-data``` and then type ```touch tmp```.
3. We need to keep the db01 container running so let's leave it running and open a new terminal tab by pressing Command T on your keyboard.
3. In this new terminal window create another container that will access the db file on the container we just setup, type ```docker run -ti --name web01 --volumes-from db01 debian:stretch bash```.
4. Now you are in the web01 container bash shell type ```ls``` and you will see the shared-data folder available on the web01 container. 
5. Type ```cd shared-data``` and ```ls``` and you will see the tmp file that you created in the db01 container.
6. Let's add another file to this folder by typing ```touch cache```.
7. Return to the previous tab that had the db01 container running and type ```exit```.
8. Now return to the terminal tab that was running the web01 container and type ```ls``` and you will see that even though the db01 container is gone the web01 container still has the two files (tmp & cache) that were created.
9. Let's start a third container for this exercise and mount the same shared-data volume by typing ```docker run -ti --name db02 --volumes-from web01 debian:stretch bash```.
10. Type ```ls``` and you will see the shared-data folder in the new db02 container. CD into the folder and you will see the tmp and cache files as well.
11. Now exit both the web01 and db02 containers
12. "Poof" now the data is gone and this demonstrates the very temporary nature of ephemeral volumes. Ephemeral volumes can be good for temp files or caches that can easily go away with out worry of retreival again.

#### Exercise 5b Working with volumes in a Dockerfile
Although Dockerfile supports the VOLUME directive you do not typically make an image with a specific volume in the Dockerfile. The VOLUME can be host specific and since containers are susposed to be deployable without any dependencies like local host volumes you instead create the container and specifiy the local volumes for your environment in the docker run -v hostdata:containerdata step. So therefore, this step in your training is simply to reinforce the concept that volumes are host and location specific and containers can be built to manipulate, process and transform local volumes where ever they go. 

### Exercise 6 Working with Networks
In this exercise we will setup two containers that will talk over a bridged network that we create for them to use. 

1. Let's first setup the network we are going to use by typing ```docker network create partytime```
2. Now let's create a web server and attach it to the partytime network. CD or change directory into the build5 directory and then CD again into the webcontainer1 folder. Now type ```docker build -t nginx:web1 .```
3. Now run the new image in a container and connect it to the network partytime by typing ```docker run -ti --net=partytime --name webserver01 nginx:web1 bash```
4. In the webserver01 container type ```service nginx start```
5. Now press Command T on your keyboard to open another terminal tab, type ```cd ..``` to jump up one level in your repository and then type ```cd curlcontainer2```.
6. Build the curl container by typing ```docker build -t curl:image .```
7. Run the curl container by typing ```docker run -ti --net=partytime --name curlcontainer curl:image bash```
8. Run the ping command by typing ```ping webserver01``` and notice that Docker has already added the webserver01 to the hosts file so that this ping will resolve the webserver01 container. Press Ctrl C to quit the ping command.
8. Now type the command ```curl -s http://webserver01/wineglas.vt | pv -L3000 -q```
9. Cheers!

## You have now completed the online exercises for Docker training. You will now be taken back to the Jump on Board website to begin the next module. Please return to the <a href="https://ctsit.github.io/J.O.B.-Jump-On-Board#dockermodule3" target="_blank">Docker Training Course Website</a> to continue to the next module.
