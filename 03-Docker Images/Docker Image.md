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
