# Docker Basics

Introduction to Docker: images, containers, and common commands.

---

## Key Concepts

| Term | Description |
|---|---|
| **Image** | Read-only template used to create containers |
| **Container** | Running instance of an image |
| **Dockerfile** | Script that defines how to build an image |
| **Registry** | Storage for images (e.g., Docker Hub, ECR) |
| **Volume** | Persistent storage that outlives a container |
| **Network** | Virtual network connecting containers |

---

## Installation Check

```bash
docker --version
docker info
```

---

## Working with Images

```bash
docker pull nginx                         # Pull an image from Docker Hub
docker pull nginx:1.25                    # Pull a specific tag
docker images                             # List local images
docker rmi nginx                          # Remove an image
docker image prune                        # Remove dangling images
docker image prune -a                     # Remove all unused images
```

---

## Running Containers

```bash
docker run nginx                          # Run a container (foreground)
docker run -d nginx                       # Run in detached (background) mode
docker run -d -p 8080:80 nginx            # Map host port 8080 → container port 80
docker run -d --name my-nginx nginx       # Assign a name
docker run -it ubuntu bash                # Interactive mode with a shell
docker run --rm ubuntu echo "hello"       # Remove container after it exits
docker run -e MY_VAR=value nginx          # Set an environment variable
docker run -v /host/path:/container/path nginx  # Bind mount a volume
```

---

## Managing Containers

```bash
docker ps                                 # List running containers
docker ps -a                              # List all containers (including stopped)
docker stop my-nginx                      # Gracefully stop a container
docker start my-nginx                     # Start a stopped container
docker restart my-nginx                   # Restart a container
docker rm my-nginx                        # Remove a stopped container
docker rm -f my-nginx                     # Force-remove a running container
docker container prune                    # Remove all stopped containers
```

---

## Inspecting & Debugging

```bash
docker logs my-nginx                      # View container logs
docker logs -f my-nginx                   # Follow logs in real time
docker exec -it my-nginx bash             # Open a shell in a running container
docker inspect my-nginx                   # Detailed container metadata (JSON)
docker stats                              # Live resource usage (CPU, memory)
docker top my-nginx                       # Processes running inside a container
docker cp my-nginx:/etc/nginx/nginx.conf ./nginx.conf  # Copy file from container
```

---

## Building Images

```bash
docker build -t my-app:1.0 .             # Build image from Dockerfile in current dir
docker build -t my-app:1.0 -f Dockerfile.prod .  # Use a specific Dockerfile
docker tag my-app:1.0 my-app:latest      # Tag an image
docker push myrepo/my-app:1.0            # Push to a registry
```

### Example Dockerfile

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

---

## Volumes

```bash
docker volume create my-data              # Create a named volume
docker volume ls                          # List volumes
docker volume inspect my-data            # Volume details
docker volume rm my-data                 # Remove a volume
docker run -v my-data:/data nginx        # Use a named volume
```

---

## Networks

```bash
docker network ls                         # List networks
docker network create my-net             # Create a custom bridge network
docker run -d --network my-net --name web nginx
docker run -d --network my-net --name app my-app
# Containers on the same network can reach each other by name
docker network inspect my-net
docker network rm my-net
```

---

## Clean Up

```bash
docker system prune                       # Remove all stopped containers, unused networks, dangling images
docker system prune -a                    # Also remove unused images
docker system prune --volumes            # Also remove unused volumes
docker system df                          # Show disk usage
```
