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

Example:

### 1.Database Container:

- Without Volume: Data stored in the container’s virtual filesystem is lost if the container is removed.

- With Volume: Data is stored in a Docker-managed volume, which persists across container removals and restarts.

### 2.Stateful Applications:

- Without Volume: Application state and data can be lost on container restarts or removals.

- With Volume: Application state is preserved by mounting a Docker volume, ensuring continuity and data retention.

Using Docker volumes, you can ensure that crucial data is preserved, enhancing the reliability and 
stability of applications that rely on persistent storage.

Note: CONTAINER sits in HOST operating system(OS)

- HOST [ Physical File System ] path ===> /home/mount/data
- 
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

1. Host Volumes (Bind Mounts)
   - Definition: Uses a directory or file from the host filesystem and mounts it into the container.
   - Management: Managed by the host system.
   - Use Case: When you need direct access to host files, such as configuration files or code repositories.
