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

### 1.Running DockerFile first time for building docker image:

During a new build, all of these file structures have to be created and written to disk, this is where 
Docker stores base images.

![Running DockerFile first time for building docker image](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/df1.png)

### 2.Running DockerFile second time for building docker image:

- When we build the same dockerfile a second time, it will reuse the cache of a previous build on the host 
  to reduce the build time.
- None of those file structures have to be created and written to disk at this time.

![Running DockerFile second time for building docker image](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/df2.png)

### 3.Running DockerFile with no-cache option:
- However, that cache may cause issues sometimes.
- To solve this issue, we need to invalidate the cache on the docker host.
- Docker build with the “— no-cache” option, which completely ignores all cache and thus makes every build take as much time as the first.
```
- e.g — docker build — no-cache -t nginx1 .
```
![Running DockerFile with no-cache option ](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/df3.png)

## Docker Image Size
- When you execute docker image ls or docker images, the column size is the size of the docker image.
- It is also not necessary that it will be the size of the current image but it may be the size of the 
  current image + all these parent images.

![Docker Image Size](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/docker_image_size.png)

## An Example of a docker image
- Here is a MySQL image that has been downloaded from Docker Hub.
- A Docker image is made up of multiple layers. The MySQL image above consists of a series of layers such as
  07aded7c29c6, f68b8cbd22de, etc, and others.

![Docker Image Example](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/docker_image.png)

![Docker Image Example](https://github.com/balusena/docker-for-devops/blob/main/03-Docker%20Images/doc_image.png)

- Tag (latest) —It identifies the image by its tag, such as version number.
- Image ID (2fe463762680) — It is a unique image identity.
- Created (4 days ago) — It is the period of time since it was created.
- Size (514MB) — It is the image’s virtual size.

## Docker Image basics command

### 1.To get the list of all images in our system:-
```
ubuntu@balasenapathi:~$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    9c7a54a9a43c   3 weeks ago   13.3kB
```
### 2.To pull an image from dockerhub registry or repository:-
```
ubuntu@balasenapathi:~$ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
dbf6a9befcde: Pull complete
Digest: sha256:dfd64a3b4296d8c9b62aa3309984f8620b98d87e47492599ee20739e8eb54fbf
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```
### 3.To use docker images help
```
ubuntu@balasenapathi:~$ docker images --help
Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
-a, --all             Show all images (default hides intermediate images)
--digests         Show digests
-f, --filter filter   Filter output based on conditions provided
--format string   Pretty-print images using a Go template
--no-trunc        Don't truncate output
-q, --quiet           Only show image IDs
```
```
ubuntu@balasenapathi:~$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    9c7a54a9a43c   3 weeks ago   13.3kB
ubuntu        latest    3b418d7b466a   5 weeks ago   77.8MB
```
### 4.To get only image ids
```
ubuntu@balasenapathi:~$ docker images -q
9c7a54a9a43c
3b418d7b466a
```
### 5.To filter an image based on condition
```
Note:- A dangling image is one that is not tagged and is not referenced/associated by any container.
-f ------> filter option

ubuntu@balasenapathi:~$ docker images -f "dangling=false"
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    9c7a54a9a43c   3 weeks ago   13.3kB
ubuntu        latest    3b418d7b466a   5 weeks ago   77.8MB

ubuntu@balasenapathi:~$ docker images -f "dangling=true"
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

ubuntu@balasenapathi:~$ docker images -f "dangling=false" -q
9c7a54a9a43c
3b418d7b466a
```
### 6.To create a docker container from a docker image
```
ubuntu@balasenapathi:~$ docker run -it -d ubuntu
f830a3324f7d379dcfa9fb8b1ae03c4fa1fa2f178520d3811ab3911fffeacf6b
a92629e82e88
e47579e053e6
```
### 7.To provide name to the container
```
ubuntu@balasenapathi:~$ docker run --name MyUbuntu -it -d ubuntu
e47579e053e6f49da44c9dc2e85ac5c3ca7ae99183a8fd879c7bcac44db579fc
```
### 8.To access a running docker container:-
```
ubuntu@balasenapathi:~$ docker exec -it f830a3324f7d bash
root@f830a3324f7d:/# pwd
/
root@f830a3324f7d:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@f830a3324f7d:/# exit
exit

===> docker pull is used to download an image from a registry to your local machine. [$ docker pull ubuntu:latest]
===> docker run is used to create and start a container based on a specified image.  [$ docker run -it ubuntu:latest]
===> docker exec is used to execute a command within a running container.            [$ docker exec -it f830a3324f7d bash]
```
### 9.To delete docker images:-
```
docker rmi <IMAGE ID>    -----> get IMAGE ID by using > docker images

ubuntu@balasenapathi:~$ docker rmi 3b418d7b466a
Error response from daemon: conflict: unable to delete 3b418d7b466a (cannot be forced) - image is being used by running container
f830a3324f7d
```
- Note: 
If any image was used by docker container then we need to stop that container to delete that image 
else we get an error, or we can use -f to remove the image forecfully.

### This image was used by two containers ids f830a3324f7d, e47579e053e6 created from image id 3b418d7b466a
```
ubuntu@balasenapathi:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED          STATUS                  PORTS     NAMES
e47579e053e6   ubuntu        "/bin/bash"   28 minutes ago   Up 28 minutes                     MyUbuntu
f830a3324f7d   ubuntu        "/bin/bash"   30 minutes ago   Up 30 minutes                     laughing_kare
bc83ae06143b   hello-world   "/hello"      5 days ago       Exited (0) 5 days ago             friendly_colden

ubuntu@balasenapathi:~$ docker rmi -f 3b418d7b466a
Error response from daemon: conflict: unable to delete 3b418d7b466a (cannot be forced) - image is being used by running container
f830a3324f7d
```
### To resolve this issue stop the running container
```
ubuntu@balasenapathi:~$ docker stop f830a3324f7d
f830a3324f7d

ubuntu@balasenapathi:~$ docker rmi -f 3b418d7b466a
Error response from daemon: conflict: unable to delete 3b418d7b466a (cannot be forced) - image is being used by running container
e47579e053e6

ubuntu@balasenapathi:~$ docker stop e47579e053e6
e47579e053e6

ubuntu@balasenapathi:~$ docker rmi -f 3b418d7b466a
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:dfd64a3b4296d8c9b62aa3309984f8620b98d87e47492599ee20739e8eb54fbf
Deleted: sha256:3b418d7b466ac6275a6bfcb0c86fbe4422ff6ea0af444a294f82d3bf5173ce74
```
### 10.To inspect docker image
```
docker inspect <image_name|ID>

ubuntu@balasenapathi:~$ docker inspect 9c7a54a9a43c

or

ubuntu@balasenapathi:~$ docker inspect hello-world
[
{
"Id": "sha256:9c7a54a9a43cca047013b82af109fe963fde787f63f9e016fdc3384500c2823d",
"RepoTags": [
"hello-world:latest"
],
"RepoDigests": [
"hello-world@sha256:fc6cf906cbfa013e80938cdf0bb199fbdbb86d6e3e013783e5a766f50f5dbce0"
],
"Parent": "",
"Comment": "",
"Created": "2023-05-04T17:37:03.872958712Z",
"Container": "347ca68872ee924c4f9394b195dcadaf591d387a45d624225251efc6cb7a348e",
"ContainerConfig": {
"Hostname": "347ca68872ee",
"Domainname": "",
"User": "",
"AttachStdin": false,
"AttachStdout": false,
"AttachStderr": false,
"Tty": false,
"OpenStdin": false,
"StdinOnce": false,
"Env": [
"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
],
"Cmd": [
"/bin/sh",
"-c",
"#(nop) ",
"CMD [\"/hello\"]"
],
"Image": "sha256:62a15619037f3c4fb4e6ba9bd224cba3540e393a55dc52f6bebe212ca7b5e1a7",
"Volumes": null,
"WorkingDir": "",
"Entrypoint": null,
"OnBuild": null,
"Labels": {}
},
"DockerVersion": "20.10.23",
"Author": "",
"Config": {
"Hostname": "",
"Domainname": "",
"User": "",
"AttachStdin": false,
"AttachStdout": false,
"AttachStderr": false,
"Tty": false,
"OpenStdin": false,
"StdinOnce": false,
"Env": [
"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
],
"Cmd": [
"/hello"
],
"Image": "sha256:62a15619037f3c4fb4e6ba9bd224cba3540e393a55dc52f6bebe212ca7b5e1a7",
"Volumes": null,
"WorkingDir": "",
"Entrypoint": null,
"OnBuild": null,
"Labels": null
},
"Architecture": "amd64",
"Os": "linux",
"Size": 13256,
"VirtualSize": 13256,
"GraphDriver": {
"Data": {
"MergedDir": "/var/snap/docker/common/var-lib-docker/
overlay2/756079e8163496cd40f9f9ad27e4c0ab18548c303b3f2aad321fbe35701c7c65/merged",
"UpperDir": "/var/snap/docker/common/var-lib-docker/
overlay2/756079e8163496cd40f9f9ad27e4c0ab18548c303b3f2aad321fbe35701c7c65/diff",
"WorkDir": "/var/snap/docker/common/var-lib-docker/
overlay2/756079e8163496cd40f9f9ad27e4c0ab18548c303b3f2aad321fbe35701c7c65/work"
},
"Name": "overlay2"
},
"RootFS": {
"Type": "layers",
"Layers": [
"sha256:01bb4fce3eb1b56b05adf99504dafd31907a5aadac736e36b27595c8b92f07f1"
]
},
"Metadata": {
"LastTagTime": "0001-01-01T00:00:00Z"
}
}
]
```
- Note:
We will get all the detailed information about the inspected image. An image typically contains a union of 
layered filesystems stacked on top of each other.

## Essential operations associated with Docker Images for effective operations in both development and production environments

### 1.Pulling
- Pull an image from a registry:
```
$ docker pull ubuntu:latest
```
- Pull a specific version of an image:
```
$ docker pull nginx:1.19.10
```
### 2.Building
- Build an image from a Dockerfile in the current directory:
```
$ docker build -t myimage:tag .
```
- Build an image with a specific Dockerfile:
```
$ docker build -t myimage:tag -f Dockerfile.dev .
```
### 3.Listing
- List all available images on your local machine:
```
$ docker images
```
- List images with a specific repository and tag:
```
$ docker images myrepository/myimage:tag
```
#4.Running
- Run a container based on an image in the background:
```
docker run -d -p 8080:80 nginx:latest
```
- Run a container interactively and attach to it:
```
docker run -it ubuntu:latest bash
```
### 5.Inspecting
- Inspect detailed information about an image:
```
$ docker image inspect ubuntu:latest
```
### 6.Tagging
- Tag an image with a new repository and tag:
```
$ docker tag myimage:tag myrepository/myimage:tag
```
### 7.Managing
- Remove an image from your local machine:
```
$ docker rmi myimage:tag
```
- Remove multiple images at once:
```
$ docker rmi image1:tag image2:tag
```
- Remove all unused images:
```
$ docker image prune
```
- Save an image to a tar archive:
```
$ docker save -o myimage.tar myimage:tag
```
- Load an image from a tar archive:
```
$ docker load -i myimage.tar
```
These commands will help us effectively work with Docker images in various scenarios, both in development and
production environments.















