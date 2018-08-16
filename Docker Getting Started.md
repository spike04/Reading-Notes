# Docker Getting Started
**Video by**: Traverse Media

Source: [Exploring Docker](https://goo.gl/Wb1XuE)

# Episode 1: 
1. Install Docker from docker.com or other.
2. General Used Syntax
	- `docker` - To Check if docker is available or not.
	- `docker version` - To Check for Version informations
	- `docker info` -  Gives detail information about containers and images available.

## Creating Nginx Container
1. -it > interactive mode
2. -p > publish

***Command:***

`docker container run -it -p 80:80 nginx`

This will create a container with nginx image or download from docker hub and install it.

Now go to http://localhost and you can see the nginx landing page running. PORT not needed for PORT=80
> Note: Running the container in interactive mode will constantly give logs in the console. 

### **Docker Image**
This docker images are obtained from [DockerHub](https://hub.docker.com/)
Eg: Nginx is obtained from https://hub.docker.com/_/nginx/

***General Syntax***
- `docker container ls`  - This will show us our running container.
- `docker ps` -  View all the containers (older way)
- `docker container ls -a`  - This will show us all our containers running or not.
- `docker container rm <id>` -  This will remove the container.
- `docker images` - Show  all the images you have downloaded
- `docker image rm <id>` - Delete images from the system
- `docker pull nginx` -  Pull images
- `docker container stop <container_name>` - Stop Container
- `docker rm $(docker ps -aq) -f` - Stop All Containers

## Running nginx in background
1. -d > detach mode
2. 8080:80 > map port in the from in our machine with port at the end - meaning it will run in port 8080 in our machine even though image runs in 80 port. **PORT MAPPING**
3. —name > give name to the container

***Command***

`docker container run -d  -p 8080:80 —name mynginx nginx`

## Creating Apache Container
To add Container: 

`docker container run -d -p 8081:80 —name myapache httpd`

To Remove Container: (force remove)

`docker container rm <container_name> -f` 

## Create MySQL Container
- container of image with environment variable

`docker container run -d -p 3306:3306 —name mysql —env MYSQL_ROOT_PASSWORD=123456 mysql`   

##  Running Files in Nginx Container
1. exec > execute 

`docker container exec -it <container_name> bash` - this will open a bash terminal of container specified

Now in nginx container the files that is to server are in path usr/share/nginx/html directory.

Editing inside the bash is an option but can be mapped to file in the system using **VOLUME MAPPING**

## Creating a Container with BIND MOUNT
1. -v or —mount > volume mount flag 
2. $(pwd) - current directory path

***Command***

`docker container run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html —name nginx-website nginx` - creates a nginx container with exposed port 8080 and volume mounted to current directory

Now the File will be mapped to the files inside container. 

Using Dockerfile to run container with required files.

Eg: 
```
FROM nginx:latest - load container with nginx image

WORKDIR /usr/share/nginx/html - specifying workdir

COPY . .    - this copies everything from current directory 					to the specified workdir
```

## Saving the Configuration as an image
After creating required files structure and docker file

-t > 

***Command***

`docker image build -t <image_name> .` 

## Pushing image to DockerHub
- Create a Account in docker hub
- In terminal:
	- `docker login` - login to docker hub
	- `docker push <image_name>` - push image to dockerhub
