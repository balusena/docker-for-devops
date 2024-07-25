# Docker Registry
### 1.What Is a Docker Registry?
Docker registry is a server-side platform used by software developers to create, store, manage, and distribute
anywhere different reusable versions of Docker images. Docker images are standalone executables that include 
everything needed to run an application or service.

The images are then used as a template to instantiate (i.e., generate) a container (i.e., lightweight packages 
of application code and its dependencies) that will be utilized to run an application process or a service. 
Typically used to build containerization-based applications (i.e., apps comprised of independent components) 
and services, a Docker registry is a bit like a warehouse shelf filled with Docker images instead of goods.

![Docker Registry](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/docker_registry_image_container_relation.png)

Each Docker registry is divided into sections (i.e., Docker repositories), just like a warehouse shelf is 
divided into different sections containing different products. Each Docker repository includes various 
versions (i.e., tags) of a single Docker image and a description.

![Ubuntu Docker Image](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/ubuntu_docker.png)

![Ubuntu Docker Image Versions](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/official_ubuntu_docker_repository_versions.png)