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
$ docker create --name my-container my-nginx-image
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
$ docker container start my-container
my-container
```
- When we start a container, Docker sets up the resources that the container needs, such as network
  resources, memory, CPU, etc. Then, it creates an environment for the container to run in.

- Once this is done, the container is up and running and begins to perform the tasks assigned to it.

The task done by the above two commands can be achieved using a single command also (`docker run`). 
This command creates the container and then starts it immediately.
```
docker run -d --name my-container my-nginx-image
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