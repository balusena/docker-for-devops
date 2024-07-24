# Dockerfile

## What is Dockerfile
A Dockerfile is a script that contains instructions for building a customized docker image. Each instruction 
in a Dockerfile creates a new layer in the image, and the final image is composed of all the layers stacked 
on top of each other.

It includes instructions for installing dependencies, copying files, setting environment variables, and 
configuring the container.

Dockerfile uses a simple, easy-to-read syntax that can be created and edited with any text editor. Once a 
Dockerfile has been created, it can be used to build an image using the docker build command. The resulting 
image can then be run as a container using the docker run command.

The structure of a Dockerfile is based on a set of simple instructions, such as "FROM", "RUN", "COPY", "ENV", 
"ADD", "CMD", "ENTRYPOINT", "EXPOSE", "LABEL", "MAINTAINER", "ONBUILD", "SHELL", "STOPSIGNAL", "USER", "VOLUME",
"WORKDIR", "ARG", and "HEALTHCHECK". Each instruction adds a new layer to the image, and each layer includes 
the instructions specified in the previous layer. The final image is a result of all the instructions specified
in the Dockerfile.

## Explanation of Dockerfile commands

# Dockerfile Commands Explained

1. **FROM**
    - Uses a specified base image to create the container.
```
 FROM python:3.9
```
2. **LABEL**
    - Adds metadata to the image, such as maintainer information or build details.
```
LABEL maintainer="balasenapathi@example.com" \
      version="1.0" \
      description="A Python Flask application"
```
3. **ENV**
    - Sets environment variables for the container, allowing configuration of the application at runtime.
```
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    ENVIRONMENT=${ENVIRONMENT}
```
4. **ARG**
    - Defines build-time variables that can be passed to the Dockerfile during the build process.
```
ARG APP_VERSION=1.0
```
5. **WORKDIR**
    - Sets the working directory inside the container, where all subsequent commands will be executed.
```
WORKDIR /usr/src/app
```
6. **COPY**
    - Copies files or directories from the host machine to the container.
```
COPY requirements.txt ./
```
7. **RUN**
    - Executes commands in the container, commonly used to install software packages or dependencies.
```
RUN pip install --no-cache-dir -r requirements.txt
```
8. **ADD**
    - Adds files, directories, or remote file URLs to the container, similar to COPY but with additional capabilities like extracting tar files.
```
ADD . /usr/src/app
```
9. **EXPOSE**
    - Documents the port on which the containerized application will run.
```
EXPOSE 5000
```
10. **VOLUME**
    - Creates a mount point with a specified path and associates it with a volume, allowing data to persist or be shared with other containers.
```
VOLUME ["/usr/src/app/logs"]
```
11. **USER**
    - Sets the user to run the container processes, enhancing security by avoiding running as the root user.
```
USER balu
```
12. **ONBUILD**
    - Defines instructions to be executed when the image is used as a base for another build.
```
ONBUILD COPY . /app/src
```
13. **STOPSIGNAL**
    - Specifies the system call signal that will be sent to the container to stop it, allowing for graceful shutdowns.
```
STOPSIGNAL SIGINT
```
14. **HEALTHCHECK**
    - Configures a command to test the container’s health, ensuring it is running as expected.
```
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:5000/ || exit 1
```
15. **SHELL**
    - Overrides the default shell used to run commands in the container.
```
SHELL ["/bin/bash", "-c"]
```
16. **CMD**
    - Sets the default command to be executed when running the container, usually the main application.
```
CMD ["python", "app.py"]
```
17. **ENTRYPOINT**
    - Configures a container to run as an executable, allowing it to be used in a more predictable and controlled manner.
```
ENTRYPOINT ["python", "app.py"]
```
18. **LABEL (additional)**
    - Adds additional labels for providing extra metadata, such as build version information.
```
LABEL build_version=$APP_VERSION
```
## Benefits of using a Dockerfile
- 1.Faster Deployments:
Dockerfiles enable faster and more efficient deployment of applications. Once a Docker image has been built, 
it can be easily deployed to any environment that supports Docker.

- 2.Automation Testing:
Dockerfiles can be used in automation testing to build and run test environments for different applications 
and services. Using a Dockerfile, you can create an image of a specific test environment that can be easily and consistently recreated, without needing to manually set up and configure the environment on each test run.

- 3.Quick CI/CD Integration: 
You can easily integrate your Dockerfile with a continuous integration and continuous deployment (CI/CD) 
pipeline, which enables you to automatically build, test and deploy your application with every code change.
This helps to reduce the risk of errors and ensures that your application is always up-to-date.

## Clone this repository and move to example folder
```
git clone https://github.com/balusena/docker-for-devops.git
cd  04-Dockerfile/examples
```
### Dockerfile
```
# Use the official Python image from the Docker Hub
FROM python:3.9

# Adds metadata to the image, such as maintainer information or build details.
LABEL maintainer="balasenapathi@example.com" \
      version="1.0" \
      description="A Python Flask application"

# Define environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    ENVIRONMENT=development

# Define build argument
ARG APP_VERSION=1.0

# Set working directory
WORKDIR /usr/src/app

# Copy and install dependencies
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy the application code
COPY . /usr/src/app

# Expose application port
EXPOSE 5000

# Define volume for logs
VOLUME ["/usr/src/app/logs"]

# Set the user (ensure this user exists or remove this line)
# USER balu

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:5000/ || exit 1

# Set default command
CMD ["python", "app.py"]

# Label with build version
LABEL build_version="${APP_VERSION}"
```
## To build the Docker image, run it as a container, and access your application, follow these steps:

### 1. Build the Docker Image
Navigate to the directory containing your Dockerfile. Open your terminal or command prompt and run:
```
ubuntu@balasenapathi:~$ docker build -t my-flask-app:1.0 .
Sending build context to Docker daemon  4.096kB
Step 1/10 : FROM python:3.9
 ---> e6f59cf92b83
Step 2/10 : LABEL maintainer="balasenapathi@example.com" version="1.0" description="A Python Flask application"
 ---> Running in 8b8d6c7b8e5d
Removing intermediate container 8b8d6c7b8e5d
 ---> 1d4b8d7d8b5c
Step 3/10 : ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1 ENVIRONMENT=development
 ---> Running in 1d3e8c7b6f5c
Removing intermediate container 1d3e8c7b6f5c
 ---> 2c8d5b4e6a7f
Step 4/10 : ARG APP_VERSION=1.0
 ---> Running in 2e8c5b3f9b6d
Removing intermediate container 2e8c5b3f9b6d
 ---> 3d5b8c9e4f7a
Step 5/10 : WORKDIR /usr/src/app
 ---> Running in 3d8e9c6b5f7d
Removing intermediate container 3d8e9c6b5f7d
 ---> 4d7c5b9e8a6f
Step 6/10 : COPY requirements.txt ./
 ---> 9d6e5b3f4a2c
Step 7/10 : RUN pip install --no-cache-dir -r requirements.txt
 ---> Running in 5b9e8c7d6f8a
Collecting flask==2.0.1
  Downloading Flask-2.0.1-py3-none-any.whl (95 kB)
     |████████████████████████████████| 95 kB 1.0 MB/s eta 0:00:01
...
Successfully installed Flask-2.0.1
Removing intermediate container 5b9e8c7d6f8a
 ---> 6d8c5b9e3f7a
Step 8/10 : COPY . /usr/src/app
 ---> 8e7c5b9d8f2a
Step 9/10 : EXPOSE 5000
 ---> Running in 9e8c5b3d6f8a
Removing intermediate container 9e8c5b3d6f8a
 ---> 7c9e5b8f3d6a
Step 10/10 : CMD ["python", "app.py"]
 ---> Running in 8d7e5b9c6f8a
Removing intermediate container 8d7e5b9c6f8a
 ---> 8e7c5b9d8f2a
Successfully built 8e7c5b9d8f2a
Successfully tagged my-flask-app:1.0
```
-t my-flask-app:1.0 tags the image with the name my-flask-app and version 1.0
. specifies the build context as the current directory

### 2. Run the Docker Container
Once the image is built, you can run a container from it. Use the following command:
```
ubuntu@balasenapathi:~$ docker run -d -p 5000:5000 --name my-flask-app-container my-flask-app:1.0
5f8d6e5b9c8f6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c
```
-d runs the container in detached mode (in the background).
-p 5000:5000 maps port 5000 on your host machine to port 5000 in the container. This allows you to access the application on your host machine through this port.
--name my-flask-app-container assigns a name to the container for easier management.
my-flask-app:1.0 is the name and tag of the image you built.

### 3. Access the Application
With the container running, you can access the Flask application through a web browser or a tool like curl 
by navigating to:
```
ubuntu@balasenapathi:~$ curl http://localhost:5000

0r

http://localhost:5000
```
Output:
```
Hello, world!
```
Note: If your application is set up correctly, you should see it responding as expected.

### Checking Container Status
To verify that your container is running and see its details, you can use the following commands:

- 1.List Running Containers:
```
ubuntu@balasenapathi:~$ docker ps
CONTAINER ID        IMAGE                COMMAND             CREATED             STATUS              PORTS                    NAMES
5f8d6e5b9c8f        my-flask-app:1.0     "python app.py"     5 minutes ago       Up 5 minutes        0.0.0.0:5000->5000/tcp my-flask-app-container
```
- 2.Inspect the Container:
```
ubuntu@balasenapathi:~$ docker inspect my-flask-app-container
[
    {
        "Id": "5f8d6e5b9c8f6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c",
        "Created": "2024-07-24T14:25:23.245943977Z",
        "Path": "python",
        "Args": [
            "app.py"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 12345,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-07-24T14:25:24.123456789Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:3d5b8c9e4f7a6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c",
        "ResolvConfPath": "/var/lib/docker/containers/5f8d6e5b9c8f6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/5f8d6e5b9c8f6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c/hostname",
        "HostsPath": "/var/lib/docker/containers/5f8d6e5b9c8f6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c/hosts",
        "LogPath": "/var/lib/docker/containers/5f8d6e5b9c8f6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c/5f8d6e5b9c8f6d8e7f8b6c5d8a9e7c6b9d8e5f8a6b7c-json.log",
        "Name": "/my-flask-app-container",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Mounts": [
            {
                "Type": "volume",
                "Name": "my-flask-app_logs",
                "Source": "/var/lib/docker/volumes/my-flask-app_logs/_data",
                "Destination": "/usr/src/app/logs",
                "Driver": "local",
                "Mode": "rw",
                "RW": true,
                "Propagation": "rshared"
            }
        ],
        "Config": {
            "Hostname": "5f8d6e5b9c8f",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": true,
            "AttachStderr": true,
            "ExposedPorts": {
                "5000/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PYTHONDONTWRITEBYTECODE=1",
                "PYTHONUNBUFFERED=1",
                "ENVIRONMENT=development",
                "APP_VERSION=1.0"
            ],
            "Cmd": [
                "python",
                "app.py"
            ],
            "Image": "my-flask-app:1.0",
            "Volumes": {
                "/usr/src/app/logs": {}
            },
            "WorkingDir": "/usr/src/app",
            "Labels": {
                "build_version": "1.0",
                "description": "A Python Flask application",
                "maintainer": "balasenapathi@example.com",
                "version": "1.0"
            },
            "Healthcheck": {
                "Test": [
                    "CMD",
                    "curl",
                    "-f",
                    "http://localhost:5000/"
                ],
                "Interval": 30000000000,
                "Timeout": 10000000000,
                "Retries": 3
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "e34b2f7e4e5d5f7e5e8b6c5d8a9e7c6b9d8e5f8a6b7c",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "5000/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "5000"
                    }
                ]
            },
            "SandboxKey": "/var/run/docker/netns/e34b2f7e4e5d",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "f3e8c7b6d9e0",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPAddressIPv6": "",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "a0b3f5c8d9e0",
                    "EndpointID": "f3e8c7b6d9e0",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```
### Key Fields Explained:
- Id: Unique identifier of the container.
- Created: Timestamp when the container was created.
- Path: Command used to start the container (python).
- Args: Arguments passed to the command (app.py).
- State: Current state of the container (running, paused, etc.).
- Image: Image used to create the container (referenced by its SHA256 digest).
- ResolvConfPath, HostnamePath, HostsPath: Paths to various configuration files within the container.
- LogPath: Path to the container’s log file.
- Name: Name assigned to the container (/my-flask-app-container).
- Mounts: Information about volumes mounted in the container.
- Config: Configuration settings for the container, including environment variables, exposed ports, and labels.
- NetworkSettings: Details about network configuration, including IP address and port mappings.

This JSON output provides a comprehensive view of the container’s configuration and current state, helping 
you manage and troubleshoot the container effectively.

- 3.View Container Logs:
```
ubuntu@balasenapathi:~$ docker logs my-flask-app-container
 * Serving Flask app "app"
 * Debug mode: off
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
192.161.112.120  - - [24/Jul/2024 14:35:12] "GET / HTTP/1.1" 200 -
192.161.112.120  - - [24/Jul/2024 14:35:15] "GET / HTTP/1.1" 200 -
192.161.112.120  - - [24/Jul/2024 14:35:20] "GET / HTTP/1.1" 200 -
```
### Explanation of the Access Logs
- 192.161.112.120- - [24/Jul/2024 14:35:12] "GET / HTTP/1.1" 200 -:
  - 192.161.112.120 is the IP address of the client making the request (in this case, likely your host machine).
  - 24/Jul/2024 14:35:12 is the timestamp of the request.
  - "GET / HTTP/1.1" shows the HTTP method and path of the request.
  - 200 is the HTTP status code returned by Flask (200 OK).
  - The final - represents additional information such as the size of the response, which is not shown in this case.

- Summary:
In a typical working scenario, the docker logs my-flask-app-container command will show you the Flask server's
output indicating it's running and listening for requests. If there are any issues, the logs will provide stack
traces and error messages that can help with debugging.





















