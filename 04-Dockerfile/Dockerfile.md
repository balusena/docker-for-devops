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