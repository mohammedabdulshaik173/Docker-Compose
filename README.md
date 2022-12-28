# Docker-Compose**
Why we use docker compose?
Instead of building docker images for specific docker files we will just write all the docker file and build the images at one shot

Steps to install the docker:
============================
step1- Login to EC2 instance
step2- Install the docker engine on top of EC2 with this command "yum install docker"
step3- start the docker engine "systemctl start docker" and "systemctl enable docker"

Steps to install the docker compose:
====================================
Copy the appropriate docker-compose binary from GitHub:
-------------------------------------------------------
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

NOTE: to get the latest version (thanks @spodnet): sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

Fix permissions after download:
-------------------------------
sudo chmod +x /usr/local/bin/docker-compose

Verify success:
--------------
docker-compose version

Write the Dockerfile and build the image by using docker compose:=
==================================================================
Steps to write the Dockerfile:
------------------------------
Step 1- First write the Dockerfile "vi Dockerfile"
Step 2- write the below code as per the requirement(for my requiremet i am writing the dockerfile for static application)

FROM ubuntu
RUN apt-get update
RUN apt-get -y install apache2
RUN apt clean
COPY index.html /var/www/html/
EXPOSE 80
CMD ["apachectl", "-D", "FOREGROUND"]


Step 3- write the docker compose file "vi docker-compose.yml"(In the below docker-compose.yml file i am trying to below multiple docker image with the same docker file)

version: "3.3"
services:
  webapp:
    build:
   # In the context value you need to mentioned the path where the docker file is present
      context: .
   # In the dockerfile value file you need to mentioned the docker file name(In have created the docker file name as "Dockerfile" that way i have mentioned the same"
      dockerfile: Dockerfile
   # in the image file you need to mentioned the image name and the image tag   
    image: static:3
  webapp:
   # In the context value you need to mentioned the path where the docker file is present
    build:
      context: .
   # In the dockerfile value file you need to mentioned the docker file name(In have created the docker file name as "Dockerfile" that way i have mentioned the same"
      dockerfile: Dockerfile
   # in the image file you need to mentioned the image name and the image tag  
    image: static:4
    
Step 4- To build the docker image by using docker compose run this command "docker-compose build"
