# Docker Compose
Docker Compose is a powerful tool for defining and managing multi-container Docker applications. It allows 
users to describe the services, networks, and volumes required for an application in a single YAML file. By 
using this configuration file, Docker Compose can start, stop, and manage multiple containers with a single 
command (docker-compose up). This simplifies the process of setting up and running complex applications, 
making it particularly useful for development, testing, and production environments. Docker Compose enhances 
efficiency and consistency by automating the orchestration of containers, ensuring that all components of an 
application work together seamlessly.

## Lifecycle of a Docker Compose Application

1. **Define the Application**  
   Create a `docker-compose.yml` file to define the services, networks, volumes, and other configurations needed for your application. Each service represents a containerized component of your application.

2. **Build or Pull Images**  
   If custom Docker images are required, specify the build context and Dockerfile for each service in the `docker-compose.yml` file. Docker Compose will build the images for you. Alternatively, you can pull pre-built images from Docker registries.

3. **Start the Application**  
   Use the `docker-compose up` command to start the application as defined in the `docker-compose.yml` file. Docker Compose will create and start the necessary containers based on the defined services, and network them together as specified.

4. **Containers Run**  
   Once the application is started, Docker Compose runs each container in the background. Each service operates within its own isolated container, and the defined networks and volumes enable them to communicate and share data.

5. **Monitor the Application**  
   Use the `docker-compose logs` command to monitor the logs of the running containers. This helps in troubleshooting and diagnosing any issues that may arise during runtime.

6. **Scale the Application**  
   If scaling is required, Docker Compose allows you to scale services up or down easily. Use the `docker-compose up --scale service_name=N` command to scale a specific service to N instances.

7. **Stop the Application**  
   To stop the running containers and clean up resources, use the `docker-compose down` command. This command stops and removes the containers, networks, and volumes created for the application.

The lifecycle of a Docker Compose application can be repeated as needed to update or redeploy your 
application. By modifying the `docker-compose.yml` file and running the appropriate Docker Compose 
commands, you can manage the entire lifecycle of your application and its dependencies in a consistent 
and reproducible manner.

![Docker Compose LifeCycle](https://github.com/balusena/docker-for-devops/blob/main/09-Docker%20Compose/docker_compose_lifecycle.png)

- **For example**:

If your application requires both an NGINX server and a Redis database, you can create a Docker Compose 
file to manage and run both containers as services. This allows you to start and manage both services 
together without needing to start each container separately.

![Docker Compose YAML](https://github.com/balusena/docker-for-devops/blob/main/09-Docker%20Compose/docker_yaml.png)

## Benefits of Docker Compose

1. **Single host deployment**
   This means you can run everything on a single piece of hardware.

2. **Quick and easy configuration**
   Due to YAML scripts.

3. **High productivity**
   Docker Compose reduces the time it takes to perform tasks.

4. **Security**
   All the containers are isolated from each other, reducing the threat landscape.

Now, you might be thinking that Docker Compose is quite similar to Docker Swarm, but that’s not the case. 
Here are some of the differences between Docker Compose and Docker Swarm:

![Docker Compose vs Swarn](https://github.com/balusena/docker-for-devops/blob/main/09-Docker%20Compose/docker_compose_swarn.png)

- **YAML file**
  YAML(a recursive acronym for "YAML Yet Another Markup Language") is a human-readable data-serialization 
  language. It is commonly used for configuration files and in applications where data is being stored or 
  transmitted.

## Installing Docker Compose

**Step 1: Install Docker Compose**

**3 Installation Methods:**

1. **From Windows**
   (Docker Compose is already included with Docker Desktop on Windows and macOS. To check if it's installed, 
   run: `docker-compose -v`)

2. **From GitHub**  
   Download the latest version of Docker Compose from the [official GitHub releases page](https://github.com/docker/compose/releases). Follow the instructions provided there for installation.

3. **Using PIP**  
   If you have Python and PIP installed, you can install Docker Compose with the following command:
   ```
   pip install -U docker-compose
   ```
```
# 1.Install Docker Compose on Ubuntu. 

ubuntu@balasenapathi:~$ sudo apt install docker-compose
[sudo] password for ubuntu: balasenapathi

# 2.Verify Docker Compose Installation.

ubuntu@balasenapathi:~$ docker-compose -v
docker-compose version 1.25.0, build unknown

# 3.Check Docker Compose Detailed Version Information.

ubuntu@balasenapathi:~$ docker-compose version
docker-compose version 1.25.0, build unknown
docker-py version: 4.1.0
CPython version: 3.8.10
OpenSSL version: OpenSSL 1.1.1f  31 Mar 2020
```

**Step 2 : Create docker compose file at any location on your system.**
docker-compose.yml

```
# 1.Create and Edit a Docker Compose File.

ubuntu@balasenapathi:~$ mkdir DockerComposeFile

# 2.Navigate to the New Directory.

ubuntu@balasenapathi:~$ cd DockerComposeFile/

# 3.Create a Docker Compose YAML File.
ubuntu@balasenapathi:~/DockerComposeFile$ touch docker-compose.yml

# 4.Edit the Docker Compose YAML File.
ubuntu@balasenapathi:~/DockerComposeFile$ nano docker-compose.yml
version: '3'

services:
  
  web:
    image: nginx
    
  database:
    image: redis

# 5.View the Content of the Docker Compose YAML File.

ubuntu@balasenapathi:~/DockerComposeFile$ cat docker-compose.yml
version: '3'
services:

web:
image: nginx

database:
image: redis
```
**Step 3 : Check the validity of file by command**
docker-compose config

```
# 1.Validate and View Docker Compose Configuration.

ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose config
services:
database:
image: redis
web:
image: nginx
version: '3.0'
```

**Step 4: Run the Docker Compose YAML File in Detached Mode**
```
# 1.Start Docker Compose Services in Detached Mode.

ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose up -d
Creating network "dockercomposefile_default" with the default driver
Pulling database (redis:)...
latest: Pulling from library/redis
faef57eae888: Already exists
bb595d48e52d: Pull complete
d479b54c3bb2: Pull complete
2044989c541a: Pull complete
01e4ba5495fa: Pull complete
ed7a9fd4b0ea: Pull complete
Digest: sha256:08a82d4bf8a8b4dd94e8f5408cdbad9dd184c1cf311d34176cd3e9972c43f872
Status: Downloaded newer image for redis:latest
Creating dockercomposefile_database_1 ... done
Creating dockercomposefile_web_1      ... done
```

**Note**
The above command starts the services defined in the docker-compose.yml file, creating the necessary
containers and networks. The output indicates that Docker Compose has successfully pulled the required 
images and created the containers.

- Always run `docker-compose up` from the directory where the `docker-compose.yml` file is located.

```
# 2.Now check the list of containers in docker.

ubuntu@balasenapathi:~/DockerComposeFile$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS      NAMES
e749e9acfc04   nginx     "/docker-entrypoint.…"   3 minutes ago   Up 2 minutes   80/tcp     dockercomposefile_web_1
5b007ea3173f   redis     "docker-entrypoint.s…"   3 minutes ago   Up 2 minutes   6379/tcp   dockercomposefile_database_1
```

**Steps 5: Bring down application by command**

```
# 1.Stop and Remove Docker Compose Services.

ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose down
Stopping dockercomposefile_web_1      ... done
Stopping dockercomposefile_database_1 ... done
Removing dockercomposefile_web_1      ... done
Removing dockercomposefile_database_1 ... done
Removing network dockercomposefile_default
```
## Expose Host Machine Port to Web Server Port

To expose your host machine’s port "8081" to the web server’s port "80", update the `docker-compose.yml` 
file as follows:

```
# 1.Edit the Docker Compose File:**

ubuntu@balasenapathi:~/DockerComposeFile$ nano docker-compose.yml
version: '3'

services:
  
  web:
    image: nginx
    ports:
      - 8081:80
    
  database:
    image: redis

# 2.Validate and View Docker Compose Configuration.    

ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose config
services:
  database:
    image: redis
  web:
    image: nginx
    ports:
    - 8081:80/tcp
version: '3.0'

# 3.Start Docker Compose Services in Detached Mode.

ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose up -d
Creating network "dockercomposefile_default" with the default driver
Creating dockercomposefile_database_1 ... done
Creating dockercomposefile_web_1      ... done

# 4.Now check the list of containers in docker.

ubuntu@balasenapathi:~/DockerComposeFile$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
7d39b51be1e2   nginx     "/docker-entrypoint.…"   46 seconds ago   Up 43 seconds   0.0.0.0:8081->80/tcp, :::8081->80/tcp   dockercomposefile_web_1
9fcb4d31a80f   redis     "docker-entrypoint.s…"   46 seconds ago   Up 42 seconds   6379/tcp                                dockercomposefile_database_1

# 5.Now go to your localhost:8081 or 127.0.0.1:8081 

Welcome to nginx!
If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.

# 6.Stop and Remove Docker Compose Services.
ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose down
Stopping dockercomposefile_web_1      ... done
Stopping dockercomposefile_database_1 ... done
Removing dockercomposefile_web_1      ... done
Removing dockercomposefile_database_1 ... done
Removing network dockercomposefile_default

# 7.Now check the list of containers in docker.

ubuntu@balasenapathi:~/DockerComposeFile$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## Scale Services with Docker Compose

**1.Define and Run Multi-Container Applications.**

Docker Compose allows you to define and run multi-container applications. You can scale services using 
the `--scale` option.

**2.Scale the Database Service.**

To scale the database service (Redis) to 4 instances, use the following command:

```
ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose up -d --scale database=4
```
- Note: 
  In this example, you are scaling the Redis database service to 4 containers, while the Nginx web service
  remains unchanged.

**3.View the Running Containers.**

After scaling, check the running containers:

```
ubuntu@balasenapathi:~/DockerComposeFile$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS       NAMES
7f059f552b0f   redis     "docker-entrypoint.s…"   About a minute ago   Up 59 seconds       6379/tcp   dockercomposefile_database_3
c79343045fcb   redis     "docker-entrypoint.s…"   About a minute ago   Up About a minute   6379/tcp   dockercomposefile_database_1
9cb418395d69   redis     "docker-entrypoint.s…"   About a minute ago   Up 59 seconds       6379/tcp   dockercomposefile_database_4
8503a7bc4494   redis     "docker-entrypoint.s…"   About a minute ago   Up About a minute   6379/tcp   dockercomposefile_database_2
1266410cf4dc   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8081->80/tcp, :::8081->80/tcp   dockercomposefile_web_1

```
**4.Stop and Remove Containers.**

To stop and remove all containers and networks, use the following command:

```
ubuntu@balasenapathi:~/DockerComposeFile$ docker-compose down
Stopping dockercomposefile_database_3 ... done
Stopping dockercomposefile_database_1 ... done
Stopping dockercomposefile_database_4 ... done
Stopping dockercomposefile_database_2 ... done
Stopping dockercomposefile_web_1      ... done
Removing dockercomposefile_database_3 ... done
Removing dockercomposefile_database_1 ... done
Removing dockercomposefile_database_4 ... done
Removing dockercomposefile_database_2 ... done
Removing dockercomposefile_web_1      ... done
Removing network dockercomposefile_default
```
**5.Verify All Containers Are Stopped.**

After running docker-compose down, check to ensure no containers are running:

```
ubuntu@balasenapathi:~/DockerComposeFile$  docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
**Summary**
Docker Compose streamlines the management of multi-container Docker applications by allowing users to 
define services, networks, and volumes in a single YAML file. It automates the setup, scaling, and 
teardown of complex applications, ensuring a consistent environment across development and production. 
By simplifying these tasks, Docker Compose enhances efficiency and reduces configuration errors, making 
it easier to manage and deploy complex applications with minimal effort. This approach fosters a more 
organized and scalable development process.






















