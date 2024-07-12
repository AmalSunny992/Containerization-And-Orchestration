# Docker Compose

## What is Docker Compose?
Docker Compose is a tool for defining and running multi-container Docker applications. With Docker Compose, you use a YAML file to configure your application’s services, networks, and volumes. Then, with a single command, you create and start all the services from your configuration.

## Benefits of Using Docker Compose
**Simplified Configuration:** Manage multi-container applications with a single YAML file.

**Easy Scaling:** Scale up or down services as needed.

**Efficient Development:** Quickly set up and tear down complex environments.

## Writing Docker Compose Files
A Docker Compose file (docker-compose.yml) defines the services, networks, and volumes required for your application. Below is an example of a simple docker-compose.yml file:

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./web:/usr/share/nginx/html
    networks:
      - webnet

  db:
    image: postgres:latest
    environment:
      POSTGRES_DB: exampledb
      POSTGRES_USER: exampleuser
      POSTGRES_PASSWORD: examplepass
    volumes:
      - dbdata:/var/lib/postgresql/data
    networks:
      - webnet

volumes:
  dbdata:

networks:
  webnet:
```

## Key Components
**version:** Specifies the version of the Docker Compose file format.

**services:** Defines the services (containers) that make up your application.

**image:** The Docker image to use for the service.

**ports:** Maps ports on the host to ports in the container.

**volumes:** Mounts host directories or volumes into the container.

**networks:** Connects the service to one or more networks.

**environment:** Sets environment variables for the service.

**volumes:** Defines named volumes that can be shared among services.

**networks:** Defines custom networks for inter-service communication.

Example Docker Compose File
Below is a more detailed example of a docker-compose.yml file for a web application with a backend service:

```yaml
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "5000:5000"
    volumes:
      - ./web:/app
    depends_on:
      - db
    networks:
      - webnet

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
    volumes:
      - dbdata:/var/lib/mysql
    networks:
      - webnet

volumes:
  dbdata:

networks:
  webnet:
```

**In this example:**

- web service is built from the ./web directory and maps port 5000 on the host to port 5000 in the container.
- db service uses the mysql:5.7 image and sets environment variables for MySQL configuration.
- volumes and networks are defined for sharing data and communication between services.

## Managing Multi-Container Applications
Docker Compose simplifies the management of multi-container applications with a set of commands.

### Basic Docker Compose Commands

#### Starting Services
Start all services defined in docker-compose.yml.

```sh
docker-compose up
```

#### Starting Services in Detached Mode
Run services in the background.

```sh
docker-compose up -d
```

#### Stopping Services
Stop all running services.

```sh
docker-compose down
```

#### Viewing Service Logs
View the logs of all services.

```sh
docker-compose logs
```

#### Scaling Services
Scale a service to the specified number of instances.

```sh
docker-compose up -d --scale <service>=<number>
# Example
docker-compose up -d --scale web=3
```

#### Listing Running Services
List all running services.

```sh
docker-compose ps
```

#### Executing Commands in a Service Container
Run a command inside a running service container.

```sh
docker-compose exec <service> <command>
# Example
docker-compose exec web /bin/bash
```

## Summary
Docker Compose is a powerful tool for managing multi-container applications. It simplifies the configuration and deployment of complex environments, making it easier to develop, test, and scale your applications. By using a single YAML file to define services, networks, and volumes, Docker Compose provides a straightforward and efficient way to handle multi-container Docker applications.