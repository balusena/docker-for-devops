# Docker 

## 1.What is Docker?
Docker is a containerization platform that provides easy way to containerize your applications, which
means, using Docker you can build container images, run the images to create containers and also push 
these containers to container regestries such as DockerHub, Quay.io and so on.

In simple words, you can understand as containerization is a concept or technology and Docker Implements 
Containerization.The Isolation provided by container gives a layer of security to the containers.

### Docker Architecture
![Docker Architecture](/docker-for-devops/01-Docker Containers Introdution and Setup/docker_architecture.jpg)

## 2.Why Docker?
- 1.Simple 
- 2.Fast Easy  
- 3.Collaboration 
- 4.Built for Developers by Developers  
- 5.Docker Community

- An application is working fine on developer console but not in testing or in production.

- In Dev there can be a software which is updated but in testing and production an older version is being used.

- Docker is a computer program/tool that makes it easier to deploy and run applications using a concept known
  as “containerization”.

- Imagine as a developer your application is packaged up with all of the parts it needs, such as libraries 
  and other dependencies, and it is shipped out as one package

- Docker in other words is a light weight virtualization tool and containerizing platform where you can run 
  and deploy applications and its dependencies which can be run in a Linux environment.

- Docker is an open-source project based on Linux containers. It uses Linux Kernel features like namespaces 
  and control groups to create containers on top of an operating system.

## 3.Docker Engine
- Docker engine is the layer on which Docker runs. It’s a lightweight runtime and tooling that manages 
  containers, images, builds, and more. It runs natively on Linux systems and is made up of:
  - A Docker Daemon that runs in the host computer.
  - A Docker Client that then communicates with the Docker Daemon to execute commands.
  - A REST API for interacting with the Docker Daemon remotely.

## 4.Docker Client
- The Docker Client is what you, as the end-user of Docker, communicate with. Think of it as the UI for 

## 5.Docker.Docker Daemon
- The Docker daemon is what actually executes commands sent to the Docker Client — like building, running, 
  and distributing your containers. The Docker Daemon runs on the host machine, but as a user, you never 
  communicate directly with the Daemon. The Docker Client can run on the host machine as well, but it’s not
  required to. It can run on a different machine and communicate with the Docker Daemon that’s running on 
  the host machine.

## 6.Volumes
- Volumes are the “data” part of a container, initialized when a container is created. Volumes allow you to 
  persist and share a container’s data. Data volumes are separate from the default Union File System and 
  exist as normal directories and files on the host filesystem. So, even if you destroy, update, or rebuild 
  your container, the data volumes will remain untouched. When you want to update a volume, you make changes
  to it directly. (As an added bonus, data volumes can be shared and reused among multiple containers, which
  is pretty neat.)

## 7.Microservice Architecture :

- The idea behind microservice is some application is easier to build and maintain where broken down to 
  smaller parts.

- Each component is developed separately and done....

Example : Online Shopping Service :
  - 1.Account Service
  - 2.Product Catalog
  - 3.Cart Server
  - 4.Order Server

### Advantages of microservice architecture :

- Building and maintenance is easy as broken down to smaller parts.
- If we need some new features or update in a module, it is easier because dependencies will be less 
  compared to the application as a whole.
- If any component go down, application will be largely unaffected.

### What is the problem to adopting microservices :

- Before DOCKER : For microservice architecture we have a host machine and there are several virtual machines
  each virtual machine is for a microservice. So problem is that lots of resource waste. As we use more and 
  more VMs for bigger application lots of disc space, RAM are unused.

## 8.How Docker solve this problem :

- We can run several microservices in one virtual machine by running various docker
- containers for each microservice.
- Docker do not need any RAM,DISK requirements initially.

### How Docker solves the problem "not having consistent computing environment throughout the process of 
   delivery (development, testing, production)" :

- Docker containers are developed by the developers.
- Docker provides a consistent computing environment throughout the whole SDLC(Software Development Life Cycle).

## 9.What is an Image?

- Docker image is the basis of containers. It’s a collection of layers stacked on top of each other. Each 
  Docker image references a list of read-only layers that represent filesystem differences. Think of it 
  like the jar file for java applications, you create one jar file but you can deploy it anywhere a java 
  run time is enabled.

- A docker image is an archive containing all the files that go in a container.

- You can create many docker containers from the same docker image.

- The image can then be deployed to any Docker environment and executable as a container.

- A Docker image is containing everything needed to run an application as a container. This includes:
     - code
     - runtime
     - libraries
     - environment variables
     - configuration files

![Docker Architecture](01-Docker Containers introdution and setup/container_layers.png.jpg)

