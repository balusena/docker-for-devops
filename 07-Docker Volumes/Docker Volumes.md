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
# Run a container with a bind mount

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
# Run a container with an anonymous volume

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
# Create a named volume
docker volume create my-named-volume

# Run a container with the named volume
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
# To see the list of docker images:

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
# Run the Docker Container1 with Volume Mount

ubuntu@balasenapathi:~$ docker run -it --name container1 -v /myvolume ubuntu sh

# List Directories in the Container1

root@8ffe7b843b01:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# Navigate to the Mounted Volume Directory in container1

root@8ffe7b843b01:/# cd myvolume

# List Contents of the Mounted Volume in container1

root@8ffe7b843b01:/# ls
```

#### 2.Now create another container2 using volumes from container1
```
# Run a Docker Container2 with Volume from Container1

ubuntu@balasenapathi:~$ docker run -it --name container2 --volumes-from container1 ubuntu sh

# List Directories in the Container2

root@6rvc9s246v03:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# Navigate to the Mounted Volume Directory in Container2

root@6rvc9s246v03:/# cd myvolume

# List Contents of the Mounted Volume in Container2

root@6rvc9s246v03:/# ls
```

#### 3.Now we are inside the container2 and if we create app.js in myvolume then it gets reflected in container1 myvolume directory

```
# Start the Container2 with Volume from the Container1

ubuntu@balasenapathi:~$ docker run -it --name container2 --volumes-from container1 ubuntu sh

# List Directories in the Container2

root@6rvc9s246v03:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin srv  sys  tmp  usr  var

# Navigate to the Mounted Volume Directory in Container2

root@6rvc9s246v03:/# cd myvolume

# List Contents of the Mounted Volume in Container2

root@6rvc9s246v03:/# ls

# Create a New File in the Volume in Container2

root@6rvc9s246v03:/# touch app.js

# Verify the New File "app.js" was created in Mounted Volume in Container2

root@6rvc9s246v03:/# ls
app.js
```
### 4.Verification: Now navigate to container1 and verify that app.js is reflected from Container2

```
# Run the Docker Container1 with Volume Mount

ubuntu-dsbda@ubuntudsbda-virtual-machine:~$ docker run -it --name container1 -v /myvolume ubuntu sh
  
# List Directories in the Container1

root@8ffe7b843b01:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# Navigate to the Mounted Volume Directory in container1

root@8ffe7b843b01:/# cd myvolume

# List Contents of the Mounted Volume in container1

root@8ffe7b843b01:/# ls
app.js
```
Now, if we go to container1 and do ls in the myvolume directory, we can see app.js reflected in container1.
This shows that both containers are mapped by using Docker volumes.

### 5.Mapping Container1 Volume with Multiple Containers.

- Now, create container3, using volumes from container1 because we have created a volume in container1.

```
# Run Container3 Using Volumes from Container1

ubuntu@balasenapathi:~$ docker run -it --name container3 --volumes-from container1 ubuntu sh

# List Root Directory Contents in Container3

root@c3nft579ner3:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root 

# Change Directory to myvolume in Container3

root@c3nft579ner3:/# cd myvolume

#List Contents of myvolume in Container3

root@c3nft579ner3:/myvolume# ls
app.js

# Create a New File in myvolume in Container3

root@c3nft579ner3:/myvolume# touch package.json

# List Updated Contents of myvolume in Container3

root@c3nft579ner3:/myvolume# ls
app.js  package.json
```
### 6.Now check container1 and container2 to see the updated file package.json created in container3 mapped with all 3 containers
- Verifying in Container1:
```
# Run the Docker Container1 with Volume Mount

ubuntu@balasenapathi:~$ docker run -it --name container1 -v /myvolume ubuntu sh

# List Directories in the Container1

root@8ffe7b843b01:/# ls

bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin	srv  sys  tmp  usr  var

# Navigate to the Mounted Volume Directory in container1

root@8ffe7b843b01:/# cd myvolume

# List Contents of the Mounted Volume in container1

root@8ffe7b843b01:/# ls
app.js	package.json
```
- Verifying in Container2:
```
# Start the Container2 with Volume from the Container1

ubuntu@balasenapathi:~$ docker run -it --name container2 --volumes-from container1 ubuntu sh

# List Directories in the Container2

root@6rvc9s246v03:/# ls
bin  boot  dev	etc  home  lib	lib32  lib64  libx32  media  mnt  myvolume  opt  proc  root  run  sbin srv  sys  tmp  usr  var

# Navigate to the Mounted Volume Directory in Container2

root@6rvc9s246v03:/# cd myvolume

# List Contents of the Mounted Volume in Container2

root@6rvc9s246v03:/# ls
app.js	package.json
```
- Note: We can confirm that the file "package.json" created in container3 is visible in both Container1 and Container2 as well.














