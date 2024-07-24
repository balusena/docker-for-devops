# Docker Images
A Docker image contains application code,libraries,tools,dependencies and other files needed to make an
application run.When a user runs an image,it can become one or many instances of a container.Docker images
have multiple layers,each one originates from the previous layer but is different from it.

- 1.Docker images are read-only templates used to build containers.
- 2.Containers are deployed instances created from those templates.
- 3.Images and containers are closely related, and are essential in powering the Docker software platform.

Note: Docker images are read-only templates used to create containers, built by Docker users, and stored in
Docker Hub or your local registry.
![Docker Image Layer](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/container_layers.png)

- An image consists of a collection of files (or layers) that pack together all the necessities — such as 
  dependencies, source code, and libraries
- A Docker image is a read-only template that contains a set of instructions for creating a container that 
  can run on the Docker platform.
- A Docker Image is a set of layers where each layer represents an instruction from the Dockerfile.
- It provides a convenient way to package up applications.
- Each new layer is only a set of differences from the previous one.
- Docker creates container images using layers. Each command that is found in a Dockerfile creates a new layer.

## Docker Layer Caching (DLC)
- Each command that is found in a Dockerfile creates a new layer.
- Docker uses a layered cache to optimize the process of building Docker images and make it faster.
- At each occurrence of a RUN, COPY and ADD command in the Dockerfile, Docker will create and commit a new
  layer to the image that comprises a Docker image.

Let’s take the example of building a docker file in three states.

- Running DockerFile first time for building docker image:
  During a new build, all of these file structures have to be created and written to disk — this is where 
  Docker stores base images.
![Running DockerFile first time for building docker image](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/df1.png)


