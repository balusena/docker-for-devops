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

### 2.Why Docker Registries Are Useful
According to Gartner, 65.9% of the money spent by organizations on traditional application software will 
be redirected to cloud-based products and services by 2025. With businesses shifting to cloud-native 
development and hybrid multi-cloud environments, Docker is becoming increasingly popular.

Anyone who has access to the Docker registry can upload an image, search for it, or download it. For example:

- Software developers access the Docker registry to select the Docker images they need to build containers.

- While application consumers use it to choose the Docker images needed to run services or web applications.

Why do you need it? Because it provides you application flexibility and portability. A Docker registry will
also extremely simplify and automate your deployment process, even for the most complex applications.

Do you remember when we said that the Docker registry is like a warehouse shelf? Good. Now, think about the
challenges a warehouse manager may face.I can tell you that the issues you get are very similar to the issues you encounter in 
application development. For example:

- How can you identify and/or prevent tampering with the products before they reach the stores?

- How do you protect your products against counterfeits?

- What process can you follow to deal with faulty products?

- How can you limit access to the products and the warehouse to a restricted number of people of your choice?

Depending on the Docker registry type you choose, it may also protect application consumers from
man-in-the-middle attacks and guarantee the integrity of your images.

Does it sound familiar? I bet it does. And guess what? The right Docker registry type will help you deal 
with these issues.

Moreover, it also:

- Offers a centralized location for your images. Teams can easily store, search, manage, and share public
  and private images (select or all versions).

- Guarantees an enhanced level of security. 55% of the respondents to the 2022 Kubernetes and cloud-native
  operations survey, confirmed that the security of container-based images is the top criteria for image selection. Docker private Registry and Docker Trusted Registry (more on these in a minute) can fulfill that requirement through access control and image signing.

- Saves time and resources. Thanks to automation, flexible version management, and integration with the 
  development workflow.

- It’s easily integrated with other tools and scalable. A Docker Registry can integrate with tools like 
  Kubernetes, GitHub, and Jenkins. The advantage? For example, images created from the codes tested in 
  GitHub can be added to Docker Registry to tackle inconsistencies and bugs.

Now that we know what benefits Docker registries offer, let’s find out how they work.

### 3.How Does a Docker Registry Work?
Before we get into the nitty-gritty of the different Docker registry types available, we have to first 
understand how a Docker registry works. Let’s say you’re a developer who wants to build a containerized 
application. To create a container, you’ll usually:

   - 1.Create a DockerFile. This is a text file with a standardized command-line interface (CLI) instructions
       to automatically build a Docker image.

   - 2.Build and save Docker images. Each image is made up of layers, and each layer represents one specific
       version of each image.

   - 3.Upload your Docker images to the Docker registry. This is where the Docker registry comes into play. 
       Once the images are created, you’ll need a place to store and manage them.

![Create Docker Image And Upload To Docker registry](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/create_docker_image_and_upload_to_docker_registry.png)

That’s it. Now, other users can access the Docker registry and search and/or download the image you’ve just 
uploaded to use for their development project or to run as a container.

![The Role of Docker Registry In Image Storage](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/the_role_of_docker_registry_in_image_storage.png)













