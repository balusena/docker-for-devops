# Docker 

## 1.What is Docker?
Docker is a containerization platform that provides easy way to containerize your applications, which
means, using Docker you can build container images, run the images to create containers and also push 
these containers to container regestries such as DockerHub, Quay.io and so on.

In simple words, you can understand as containerization is a concept or technology and Docker Implements 
Containerization.The Isolation provided by container gives a layer of security to the containers.

### Docker Architecture
![Docker Architecture](https://github.com/balusena/docker-for-devops/blob/main/01-Docker%20Containers%20Introduction%20and%20Setup/docker_architecture.png)

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

## 5.Docker Daemon
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

![Docker Architecture](https://github.com/balusena/docker-for-devops/blob/main/01-Docker%20Containers%20Introduction%20and%20Setup/container_layers.png)

## 10.Dockerfile

Blueprint of a docker image (a text document) is known as Dockerfile. This file contains all the commands 
you would run in order to build the docker image you want. Docker can build images reading this file, which
is one of the key advantages of docker.
```
# Super simple example of a Dockerfile
FROM ubuntu:latest
MAINTAINER Tikam Alma 

RUN apt-get update
RUN apt-get install -y python python-pip wget
RUN pip install Flask

ADD hello.py /home/hello.py

WORKDIR /home
```
We first write a Dockerfile which is like the definition of the image. Using the Dockerfile we create
a docker image. We then push this image to Docker Hub and provide a unique tag that can be used to identify
our image. Using this tag and image name, we can pull the docker image and deploy on another computer as a 
docker container.

## 11.Docker Environment?
The Docker environment refers to the overall setup and infrastructure necessary for running Docker
containers and managing containerized applications.

It includes the following components:

### 1.Docker Engine:
- The core component of the Docker environment, responsible for running and managing containers. It consists
  of the Docker daemon, Docker CLI, and Docker API.

### 2.Docker Images:
- Docker images are the building blocks of containers. They contain the application code, dependencies, and
  configurations needed to run an application within a container.

### 3.Docker Containers:
- Containers are lightweight and isolated runtime instances created from Docker images. They encapsulate the
  application and its dependencies, providing a consistent and reproducible execution environment.

### 4.Dockerfile:
- A Dockerfile is a text file that specifies the instructions to build a Docker image. It includes commands
  to install dependencies, configure the environment, and define how the container should be run.

### 5.Docker Compose:
- Docker Compose is a tool for defining and managing multi-container applications. It uses YAML files to
  specify the services, networks, and volumes required for an application. Docker Compose simplifies the
  process of running and orchestrating multiple containers as a cohesive unit.

### 6.Docker Registry:
- A Docker Registry is a repository for storing and distributing Docker images. Docker Hub is the default
  public registry, but you can also set up private registries to store custom images within your organization.

### 7.Docker Networking:
- Docker provides networking capabilities to enable communication between containers and external networks.
  It allows containers to be connected to networks, assign IP addresses, and define network aliases for easy
  communication.

### 8.Docker Volumes:
- Docker volumes provide a way to persist data beyond the lifecycle of a container. Volumes can be attached
  to containers, allowing data to be shared between containers or stored outside the container's filesystem.

# Containers

## 1.What is a container?

A container is a standard unit of software that packages up code and all its dependencies so the application
runs quickly and reliably from one computing environment to another.

- A docker container image is s lightweight, standalone, executable package of software that includes 
  everything needed to run an application: code, libraries, settings etc.

- Container images become containers at runtime and in the case of docker containers images become 
  containers when they run on docker engine.

- Containers share the machine's OS system kernel and therefore do not require an OS per application.

- Applications are safer in containers and Docker provides the strongest default isolation capabilities in 
  the industry.

- Docker container is the actual running piece created from a docker image. The only difference between a 
  docker image and a docker container is a top writable layer. When you create a new container, you add a 
  new, thin, writable layer on top of the underlying stack. This layer is often called the “container layer”
  All changes made to the running container — such as writing new files, modifying existing files, and 
  deleting files — are written to this thin writable container layer. But once you delete the container, 
  this top layer will be deleted as well. So it’s not persistent. The best thing with docker is that you 
  can create a docker image using the current docker container with a commit. Hence, enabling us to capture
  system information and make it immutable so its reproducible anywhere. This solves many of the server 
  related problems we encounter these days.

![Docker Container Image](https://github.com/balusena/docker-for-devops/blob/main/01-Docker%20Containers%20Introduction%20and%20Setup/docker_image.png)

## 2.Containers Vs VM

- When talking about containerization it is very often compared to virtual machines. Let’s take a look at 
  the following image to see the main difference :

- The Docker container platform is always running on top of the host operating system. Containers are 
  containing the binaries, libraries, and the application itself. Containers do not contain a guest operating
  system which ensures that containers are lightweight.

- In contrast virtual machines are running on a hypervisor (responsible for running virtual machines) and 
  include it’s own guest operating system. This increased the size of the virtual machines significantly, 
  makes setting up virtual machines more complex and requires more resources to run each virtual machine.

![Container vs Virtual Machine](https://github.com/balusena/docker-for-devops/blob/main/01-Docker%20Containers%20Introduction%20and%20Setup/container_vs_vm.png)

Containers and virtual machines are both technologies used to isolate applications and their dependencies, 
but they have some key differences:

1. Resource Utilization: Containers share the host operating system kernel, making them lighter and faster 
   than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

2. Portability: Containers are designed to be portable and can run on any system with a compatible host 
   operating system. VMs are less portable as they need a compatible hypervisor to run.

3. Security: VMs provide a higher level of security as each VM has its own operating system and can be 
   isolated from the host and other VMs. Containers provide less isolation, as they share the host operating
   system.

4. Management: Managing containers is typically easier than managing VMs, as containers are designed to be
   lightweight and fast-moving.

![Docker Container vs VM](https://github.com/balusena/docker-for-devops/blob/main/01-Docker%20Containers%20Introduction%20and%20Setup/dockervsvm.png)

### Why are containers light weight ?
Containers are lightweight because they use a technology called containerization, which allows them to 
share the host operating system's kernel and libraries, while still providing isolation for the application
and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the 
containers do not need to include a full operating system. Additionally, Docker containers are designed to 
be minimal, only including what is necessary for the application to run, further reducing their size.

Let's try to understand this with an example:

Below is the screenshot of official ubuntu base image which you can use for your container. 
It's just ~ 22 MB, isn't it very small ? on a contrary if you look at official ubuntu VM image 
it will be close to ~ 2.3 GB. So the container base image is almost 100 times less than VM image.

![Ubuntu Image Light Weight](https://github.com/balusena/docker-for-devops/blob/main/01-Docker%20Containers%20Introduction%20and%20Setup/ubuntu_img_lw.png)

To provide a better picture of files and folders that containers base images have and files and folders 
that containers use from host operating system (not 100 percent accurate -> varies from base image to base
image). Refer below.

### Files and Folders in containers base images

```
    /bin: contains binary executable files, such as the ls, cp, and ps commands.

    /sbin: contains system binary executable files, such as the init and shutdown commands.

    /etc: contains configuration files for various system services.

    /lib: contains library files that are used by the binary executables.

    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.

    /var: contains variable data, such as log files, spool files, and temporary files.

    /root: is the home directory of the root user.
```

### Files and Folders that containers use from host operating system

```
    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
```

It's important to note that while a container uses resources from the host operating system, it is still 
isolated from the host and other containers, so changes to the container do not affect the host or other 
containers.

Note: There are multiple ways to reduce your VM image size as well, but I am just talking about the default
for easy comparision and understanding.

so, in a nutshell, container base images are typically smaller compared to VM images because they are 
designed to be minimalist and only contain the necessary components for running a specific application or 
service. VMs, on the other hand, emulate an entire operating system, including all its libraries, utilities,
and system files, resulting in a much larger size.

I hope it is now very clear why containers are light weight in nature.

## Working of Containers Deep-Dive
The term “container” is really just an abstract concept to describe how a few different features work 
together to visualize a “container”. Let’s run through them real quick:

### 1.Namespaces
Namespaces provide containers with their own view of the underlying Linux system, limiting what the container
can see and access. When you run a container, Docker creates namespaces that the specific container will use. There are several different types of namespaces in a kernel that Docker makes use of, for example:

### 2.NET
Provides a container with its own view of the network stack of the system (e.g., its own network devices, IP
addresses, IP routing tables, /proc/net directory, port numbers, etc.).

### 3.PID
PID stands for Process ID. If you’ve ever run ps aux in the command line to check what processes are running
on your system, you’ll have seen a column named “PID”. The PID namespace gives containers their own scoped 
view of processes they can view and interact with, including an independent init (PID 1), which is the 
“ancestor of all processes”.

### 4.MNT
Gives a container its own view of the “mounts” on the system. So, processes in different mount namespaces 
have different views of the filesystem hierarchy.

### 5.UTS
UTS stands for UNIX Timesharing System. It allows a process to identify system identifiers (i.e., hostname, 
domain name, etc.). UTS allows containers to have their own hostname and NIS domain name that is independent
of other containers and the host system.

### 6.IPC
IPC stands for InterProcess Communication. IPC namespace is responsible for isolating IPC resources between
processes running inside each container.

### 7.USER
This namespace is used to isolate users within each container. It functions by allowing containers to have a
different view of the uid (user ID) and gid (group ID) ranges, as compared with the host system. As a 
result, a process’s uid and gid can be different inside and outside a user namespace, which also allows a 
process to have an unprivileged user outside a container without sacrificing root privilege inside a 
container.

### 8.Control Groups
Control groups (also called cgroups) are a Linux kernel feature that isolates, prioritizes, and accounts for
the resource usage (CPU, memory, disk I/O, network, etc.) of a set of processes. In this sense, a cgroup 
ensures that Docker containers only use the resources they need — and, if needed, set up limits to what 
resources a container can use. Cgroups also ensure that a single container doesn’t exhaust one of those 
resources and bring the entire system down.

### 9.Isolated Union File System
Docker uses Union File Systems to build up an image. You can think of a Union File System as a stackable file
system, meaning files and directories of separate file systems (known as branches) can be transparently 
overlaid to form a single file system.

The contents of directories which have the same path within the overlaid branches are seen as a single merged
directory, which avoids the need to create separate copies of each layer. Instead, they can all be given 
pointers to the same resource; when certain layers need to be modified, it’ll create a copy and modify a 
local copy, leaving the original unchanged. That’s how file systems can appear writable without actually 
allowing writes. (In other words, a “copy-on-write” system.)

Layered systems offer two main benefits:

- 1.Duplication-Free
    Layers help avoid duplicating a complete set of files every time you use an image to create and run a 
    new container, making instantiation of Docker containers very fast and cheap.

- 2.Layer Segregation
    Making a change is much faster — when you change an image, Docker only propagates the updates to the 
    layer that was changed.

## 3.Docker Installation

### 1.Install Docker
A very detailed instructions to install Docker is provided in the below link

https://docs.docker.com/get-docker/

For Demo,

You can create an Ubuntu EC2 Instance on AWS and run the below commands to install docker.
```
sudo apt update
sudo apt install docker.io -y
```

### 2.Start Docker and Grant Access
A very common mistake that many beginners do is, After they install docker using the sudo access, they miss the step to Start the Docker daemon and grant acess to the user they want to use to interact with docker and run docker commands.

Always ensure the docker daemon is up and running.

A easy way to verify your Docker installation is by running the below command.
```
docker run hello-world
```
If the output says:
```
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```
This can mean two things,

- 1.Docker deamon is not running.
- 2.Your user does not have access to run docker commands.

### 3.Start Docker daemon
You use the below command to verify if the docker daemon is actually started and Active.
```
sudo systemctl status docker
```
If you notice that the docker daemon is not running, you can start the daemon using the below command
```
sudo systemctl start docker
```

### 4.Grant Access to your user to run docker commands
To grant access to your user to run the docker command, you should add the user to the Docker Linux group. 
Docker group is create by default when docker is installed. 
```
sudo usermod -aG docker ubuntu
```
In the above command ubuntu is the name of the user, you can change the username appropriately.

NOTE: : You need to logout and login back for the changes to be reflected.

### 5.Docker is Installed, up and running 🥳🥳
Use the same command again, to verify that docker is up and running.
```
docker run hello-world
```
Output should look like:
```
....
....
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
...
```

- Great Job, Now start with the examples folder to write your first Dockerfile and move to the next 
  examples. Happy Learning :)

### 6.Clone this repository and move to example folder
```
git clone https://github.com/balusena/docker-for-devops.git
cd  01-Docker Containers Introduction and Setup/examples
```

### 7.Login to Docker [Create an account with https://hub.docker.com/]
```
docker login
```
```
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: < Provide Your Username >
Password: < Provide Your Password >
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

### 8.Build your first Docker Image
You need to change the username accordingly in the below command.
```
docker build -t balasena21/my-first-docker-image:latest .
```
Output of  the above command:
```
    Sending build context to Docker daemon  992.8kB
    Step 1/6 : FROM ubuntu:latest
    latest: Pulling from library/ubuntu
    677076032cca: Pull complete
    Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
    Status: Downloaded newer image for ubuntu:latest
     ---> 58db3edaf2be
    Step 2/6 : WORKDIR /app
     ---> Running in 630f5e4db7d3
    Removing intermediate container 630f5e4db7d3
     ---> 6b1d9f654263
    Step 3/6 : COPY . /app
     ---> 984edffabc23
    Step 4/6 : RUN apt-get update && apt-get install -y python3 python3-pip
     ---> Running in a558acdc9b03
    Step 5/6 : ENV NAME World
     ---> Running in 733207001f2e
    Removing intermediate container 733207001f2e
     ---> 94128cf6be21
    Step 6/6 : CMD ["python3", "app.py"]
     ---> Running in 5d60ad3a59ff
    Removing intermediate container 5d60ad3a59ff
     ---> 960d37536dcd
    Successfully built 960d37536dcd
    Successfully tagged balusena21/my-first-docker-image:latest
```

### 9.Verify Docker Image is created
```
docker images
```
Output:
```
REPOSITORY                         TAG       IMAGE ID       CREATED          SIZE
balusena21/my-first-docker-image   latest    960d37536dcd   26 seconds ago   467MB
ubuntu                             latest    58db3edaf2be   13 days ago      77.8MB
hello-world                        latest    feb5d9fea6a5   16 months ago    13.3kB
```

### 10.Run your First Docker Container
```
docker run -it balusena21/my-first-docker-image
```
Output:
```
Hello World
```
### 11.Push the Image to DockerHub and share it with the world
```
docker push balusena21/my-first-docker-image
```
Output:
```
Using default tag: latest
The push refers to repository [docker.io/balusena2121/my-first-docker-image]
896818320e80: Pushed
b8088c305a52: Pushed
69dd4ccec1a0: Pushed
c5ff2d88f679: Mounted from library/ubuntu
latest: digest: sha256:6e49841ad9e720a7baedcd41f9b666fcd7b583151d0763fe78101bb8221b1d88 size: 1157
```

# You must be feeling like a champ already