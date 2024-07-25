# Docker Lifecycle
Docker has become a widely used method for packaging and running applications. To work with Docker efficiently,
it is essential to comprehend the various stages of a container's existence, which is called its `lifecycle`.
The Docker container lifecycle is comprised of several states, such as `created`, `running`, `paused`, `exited`, 
and `dead`. In this repo we will examine each of these states in detail and explore what they represent and
when a container enters each state. By understanding these stages, we will have a better grasp of how to work 
with Docker containers.


![Docker Lifecycle](https://github.com/balusena/docker-for-devops/blob/main/05-Docker%20Lifecycle/docker_lifecycle.png)

## Summary of these states before we deep dive into it:
### 1.created:
This is the initial state of a Docker container after it has been created, but before it has been started.

### 2.running:
When a Docker container is started, it transitions to the running state. In this state, the container is 
actively executing its processes.

### 3.paused:
If a container is paused, it is temporarily stopped from running its processes, but it is not terminated.

### 4.exited:
If a container's main process completes, the container stops and transitions to the exited state.

### 5.dead:
If a container fails to start, it is in the dead state. Containers in this state cannot be restarted and 
must be recreated.

## Container States in detail

### 1.Created:
This is the first state in the container lifecycle. This state denotes that the container is created but 
it's not running. This is achieved using the `docker create` command.
```
ubuntu@balasenapathi:~$ docker create --name my-container my-nginx-image
e21a7fc8a3e5b1e0f1a8c42e287cb9c1b8b564e3c2c74df0f82f558d3e9d62d1
```
- When we create a Docker container, a read-write (R/W) layer is added to the read-only (R/O) layer of the 
  image we've chosen. This prepares the container to run the program like pulling the image, setup the 
  environment variables, setup entrypoint, etc.

- It's important to note that creating a container does not automatically start the program. However, we can
  set all the necessary configuration parameters during the creation process, such as CPU and memory limits,
  container image, and capabilities. When the container is in this state, the configuration of the container
  can also be updated using the `docker update` command.

- This means that we can create the container once with all the required parameters and start it later 
  without having to specify them again.

- Another thing to note here is that resources are not allocated in this state.

### 2.Started/Running
This state denotes that the commands listed in the image are being executed one by one by the container.
```
ubuntu@balasenapathi:~$ docker container start my-container
my-container
```
- When we start a container, Docker sets up the resources that the container needs, such as network
  resources, memory, CPU, etc. Then, it creates an environment for the container to run in.

- Once this is done, the container is up and running and begins to perform the tasks assigned to it.

The task done by the above two commands can be achieved using a single command also (`docker run`). 
This command creates the container and then starts it immediately.
```
ubuntu@balasenapathi:~$ docker run -d --name my-container my-nginx-image
e21a7fc8a3e5b1e0f1a8c42e287cb9c1b8b564e3c2c74df0f82f558d3e9d62d1
```
### 3.Killed / Exited
The `exited` state refers to a container that has stopped running. A container can transition to the exited 
state for a variety of reasons, including:

   - 1.The process running inside the container completed its task and exited.

   - 2.The process running inside the container was stopped by a user or an external signal.

   - 3.An error occurred while running the process inside the container.

The killed state is a sub-state of the exited state. When a container is killed, it means that the process 
inside the container was forcibly terminated by Docker. This can happen if a user runs the `docker kill` 
command, or if a container fails to respond to a SIGTERM signal and Docker sends a SIGKILL signal to force
the container to stop.

- Command to stop a docker container:
```
ubuntu@balasenapathi:~$ docker container stop my-container
my-container
```
When we run `docker stop` command, it goes through the following steps:
   - Docker sends a SIGTERM signal to the main process (PID 1) running inside the container. This signal is
     a request for the process to stop gracefully.

   - If the process does not respond to the SIGTERM signal within a set amount of time (10 seconds by default
     , to override use the `-t` flag), Docker sends a SIGKILL signal to forcefully terminate the process.

   - Once the process is stopped, Docker moves the container to the exited state.

   - If the container was started with the --rm flag, Docker will remove the container and its filesystem 
     from the system.
Overall, running the docker stop command allows us to gracefully stop a container and cleanly shut down 
the program or process running inside it.

### 4.Paused
- The `paused` state refers to a container that has been temporarily suspended. When a container is paused,
  it is still running, but all of its processes are paused and no new processes can be started until the 
  container is unpaused.

- Pausing a container can be useful when we need to temporarily free up resources on the host system or when
  we need to troubleshoot an issue with the container. When a container is paused, it continues to consume
  resources such as memory and CPU, but at a much lower rate than when it is running normally.

- Pausing the container still holds the state of the execution in the memory, and on unpausing it will 
  resume from the same point. For e.g. if my docker container counts from 1 to 100, and if I pause it at 
  any point, on resuming it will resume from that point. When it's in paused the CPU usage would be almost
  0 but memory usage would be non-zero.

- To pause a running container, you can use the docker pause command followed by the container ID or name.
  To unpause a paused container, you can use the docker unpause command followed by the container ID or name.
```
ubuntu@balasenapathi:~$ docker pause my-container
ubuntu@balasenapathi:~$ docker unpause my-container
```
It's important to note that not all containers can be paused. Containers that are running in privileged 
mode or with certain types of system capabilities cannot be paused. Additionally, pausing a container 
that is running a critical process can have unintended consequences, so it's important to use caution when
pausing containers in production environments.

### 5.Deleted
- There's no official state as Deleted, as in this state the container is already removed.

- The `deleted` state refers to a container that has been removed from the Docker host system. When a 
  container is deleted, all of its resources, including its filesystem and configuration, are permanently
  removed from the host system.

- Deleting a container can be done using the docker rm command followed by the container ID or name. When
  a container is deleted, it cannot be restarted or resumed, and any data or changes that were made to the
  container are lost.
```
ubuntu@balasenapathi:~$ docker container rm my-container
my-container
```
It's important to note that deleting a container does not remove the image that was used to create the 
container. The image can still be used to create new containers, and any changes made to the image will 
be reflected in new containers that are created from that image.

### 6.Dead
When a Docker container cannot be removed due to some resources still being in use by an external process,
it is moved to the dead state. In this state, the container becomes non-functional and cannot be restarted.
It can only be removed. Although it is partially removed, it does not consume any memory or CPU.
```
ubuntu@balasenapathi:~$ docker container rm my-container
Error response from daemon: Removal failed: my-container: cannot be removed, it is in a dead state
```

## Docker commands associated with the container lifecycle, covering various scenarios:

### 1.Image Lifecycle
- 1.Build an Image:
```
ubuntu@balasenapathi:~$ docker build -t my-nginx-image .
```
Builds an image named my-nginx-image from the Dockerfile in the current directory.

- 2.List Images:
```
ubuntu@balasenapathi:~$ docker images
```
Lists all Docker images on the system.

- Remove an Image:
```
ubuntu@balasenapathi:~$ docker rmi my-nginx-image
```
Removes the image named my-nginx-image.

### 2.Container Creation
- 1.Create a Container:
```
ubuntu@balasenapathi:~$ docker create --name my-container my-nginx-image
```
Creates a container named my-container from the my-nginx-image image but does not start it.

- 2.Run a Container:
```
ubuntu@balasenapathi:~$ docker run -d --name my-running-container -p 8080:80 my-nginx-image
```
Creates and starts a container named my-running-container from my-nginx-image, mapping port 80 in the 
container to port 8080 on the host.

### 3.Running Containers
- 1.Start a Container:
```
ubuntu@balasenapathi:~$ docker start my-container
```
Starts a previously created (but stopped) container named my-container.

- 2.Stop a Container:
```
ubuntu@balasenapathi:~$ docker stop my-running-container
```
Stops the running container named my-running-container.

- 3.Pause a Container:
```
ubuntu@balasenapathi:~$ docker pause my-running-container
```
Pauses all processes in the container named my-running-container.

- 4.Unpause a Container:
```
ubuntu@balasenapathi:~$ docker unpause my-running-container
```
Unpauses the container named my-running-container.

- 5.Restart a Container:
```
ubuntu@balasenapathi:~$ docker restart my-running-container
```
Restarts the container named my-running-container.

### 4.Monitoring and Managing Containers
- 1.List Running Containers:
```
ubuntu@balasenapathi:~$ docker ps
```
Lists all running containers.

- 2.List All Containers:
```
ubuntu@balasenapathi:~$ docker ps -a
```
Lists all containers, including those that are stopped.

- 3.View Container Logs:
```
ubuntu@balasenapathi:~$ docker logs my-running-container
```
Fetches logs from the container named my-running-container.

- 4.Inspect a Container:
```
ubuntu@balasenapathi:~$ docker inspect my-running-container
```
Displays detailed information about the container named my-running-container.

### 5.Interacting with Containers
- 1.Execute a Command in a Running Container:
```
ubuntu@balasenapathi:~$ docker exec -it my-running-container /bin/bash
```
Opens an interactive shell (/bin/bash) inside the running container named my-running-container.

- 2.Attach to a Running Container:
```
ubuntu@balasenapathi:~$ docker attach my-running-container
```
Attaches to the container’s main process.

### 6.Stopping and Removing Containers
- 1.Stop and Remove a Container:
```
ubuntu@balasenapathi:~$ docker rm -f my-container
```
Forces removal of the container named my-container, stopping it if it is running.

- 2. Remove a Container:
```
ubuntu@balasenapathi:~$ docker rm my-stopped-container
```
Removes the container named my-stopped-container, which must be stopped first.

### 7.Container Cleanup
- 1.Remove Unused Containers:
```
ubuntu@balasenapathi:~$ docker container prune
```
Removes all stopped containers.

- 2.Remove Unused Images:
```
ubuntu@balasenapathi:~$ docker image prune
```
Removes dangling images (images not referenced by any container).

- 3.Remove Unused Networks:
```
ubuntu@balasenapathi:~$ docker network prune
```
Removes unused networks.

- 4.Remove Unused Volumes:
```
ubuntu@balasenapathi:~$ docker volume prune
```
Removes unused volumes.

### 8.System Maintenance
- 1.Clean Up System:
```
ubuntu@balasenapathi:~$ docker system prune
```
Removes all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes.




















