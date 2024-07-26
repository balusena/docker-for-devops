# Docker Volumes
Docker volumes provide persistent storage for Docker containers. Volumes can be attached to containers, 
allowing data to be shared and stored outside the container's lifecycle. Volumes are used to manage data
that needs to be retained even if the container is deleted or replaced. This makes them ideal for maintaining
important data across container deployments.

![Docker Volumes](https://github.com/balusena/docker-for-devops/blob/main/07-Docker%20Volumes/docker_volumes.png)

## When Do We Need Docker Volumes
- Use Case:

Consider a scenario where a container running a MySQL database has its data stored at /var/lib/mysql/data 
within the container’s virtual filesystem. If the container is removed or restarted without persistence 
mechanisms in place, the data within the container's virtual filesystem will be lost. This results in a loss
of state and historical data, which is unacceptable for databases and other stateful applications where data
integrity and continuity are crucial.

To address this, Docker volumes are used. Volumes are managed outside the container’s lifecycle and are 
stored in the host’s filesystem. By using volumes, data can be preserved even when containers are stopped,
started, or removed. This ensures that the database or application data remains intact, allowing the 
application to maintain its state and operate effectively over time.

#### Example to show when we use Docker Volumes:

### 1.Database Container:

- Without Volume: Data stored in the container’s virtual filesystem is lost if the container is removed.

- With Volume: Data is stored in a Docker-managed volume, which persists across container removals and restarts.

### 2.Stateful Applications:

- Without Volume: Application state and data can be lost on container restarts or removals.

- With Volume: Application state is preserved by mounting a Docker volume, ensuring continuity and data retention.

Using Docker volumes, you can ensure that crucial data is preserved, enhancing the reliability and 
stability of applications that rely on persistent storage.

Note: CONTAINER sits in HOST operating system(OS)

- HOST [ Physical File System ] path     ===> /home/mount/data

- CONTAINER [ Virtual File System ] path ===> /var/lib/mysql/data 

## What is a Docker Volume?
Docker volumes are a mechanism to manage persistent data in Docker containers. They allow data to be stored
outside of the container's lifecycle, ensuring that data is preserved even when containers are stopped, 
restarted, or removed.

### 1.Volume Mapping:

- Host Filesystem Path: /home/mount/data

- Container Filesystem Path: /var/lib/mysql/data

- Docker volumes work by mapping a directory from the host's physical filesystem to a directory in the 
  container's virtual filesystem. For example, a directory on the host (/home/mount/data) can be mounted 
  to a directory inside the container (/var/lib/mysql/data).

### 2.Data Synchronization:

- When a container writes data to its virtual filesystem path (/var/lib/mysql/data), this data is 
  automatically reflected in the host's physical filesystem path (/home/mount/data).

- Conversely, any changes made directly to the host directory (/home/mount/data) are immediately visible to
  the container's virtual filesystem path (/var/lib/mysql/data).

This synchronization ensures that data is shared between the host and the container, maintaining 
persistence across container operations.

### 3.Data Persistence:

- Docker volumes ensure that data stored in the mounted directories persists even when containers are 
  started, stopped, restarted, or deleted.

- This is essential for applications that require data continuity, such as databases and other stateful
  applications.

By using Docker volumes, you can manage and preserve data efficiently, providing a seamless experience 
for applications that need reliable and persistent storage.

## Docker Volume Types
Docker supports 3 different types of volumes for handling persistent data. Each type serves specific use 
cases and has distinct management characteristics. Here are the main types of Docker volumes:

### 1.Host Volumes (Bind Mounts)
#### Definition: 
Uses a directory or file from the host filesystem and mounts it into the container.
#### Management: 
Managed by the host system.
#### Use Case: 
When you need direct access to host files, such as configuration files or code repositories.

```
# 1.Run a container with a bind mount.

ubuntu@balasenapathi:~$ docker run -d -v /path/on/host:/path/in/container --name my-container nginx
```
- /path/on/host      ====> your host machine (physical file) path ====> /home/mount/data

- /path/in/container ====> your container (virtual file) path     ====> /usr/share/nginx/html

Note: Changes to /path/on/host on the host are reflected in /path/in/container inside the container, and vice versa.

### 2.Anonymous Volumes
#### Definition: 
Volumes that Docker creates automatically when the -v option is used without specifying a name.
#### Management: 
Managed by Docker, but the volume is given a random name.
#### Use Case: 
Temporary storage for data that doesn't need to be named or reused after the container is removed.

```
# 2.Run a container with an anonymous volume.

ubuntu@balasenapathi:~$ docker run -d -v /path/in/container --name my-container nginx

```
Note: Docker automatically creates a volume and mounts it to /path/in/container inside the container.

### 3.Named Volumes (Managed Volumes)
#### Definition: 
Volumes that you explicitly create and name using Docker commands.
#### Management: 
Managed by Docker and can be easily referenced and reused by name.
#### Use Case: 
Persistent storage that needs to be retained across multiple container instances and easily shared among containers.

```
# 3.Create a named volume.

docker volume create my-named-volume

# 3.Run a container with the named volume.

docker run -d -v my-named-volume:/path/in/container --name my-container nginx
```

Note: The named volume my-named-volume is used for /path/in/container, and data persists even if the container is removed.

![Docker Volume Types](https://github.com/balusena/docker-for-devops/blob/main/07-Docker%20Volumes/docker_volume_types.png)

### Important Points about Docker Volumes

#### 1.Definition: 
A volume is a directory outside of the container's filesystem that can be mounted inside containers.

#### 2.Declaration: 
You must declare a directory as a volume and then share it.

#### 3.Persistence: 
Volumes persist data even if the container is stopped.

#### 4.Creation: 
Volumes are created independently of containers and can be managed separately.

#### 5.Declaration Timing: 
You declare a directory as a volume when creating a container.

#### 6.Limitations: 
You cannot create a new volume from within an existing container.

#### 7.Sharing: 
One volume can be shared across any number of containers.

#### 8.Image Updates: 
Volumes are not included when updating an image. They remain independent of the image lifecycle.

### Benefits of Volumes

#### 1.Decoupling Storage: 
Volumes decouple storage from the container, allowing data to persist independently of the container's lifecycle.

#### 2.Sharing: 
Volumes can be shared among different containers, facilitating data sharing and coordination.

#### 3.Attachment: 
Volumes can be easily attached and detached from containers, providing flexible data management.

#### 4.Persistence: 
Volumes are not deleted when a container is deleted, ensuring data persistence and stability.

## Mapping Volumes in Docker
This can be done in two ways:

### 1.Container to Container: 
Volumes can be shared between containers, allowing multiple containers to access and modify the same 
data. This is particularly useful for shared configurations, databases, or other data that multiple 
services need to access and manipulate. Sharing volumes in this way facilitates seamless communication 
and data consistency between related containers.

### 2.Host to Container: 
Volumes can map directories from the host filesystem to the container, ensuring persistent data storage
that survives container restarts and deletions. This setup allows data to be easily accessed, managed, 
and backed up from the host system, providing a robust solution for maintaining critical application data.

### Example for mapping Volumes in Docker:

#### 1.Container to Container: 
Volumes can be shared between containers, allowing multiple containers to access and modify the same data.

- Note: Before creating a container see the list of docker images

```
# 1.To see the list of docker images.

ubuntu@balasenapathi:~$ docker images
REPOSITORY           TAG               IMAGE ID       CREATED        SIZE
ubuntu               latest            5a81c4b8502e   7 days ago     77.8MB
balusena/money_api   latest            1665bedba94e   2 weeks ago    626MB
python               3.8-slim-buster   52456bc7bf3e   4 weeks ago    118MB
hello-world          latest            9c7a54a9a43c   2 months ago   13.3kB
```
- Note we will use "ubuntu" official docker image for our demonstration purpose.

#### 1.Now create a container1 with volume in it:

```
# 1.Run the Docker Container1 with Volume Mount.

ubuntu@balasenapathi:~$ docker run -it --name container1 -v /myvolume ubuntu sh

# 2.List Directories in the Container1.

root@8ffe7b843b01:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory in container1.

root@8ffe7b843b01:/# cd myvolume

# 4.List Contents of the Mounted Volume in container1.

root@8ffe7b843b01:/# ls
```

#### 2.Now create another container2 using volumes from container1:

```
# 1.Run a Docker Container2 with Volume from Container1.

ubuntu@balasenapathi:~$ docker run -it --name container2 --volumes-from container1 ubuntu sh

# 2.List Directories in the Container2.

root@6rvc9s246v03:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory in Container2.

root@6rvc9s246v03:/# cd myvolume

# 4.List Contents of the Mounted Volume in Container2.

root@6rvc9s246v03:/# ls
```

#### 3.Now we are inside the container2 and if we create app.js in myvolume then it gets reflected in container1 myvolume directory:

```
# 1.Start the Container2 with Volume from the Container1.

ubuntu@balasenapathi:~$ docker run -it --name container2 --volumes-from container1 ubuntu sh

# 2.List Directories in the Container2.

root@6rvc9s246v03:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory in Container2.

root@6rvc9s246v03:/# cd myvolume

# 4.List Contents of the Mounted Volume in Container2.

root@6rvc9s246v03:/# ls

# 5.Create a New File in the Volume in Container2.

root@6rvc9s246v03:/# touch app.js

# 6.Verify the New File "app.js" was created in Mounted Volume in Container2.

root@6rvc9s246v03:/# ls
app.js
```

### 4.Verification: Now navigate to container1 and verify that app.js is reflected from Container2:

```
# 1.Run the Docker Container1 with Volume Mount.

ubuntu@balasenapathi:~$  docker run -it --name container1 -v /myvolume ubuntu sh
  
# 2.List Directories in the Container1.

root@8ffe7b843b01:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory in container1.

root@8ffe7b843b01:/# cd myvolume

# 4.List Contents of the Mounted Volume in container1.

root@8ffe7b843b01:/# ls
app.js
```

Now, if we go to container1 and do ls in the myvolume directory, we can see app.js reflected in container1.
This shows that both containers are mapped by using Docker volumes.

### 5.Mapping Container1 Volume with Multiple Containers:

- Now, create container3, using volumes from container1 because we have created a volume in container1.

```
# 1.Run Container3 Using Volumes from Container1.

ubuntu@balasenapathi:~$ docker run -it --name container3 --volumes-from container1 ubuntu sh

# 2.List Root Directory Contents in Container3.

root@c3nft579ner3:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root 

# 3.Change Directory to myvolume in Container3.

root@c3nft579ner3:/# cd myvolume

# 4.List Contents of myvolume in Container3.

root@c3nft579ner3:/myvolume# ls
app.js

# 5.Create a New File in myvolume in Container3.

root@c3nft579ner3:/myvolume# touch package.json

# 6.List Updated Contents of myvolume in Container3.

root@c3nft579ner3:/myvolume# ls
app.js  package.json
```

### 6.Check container1, container2 where file "package.json" created in container3 mapped with the 2 containers:
 
- Verifying in Container1:

```
# 1.Run the Docker Container1 with Volume Mount.

ubuntu@balasenapathi:~$ docker run -it --name container1 -v /myvolume ubuntu sh

# 2.List Directories in the Container1.

root@8ffe7b843b01:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory in container1.

root@8ffe7b843b01:/# cd myvolume

# 4.List Contents of the Mounted Volume in container1.

root@8ffe7b843b01:/# ls
app.js	package.json
```

- Verifying in Container2:

```
# 1.Start the Container2 with Volume from the Container1.

ubuntu@balasenapathi:~$ docker run -it --name container2 --volumes-from container1 ubuntu sh

# 2.List Directories in the Container2.

root@6rvc9s246v03:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory in Container2.

root@6rvc9s246v03:/# cd myvolume

# 4.List Contents of the Mounted Volume in Container2.

root@6rvc9s246v03:/# ls
app.js	package.json
```

- Note: We can confirm that the file "package.json" created in container3 is visible in both Container1 and Container2 as well.

### 7.Creating a volume by using Dockerfile:

#### 1.Clone this repository and move to example folder:
```
git clone https://github.com/balusena/docker-for-devops.git
cd  07-Docker Volumes/examples
```
#### 2.Dockerfile:
```
FROM ubuntu
VOLUME ["/volume1"]
```

#### 3.Create image from the Dockrfile:

```
# 1.List the docker images.

ubuntu@balasenapathi:~$ docker images
REPOSITORY           TAG               IMAGE ID       CREATED        SIZE
ubuntu               latest            5a81c4b8502e   8 days ago     77.8MB
balusena/money_api   latest            1665bedba94e   2 weeks ago    626MB
python               3.8-slim-buster   52456bc7bf3e   4 weeks ago    118MB
hello-world          latest            9c7a54a9a43c   2 months ago   13.3kB

# 2.Building a Docker Image Named myvolumeimage with a Volume Specified in Dockerfile.

ubuntu@balasenapathi:~$ docker build -t myvolumeimage .
Sending build context to Docker daemon  6.026GB
Step 1/2 : FROM ubuntu
---> 5a81c4b8502e
Step 2/2 : VOLUME ["/volume1"]
---> Running in 1ad3feaeec36
Removing intermediate container 1ad3feaeec36
---> d1b35717921f
Successfully built d1b35717921f
Successfully tagged myvolumeimage:latest

# 3.List the docker images.

ubuntu@balasenapathi:~$ docker images
REPOSITORY           TAG               IMAGE ID       CREATED          SIZE
myvolumeimage        latest            d1b35717921f   45 minutes ago   77.8MB
ubuntu               latest            5a81c4b8502e   8 days ago       77.8MB
balusena/money_api   latest            1665bedba94e   2 weeks ago      626MB
python               3.8-slim-buster   52456bc7bf3e   4 weeks ago      118MB
hello-world          latest            9c7a54a9a43c   2 months ago     13.3kB
```

#### 4.Create a container from "myvolumeimage" docker image:

```
# 1.Create a Container from myvolumeimage Docker Image.

ubuntu@balasenapathi:~$ docker run -it --name containerfromimage myvolumeimage sh

# 2.List Root Directory Contents.

root@b7f4a9d5e8c9:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume1

# 3.Change Directory to volume1:

root@b7f4a9d5e8c9:/# cd volume1

# 4.List Contents of volume1:

root@b7f4a9d5e8c9:/volume1# ls
```

- Note:
  You can notice that we have not added the -v flag in the docker run command because we have already declared the volume inside the Dockerfile while creating the Docker image, i.e., myvolumeimage. The volume specified in the Dockerfile is automatically included when creating a container from this image.

#### 5.Share volume with other containers (Container to container):

Now, create container4 using the Docker image myvolumeimage which has the volume defined, and share the 
volume from containerfromimage

```
# 1.Create container4 using Docker image myvolumeimage which has the volume defined, share volume from containerfromimage.

ubuntu@balasenapathi:~$ docker run -it --name container4 --volumes-from containerfromimage ubuntu sh

# 2.List Root Directory Contents.

root@b8d5c2e1f8b9:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume1

# 3.Change Directory to volume1.

root@b8d5c2e1f8b9:/# cd volume1 

# 4.List Contents of volume1.

root@b8d5c2e1f8b9:/volume1# ls
```

#### 6.Create a file new.js in container4 this gets reflected and mapped in containerfromimage container volume1 directory:

```
# 1.Create container4 using Docker image myvolumeimage which has the volume defined, share volume from containerfromimage.

ubuntu@balasenapathi:~$ docker run -it --name container4 --volumes-from containerfromimage ubuntu sh

# 2.List Root Directory Contents.

root@b8d5c2e1f8b9:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume1

# 3.Change Directory to volume1.

root@b8d5c2e1f8b9:/# cd volume1 

# 4.Create a New File new.js.

root@b8d5c2e1f8b9:/volume1# touch new.js

# 5.List Contents of volume1.

root@b8d5c2e1f8b9:/volume1# ls
new.js
```

#### 7.Verify new.js File reflected in containerfromimage volume1 After created in container4:
```
# 1.Create a Container from myvolumeimage Docker Image.

ubuntu@balasenapathi:~$ docker run -it --name containerfromimage myvolumeimage sh

# 2.List Root Directory Contents.

root@b7f4a9d5e8c9:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  volume1

# 3.Change Directory to volume1.

root@b7f4a9d5e8c9:/# cd volume1

# 4.List Contents of volume1.

root@b7f4a9d5e8c9:/volume1# ls
new.js
```

- Note:
  The new.js file created in container4 is successfully reflected in the volume1 directory of 
  containerfromimage. This confirms that volumes are correctly shared across containers.


#### 2.Host to Container:
Volumes can map directories from the host filesystem to the container.

#### 1.Create a container with volume shared from host:

- Now let's create another directory

```
# 1.List all the files and directories.

ubuntu@balasenapathi:~$ ls
Desktop             Dockers-ML-Flask-App  hello_file.ipynb                 Pictures                       pythonprograms
devops_balu_github  Documents             learnlinux                       prometheus-2.44.0.linux-amd64  snap
Dockerfile          Downloads             Music                            Public                         Templates
docker_flask        hadoopMyFiles         node_exporter-1.6.0.linux-amd64  PycharmProjects                Videos

# 2.Change to the home directory.

ubuntu@balasenapathi:~$ cd ..

# 3.Create h2c-volume in your home directory using sudo user to get all permissions to the file.

ubuntu@balasenapathi:/home$ sudo mkdir h2c-volume
[sudo] password for ubuntu: balasenapathi # <Provide your Root Password>

# 4.List all the files and directories.

ubuntu@balasenapathi:/home$ ls
devops  h2c-volume  hdoop  ubuntu

# 5.Navigate to the h2c-volume directory.

ubuntu@balasenapathi:/home$ cd h2c-volume

# 6.Check the present working directory.

ubuntu@balasenapathi:/home/h2c-volume$ pwd
/home/h2c-volume

# 7.List all the files and directories.

ubuntu@balasenapathi:/home/h2c-volume$ ls
```

#### 2.Now create a file app.js in /home/h2c-volume directory

```
# 1.Create file app.js in h2c-volume directory.

ubuntu@balasenapathi:/home/h2c-volume$ sudo touch app.js

# 2.List all the files and directories.

ubuntu@balasenapathi:/home/h2c-volume$ ls
app.js
```

#### 3.Creating Docker Container "h2c" with Host Volume "h2c-volume"

```
# 1.Running Docker Container h2c with Host Volume h2c-volume.

ubuntu@balasenapathi:/home/h2c-volume$ docker run -it --name h2c -v ${PWD}:/myvolume ubuntu sh

# 2.Check Container's Root Directory Contents.

root@d1b35717921f:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory.

root@d1b35717921f:/# cd myvolume

# 4.List Contents of Mounted Volume Directory.

root@d1b35717921f:/myvolume# ls
app.js
```
- Note: 
  This is how we can map a host directory (/home/h2c-volume) to the container (h2c) myvolume directory by 
  using docker volumes.

#### 4.Now create a file new.json in container h2c in myvloume directory.

```
# 1.Running Docker Container h2c with Host Volume h2c-volume.

ubuntu@balasenapathi:/home/h2c-volume$ docker run -it --name h2c -v ${PWD}:/myvolume ubuntu sh

# 2.Check Container's Root Directory Contents.

root@d1b35717921f:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory.

root@d1b35717921f:/# cd myvolume

# 4.Now create new.json in myvolume directory.

root@d1b35717921f:/myvolume# touch new.json

# 5.List Contents of Mounted Volume Directory.

root@d1b35717921f:/myvolume# ls
app.js  new.json
```

#### 5.Now verify that new.json file is reflected in host h2c-volume directory.

```
# 1. List the file new.json in h2c-volume directory

ubuntu@balasenapathi:/home/h2c-volume$ ls
app.js new.json
```

- Note: The presence of new.json in the /home/h2c-volume directory on the host confirms that changes 
  made within the container's /myvolume directory are successfully reflected on the host. This 
  demonstrates the effective synchronization provided by the Docker bind mount, ensuring that files 
  created or modified in the container are also updated on the host.

#### 6.What happens if we delete the container "h2c", does data is persistent in host "h2c-volume" directory.

```
# 1.Running Docker Container h2c with Host Volume h2c-volume.

ubuntu@balasenapathi:/home/h2c-volume$ docker run -it --name h2c -v ${PWD}:/myvolume ubuntu sh

# 2.Check Container's Root Directory Contents.

root@d1b35717921f:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin srv  sys  tmp  usr  var

# 3.Navigate to the Mounted Volume Directory.

root@d1b35717921f:/# cd myvolume

# 4.Now create files "app.py" , "node.js"  in myvolume directory.

root@d1b35717921f:/myvolume# touch app.py  new.js

# 5.List Contents of Mounted Volume Directory.

root@d1b35717921f:/myvolume# ls
app.py  node.js

# 6.Stop and remove the container:h2c.

ubuntu@balasenapathi:~$ docker rm -f h2c
h2c

*******************************************************************************

# 7.Now go to host directory and list the files in h2c-volume directory

ubuntu@balasenapathi:/home/h2c-volume$ ls
app.py  node.js
```

- Note: 
  Deleting the container does not delete the data in the host directory (/home/h2c-volume). The data in 
  the bind-mounted directory persists on the host even after the container is removed. This ensures that 
  your important data is retained independently of the container's lifecycle.

### Example for Docker Volume types:

#### 1.Create a container without using any volumes and the container uses the docker host as the default directory.

```
# 1.List All the Docker Containers

ubuntu@balasenapathi:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 2.List All the Docker Volumes

ubuntu@balasenapathi:~$ docker volume ls
DRIVER    VOLUME NAME

# 3.Run an Nginx Container Interactively with Bash

ubuntu@balasenapathi:~$ docker run -it --name vtwebint01 nginx /bin/bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
faef57eae888: Pull complete
76579e9ed380: Pull complete
cf707e233955: Pull complete
91bb7937700d: Pull complete
4b962717ba55: Pull complete
f46d7b05649a: Pull complete
103501419a0a: Pull complete
Digest: sha256:08bc36ad52474e528cc1ea3426b5e3f4bad8a130318e3140d6cfe29c8892c7ef
Status: Downloaded newer image for nginx:latest

# Check Disk Space Usage Inside a Docker Container in a human-readable format such as GB, MB, or KB.

root@8ffe7b843b01:/# df -h                       
Filesystem      Size  Used Avail Use% Mounted on
overlay          98G   35G   59G  37% /
tmpfs            64M     0   64M   0% /dev
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
shm              64M     0   64M   0% /dev/shm
/dev/sda5        98G   35G   59G  37% /etc/hosts
tmpfs           1.9G     0  1.9G   0% /proc/asound
tmpfs           1.9G     0  1.9G   0% /proc/acpi
tmpfs           1.9G     0  1.9G   0% /proc/scsi
tmpfs           1.9G     0  1.9G   0% /sys/firmware
```

#### 2.Creating "file1", "file2" Files Inside a Docker Container and Locating Them on the Docker Host.

```
# 1.Creating files inside the docker container.
 
root@8ffe7b843b01:/# touch file1 file2
```

#### 3.Finding Files Created Inside a Docker Container on the Host.

```
# 1.List Docker Volumes on the Host.

ubuntu@balasenapathi:~$ docker volume ls
DRIVER    VOLUME NAME

# 2.Search for the File file1 on the Host:

ubuntu@balasenapathi:~$ sudo find / -name file1
[sudo] password for ubuntu: balasenapathi
/var/lib/docker/overlay2/cd10d8f274ecb023b1cb9a80a3c12fe187d1d19519466730b6d320c432b3b643/diff/file1
/var/lib/docker/overlay2/cd10d8f274ecb023b1cb9a80a3c12fe187d1d19519466730b6d320c432b3b643/merged/file1
```
- Note: 
  The file file1 created inside the Docker container is stored on the host under the directory /var/lib/docker/overlay2/
  with a unique hash (cd10d8f274ecb023b1cb9a80a3c12fe187d1d19519466730b6d320c432b3b643) subdirectory. 
  This hash directory is part of Docker's overlay filesystem, which tracks changes made in the container 
  compared to the base image.

#### 4.Now check your docker container

```
# 1.List all the docker containers.

ubuntu@balasenapathi:~$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
8ffe7b843b01   nginx     "/docker-entrypoint.…"   30 minutes ago   Up 30 minutes   80/tcp    vtwebint01
```
#### 5.Now we are going to exit this container and stop this container
````
# 1.Exit from the docker container.

root@8ffe7b843b01:/# exit
exit

# 2.To list the all docker containers.

ubuntu@balasenapathi:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                     PORTS     NAMES
8ffe7b843b01   nginx     "/docker-entrypoint.…"   33 minutes ago   Exited (0) 8 seconds ago             vtwebint01

# 3.Stop and remove the container:vtwebint01.

```
ubuntu-dsbda@ubuntudsbda-virtual-machine:~$ docker rm vtwebint01
vtwebint01
```
#### 6.Searching for file1 After Deleting the Container vtwebint01.

```
# 1.Search for the File file1 on the Host After Deleting the Container:.

ubuntu@balasenapathi:~$ sudo find / -name file1
[sudo] password for ubuntu: balasenapathi
find: ‘/run/user/1000/doc’: Permission denied
find: ‘/run/user/1000/gvfs’: Permission denied
```

- Note: 
  "file1" does not exist on the host after the container deletion because Docker removed the underlying 
  backend directory that contained file1. This demonstrates the importance of using Docker volumes to
  persist data, ensuring that files remain available even after the container is removed.

























