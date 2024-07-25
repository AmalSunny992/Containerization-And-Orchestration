# Docker Basics

## Docker Images and Containers

### Docker Images
Docker images are immutable templates used to create containers. They contain everything needed to run an application, including the code, runtime, libraries, environment variables, and configuration files.

### Key Concepts
**Layers:** Images are built in layers, each representing a set of file changes or additions. Layers are stacked, and each layer is a read-only snapshot of the filesystem.

**Dockerfile:** A script containing a series of instructions on how to build a Docker image. Each instruction in a Dockerfile creates a new layer in the image.

**Registries:** Images are stored in registries. Docker Hub is the default public registry, but private registries can also be used.

Example Dockerfile

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

### Docker Containers
Docker containers are runtime instances of Docker images. They are isolated environments where applications run. Containers encapsulate an application and its dependencies, providing consistency across various environments.

### Key Concepts
**Isolation:** Containers run in isolated processes with their own filesystem, network interfaces, and process space.

**Portability:** Containers can run on any system that supports Docker, making them highly portable.

**Ephemeral:** Containers are meant to be temporary and disposable. They can be stopped and removed when no longer needed.

## Docker Commands

### Basic Docker Commands

#### Docker Version
Check the installed Docker version.

```sh
docker --version
```

#### Docker Info
Display system-wide information about Docker.

```sh
docker info
```

### Working with Images

#### Pull an Image
Download an image from a registry.

```sh
docker pull <image_name>

# Example
docker pull nginx
```

#### List Images
List all downloaded images.

```sh
docker images
```

#### Build an Image
Build an image from a Dockerfile.

```sh
docker build -t <image_name> .
# Example
docker build -t my-python-app .
```

#### Remove an Image
Delete an image from the local system.

```sh
docker rmi <image_name>
# Example
docker rmi nginx
```

### Working with Containers

#### Run a Container
Run a new container from an image.

```sh
docker run -d -p 80:80 --name <container_name> <image_name>
# Example
docker run -d -p 80:80 --name my-nginx nginx
```
The docker run command allows you to run Docker containers with various options. 
Here are some commonly used options:

	•	-d: Run container in detached mode (in the background).
	•	-i: Keep STDIN open even if not attached.
	•	-t: Allocate a pseudo-TTY (terminal).
	•	-p: Publish a container’s port(s) to the host.
	•	--name: Assign a name to the container.
	•	-v: Bind mount a volume.
	•	-e: Set environment variables.
	•	--rm: Automatically remove the container when it exits.
	•	--network: Connect a container to a network.

Example usage:
```
docker run -d -p 8080:80 --name mycontainer nginx
```

This runs an Nginx container in the background, maps port 80 of the container to port 8080 on the host, and names the container mycontainer.


#### List Running Containers
List all running containers.

```sh
docker ps
```

#### List All Containers
List all containers, including stopped ones.

```sh
docker ps -a
```

### Stop a Container
Stop a running container.

```sh
docker stop <container_name>
# Example
docker stop my-nginx
```

#### Remove a Container
Remove a stopped container.

```sh
docker rm <container_name>
# Example
docker rm my-nginx
```

#### Container Logs
View the logs of a container.

```sh
docker logs <container_name>
# Example
docker logs my-nginx
```

#### Execute Commands in a Container

The docker exec command is used to run commands inside an existing container. 
Here are some commonly used options:

	•	-d, --detach: Run command in the background.
	•	-i, --interactive: Keep STDIN open even if not attached.
	•	-t, --tty: Allocate a pseudo-TTY.
	•	-u, --user: Username or UID to run the command as.
	•	--workdir: Working directory inside the container.
	•	-e, --env: Set environment variables.

```sh
docker exec -it <container_name> <command>

Example usage:

docker exec -it mycontainer /bin/bash
```

This runs an interactive Bash shell inside the mycontainer container.


## Summary
Docker simplifies the development, shipping, and deployment of applications using containerization. Docker images serve as templates for creating containers, which are isolated environments for running applications. Mastering Docker commands is essential for effectively managing Docker images and containers, enabling efficient development workflows and consistent application deployments.