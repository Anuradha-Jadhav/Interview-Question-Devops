# Docker - Comprehensive Guide

## Introduction to Docker
Docker is a **containerization platform** that enables developers to package applications and their dependencies into standardized units called **containers**. These containers run consistently across different environments, making deployment easier and more reliable.

## Docker Architecture
Docker follows a **client-server architecture**, consisting of the following components:

### 1. **Docker Client**
- The command-line interface (CLI) that interacts with the Docker Daemon.
- Commands like `docker run`, `docker build`, and `docker ps` are executed here.

### 2. **Docker Daemon (dockerd)**
- Runs in the background and manages container lifecycle.
- Handles container execution, builds images, and maintains networks and volumes.

### 3. **Docker Images**
- Read-only templates used to create containers.
- Built using a `Dockerfile` and stored in a registry.

### 4. **Docker Containers**
- Lightweight, portable, and isolated instances created from Docker images.
- Can be run, stopped, restarted, and removed as needed.

### 5. **Docker Registry**
- Central repository for storing and sharing Docker images.
- Examples: Docker Hub, AWS ECR, Azure Container Registry.

### 6. **Docker Networks**
- Provides communication between containers and the host.
- Types: **Bridge**, **Host**, **Overlay**, and **Macvlan**.

### 7. **Docker Volumes**
- Used for persistent data storage.
- Allows data sharing between containers and the host.

## Docker Workflow
1. Write a `Dockerfile`.
2. Build an image using `docker build`.
3. Push the image to a registry (`docker push`).
4. Pull the image and run a container (`docker pull` & `docker run`).

## Dockerfile - Creating Docker Images
A **Dockerfile** is a script containing instructions to build a Docker image.

Example:
```dockerfile
# Base Image
FROM ubuntu:latest

# Install dependencies
RUN apt update && apt install -y nginx

# Copy application files
COPY index.html /var/www/html/

# Start the service
CMD ["nginx", "-g", "daemon off;"]
```

### Building an Image:
```sh
docker build -t my-nginx .
```

## Container Management
### Running Containers:
```sh
docker run -d --name mycontainer nginx
```

### Listing Running Containers:
```sh
docker ps
```

### Stopping a Container:
```sh
docker stop mycontainer
```

### Removing a Container:
```sh
docker rm mycontainer
```

## Docker Compose - Multi-Container Applications
Docker Compose is a tool for defining and managing **multi-container applications** using a `docker-compose.yml` file.

Example:
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  database:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

### Running Services with Compose:
```sh
docker-compose up -d
```

### Stopping Services:
```sh
docker-compose down
```

## Docker Networking
Docker provides several networking options:
1. **Bridge Network** (default) - Allows communication between containers on the same host.
2. **Host Network** - Containers share the host’s network.
3. **Overlay Network** - Used in **Swarm mode** for multi-host communication.

### Creating a Custom Network:
```sh
docker network create mynetwork
```

## Persistent Storage with Docker Volumes
Docker Volumes enable data persistence between container restarts.

### Creating a Volume:
```sh
docker volume create myvolume
```

### Attaching a Volume to a Container:
```sh
docker run -d --name mycontainer -v myvolume:/data nginx
```

## Docker Security Best Practices
- Use official and minimal base images.
- Run containers as non-root users.
- Scan images for vulnerabilities (`docker scan` or `Trivy`).
- Limit container privileges (`--cap-drop` and `--cap-add`).
- Enable resource limits (`--memory`, `--cpu-shares`).

## Docker Swarm - Container Orchestration
Docker Swarm is a native clustering tool for managing multiple Docker nodes.

### Initializing Swarm Mode:
```sh
docker swarm init
```

### Deploying a Service:
```sh
docker service create --name web --replicas 3 -p 80:80 nginx
```

## Kubernetes vs. Docker Swarm
| Feature          | Docker Swarm | Kubernetes |
|----------------|-------------|------------|
| Ease of Setup | Easy | Complex |
| Scalability | Medium | High |
| Load Balancing | Basic | Advanced |
| Networking | Simple Overlay | Robust CNI |

## Conclusion
Docker is a powerful containerization tool that simplifies application deployment. When combined with **Docker Compose** and **Swarm/Kubernetes**, it provides an efficient DevOps workflow for managing containers at scale.

## Further Learning
- [Docker Docs](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Kubernetes Docs](https://kubernetes.io/docs/)

