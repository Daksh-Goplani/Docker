# Docker Notes

## 1. Why Docker?

Docker is used to solve a very common problem in software development and deployment:

> "It works on my machine."

Sometimes an application runs perfectly on one developer's computer but fails on another because of different:
- operating systems
- library versions
- environment settings
- dependencies
- runtime configurations

Docker helps by packaging the application and all its required dependencies into a single unit called a container. This makes the application behave the same way across different machines.

### Benefits of Docker
- Consistent environment across machines
- Easy sharing of software
- Better collaboration between developers
- Helps deploy apps to other environments more reliably
- Supports cross-platform execution
- Makes software portable and predictable

### Real-life example
If a team member builds an app on Linux and another runs it on Windows, the app may fail because of missing packages or OS differences. Docker reduces this risk by creating a consistent environment for the app.

---

## 2. What is a Docker Container?

A Docker container is a running instance of a Docker image. It is a lightweight, isolated environment that contains:
- the application code
- required libraries
- configuration files
- system dependencies

### Key characteristics of a container
- Portable: can be shared and run on different machines
- Lightweight: smaller than full virtual machines
- Isolated: each container runs in its own environment
- Consistent: same app behaves similarly everywhere
- Fast to create and remove

### Why containers are useful
Containers allow developers to run multiple apps or services with different dependencies in parallel without conflicts. This is especially useful when you need different versions of tools or libraries for different projects.

### Container vs Virtual Machine
A container does not virtualize an entire operating system like a VM. It uses the host OS kernel and shares system resources, which makes it faster and lighter.

---

## 3. What is a Docker Image?

A Docker image is like a blueprint or template used to create containers.

It contains:
- instructions to build the environment
- app code and dependencies
- configuration needed to run the application

An image is read-only. Once it is created, it can be used to start multiple containers.

### Image and Container relationship
The relationship is similar to:
- Class = Docker image
- Object = Docker container

This means:
- One image can create many containers
- Each container is an independent running copy of the image
- The image is shared and reused across environments

### Storage and resource usage
- Images are reusable and usually stored centrally
- Containers consume runtime resources while running
- The image is relatively small compared to the full environment needed to run an app
- Multiple containers can run from the same image

---

## 4. Docker in Simple Words

Docker helps developers package an application with everything it needs to run. Once packaged, it can be moved and launched on another machine without manually installing dependencies.

This makes software setup easier, faster, and more reliable.

---

## 5. Common Docker Commands

### Pull an image
```bash
docker pull image_name
```
Downloads an image from Docker Hub or a registry.

### List downloaded images
```bash
docker images
```
Shows all local Docker images.

### Run an image
```bash
docker run image_name
```
Creates and starts a container based on the image.

### Run in interactive mode
```bash
docker run -it image_name
```
Starts the container in interactive mode so you can access the terminal inside it.

### List running containers
```bash
docker ps
```
Shows all currently running containers.

### List all containers
```bash
docker ps -a
```
Shows all containers, including stopped ones.

### Start an existing container
```bash
docker start container_name
```
Or:
```bash
docker start container_id
```

### Stop a running container
```bash
docker stop container_name
```
Or:
```bash
docker stop container_id
```

### Remove a container
```bash
docker rm container_name
```
Deletes a container.

### Remove an image
```bash
docker rmi image_name
```
Deletes an image. Usually, the container created from that image must be removed first.

### Pull a specific version of an image
```bash
docker pull mysql:8.0
```
This pulls a specific version instead of the latest one.

### Run a MySQL container with environment variable
```bash
docker run -d -e MYSQL_ROOT_PASSWORD=secret mysql
```
- `-d` = run in detached mode
- `-e` = set environment variable

### Run with a custom container name
```bash
docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-older mysql:8.0
```
This creates a container named `mysql-older` using MySQL version 8.0.

---

## 6. Docker Image Layers

Docker images are built in layers.

Each layer represents a specific instruction or change in the image, such as:
- installing a package
- copying files
- setting environment variables
- creating directories

### Important fact
All layers are immutable except the top writable layer of the container.

When you create a container from an image:
- a new writable layer is added on top of the image layers
- the image layers remain unchanged
- the container layer can be modified while the app is running

### Why layering is useful
- saves space and bandwidth
- allows image reuse
- makes updates faster
- improves caching during builds

Example idea:
If you change only one file in your app, Docker can rebuild only the affected layer instead of recreating everything from scratch.

---

## 7. Port Binding

Port binding is used to connect the host machine's port to the container's port.

### Syntax
```bash
docker run -p host_port:container_port image_name
```

### Example
```bash
docker run -d -e MYSQL_ROOT_PASSWORD=secret --name mysql-latest -p8080:3306 mysql
```

### Explanation
- Host port: `8080`
- Container port: `3306`
- This means if you access `localhost:8080` on your machine, it will be forwarded to port `3306` inside the container.

### Important note
A single host port can be bound to only one container at a time. So if port 8080 is already used, you cannot bind another container to that same host port unless you stop or change the mapping.

---

## 8. Summary

Docker is a tool that helps package and run applications in isolated containers. It solves environment mismatch problems and makes deployment easier and more reliable.

### Key points to remember
- Docker image = blueprint
- Docker container = running instance of image
- Images are shared and reusable
- Containers are lightweight and portable
- Layers make images efficient and modular
- Port binding allows access to app services from the host machine

### Quick revision
```bash
docker pull image_name
docker images
docker run image_name
docker ps
docker ps -a
docker start container_name
docker stop container_name
docker rm container_name
docker rmi image_name
```

---

## 9. Troubleshooting Commands

Sometimes a container may not behave as expected. Docker provides useful commands to inspect and debug running containers.

### View container logs
```bash
docker logs container_id
```
Or:
```bash
docker logs container_name
```
This shows the logs produced by the container, which is helpful for identifying runtime errors.

### Execute a command inside a running container
```bash
docker exec -it container_id /bin/bash
```
Or:
```bash
docker exec -it container_name /bin/bash
```
This opens an interactive shell inside the running container so you can inspect files, environment variables, or configuration.

---

## 10. Docker vs Virtual Machine

Docker and virtual machines both help isolate applications, but they work in different ways.

### Docker
Docker virtualizes only the application layer. It shares the host operating system kernel and runs containers as lightweight isolated processes.

### Virtual Machine
A virtual machine virtualizes the host OS kernel and the application layer. It runs a full guest operating system inside a hypervisor, which makes it heavier and slower.

### Comparison
- Docker is faster
- Docker is lighter in size
- Docker uses fewer system resources
- Docker is more portable and efficient for app deployment
- Virtual machines are more complete in isolation but use more memory and CPU

### Why Docker is preferred for modern apps
Docker is often preferred because it is:
- lightweight
- quick to start
- easy to share and deploy
- suitable for microservices and containers

Docker Desktop is used to run Docker containers across different operating systems, helping developers work on platforms like Windows, macOS, and Linux more smoothly.

---

## 11. Final note

Docker allows developers to build, ship, and run applications consistently across multiple environments. It is one of the most important tools in modern software development, DevOps, and deployment workflows.
