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
   

