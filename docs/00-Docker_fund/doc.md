## Docker Containers vs Virtual Machines
Virtual machines (VMs) and Docker containers are two widely used technologies in modern computing environments, although they have different uses and benefits. Making an informed choice on which technology to choose for a given use case requires an understanding of their differences.

![image](https://github.com/user-attachments/assets/4230033e-1628-4aa4-b3bd-15f85aded076)

## Docker Architecture
Docker uses a client-server architecture. The Docker client communicates with the Docker daemon, which builds, manages, and distributes your Docker containers. The Docker daemon does all the heavy lifting.

A Docker client can also be connected to a remote Docker daemon, or the daemon and client can operate on the same machine. They communicate over a REST API, over UNIX sockets, or a network interface.

Docker Architecture
![image](https://github.com/user-attachments/assets/afbb6343-9b14-4e81-b8f1-20d89deacaf0)
Docker's architecture is designed around a client-server model to manage and run containers. Here's a breakdown of its main components and how they interact:

### 1. **Docker Client**

   - The Docker Client (`docker`) is the primary way users interact with Docker. It provides a command-line interface (CLI) through which users send commands to the Docker Daemon (server), such as `docker run` or `docker pull`.
   - The client communicates with the Docker Daemon using REST APIs, which enables Docker’s functionality to be accessible over different platforms.

### 2. **Docker Daemon (dockerd)**

   - The Docker Daemon (`dockerd`) is the core component that performs all container management tasks, including building, running, and managing Docker containers.
   - It listens for commands from the Docker Client and also communicates with other daemons to manage Docker services in a cluster environment.

### 3. **Docker Engine**

   - Docker Engine is the complete package that includes the Docker Daemon, the Docker API, and the CLI.
   - It’s responsible for enabling all Docker features, such as image building, containerization, and orchestration.

### 4. **Docker Images**

   - A Docker Image is a read-only template that contains the application code, runtime, libraries, environment variables, and all dependencies required to run a containerized application.
   - Images are built from a `Dockerfile` using the `docker build` command. Once built, they can be stored in registries (such as Docker Hub) and shared or reused across environments.
   - Each time a container is launched, Docker pulls a copy of the image to create an isolated container environment.

### 5. **Containers**

   - Containers are running instances of Docker images and represent isolated processes. Each container has its own filesystem, networking, and process space but shares the same OS kernel with the host.
   - Containers are lightweight, start quickly, and consume minimal resources compared to virtual machines because they share the host's OS kernel.
   - Containers are managed by the Docker Daemon, and you can start, stop, or remove containers through the Docker Client.

### 6. **Docker Registry**

   - Docker Registry is a storage and distribution system for Docker images. Public registries like **Docker Hub** or private registries can store and distribute container images.
   - When you run `docker pull <image-name>`, Docker retrieves the specified image from the registry, while `docker push` uploads images to a registry.
   - Docker Hub, the default public registry, provides a wide range of official and community images that can be used as base images for applications.

### 7. **Docker Network**

   - Docker Networks allow containers to communicate with each other, either within the same host or across multiple hosts in a Docker Swarm or Kubernetes cluster.
   - Docker provides different networking options like **Bridge**, **Host**, and **Overlay** networks, depending on the required isolation and connectivity.

### 8. **Docker Storage**

   - Docker Storage manages persistent storage for containers, allowing data to be shared across containers or retained after a container is stopped.
   - Options include **Volumes** (managed by Docker) and **Bind Mounts** (linked to a specific file path on the host).

### Docker Architecture Workflow

1. **Image Building**: Using a Dockerfile, the Docker Client sends a request to the Docker Daemon to build an image, which is then saved in the local cache or uploaded to a registry.

2. **Running a Container**: When a container is run, the Docker Daemon creates a new isolated environment based on the specified image and manages it according to the user's instructions.

3. **Networking**: Docker sets up the network as defined, allowing containers to communicate as needed.

4. **Storage Management**: Persistent data can be stored in volumes or bind mounts, which containers can access based on defined configurations.

This design makes Docker a powerful, modular, and efficient containerization tool for consistent and isolated application deployment across different environments.
---------------------------------------------------------------------------------------------------------
Here’s a list of common Docker commands, categorized by purpose:

### 1. **Basic Commands**

   - `docker version`: Shows Docker client and server version.
   - `docker info`: Provides system-wide information about Docker.
   - `docker help`: Lists all Docker commands.

### 2. **Container Management**

   - `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`: Creates and starts a container from an image.
   - `docker start <container_name or ID>`: Starts a stopped container.
   - `docker stop <container_name or ID>`: Stops a running container.
   - `docker restart <container_name or ID>`: Restarts a container.
   - `docker pause <container_name or ID>`: Pauses all processes in a container.
   - `docker unpause <container_name or ID>`: Resumes a paused container.
   - `docker rm <container_name or ID>`: Removes a stopped container.
   - `docker kill <container_name or ID>`: Forcefully stops a container.
   - `docker exec -it <container_name or ID> <command>`: Executes a command in a running container (e.g., `bash` for an interactive shell).
   - `docker attach <container_name or ID>`: Connects to a running container.
   - `docker logs <container_name or ID>`: Retrieves logs of a container.

### 3. **Image Management**

   - `docker images`: Lists all images on the system.
   - `docker pull <image_name>`: Downloads an image from a Docker registry.
   - `docker push <image_name>`: Uploads an image to a Docker registry.
   - `docker build -t <image_name> .`: Builds an image from a Dockerfile in the current directory.
   - `docker rmi <image_name or ID>`: Removes an image.
   - `docker tag <source_image> <target_image>`: Tags an image for easier identification.
   - `docker save -o <file_name.tar> <image_name>`: Saves an image as a tar file.
   - `docker load -i <file_name.tar>`: Loads an image from a tar file.

### 4. **Dockerfile Commands**

   - `FROM <image>`: Specifies the base image.  command:1.docker image ls > txt.txt 2.grep 'ago' txt.txt | wc -l
   - `COPY <src> <dest>`: Copies files or directories to the image.
   - `RUN <command>`: Executes commands in the image.
   - `CMD <command>`: Specifies the default command to run in a container.
   - `EXPOSE <port>`: Declares the port on which the container listens.
   - `ENV <key> <value>`: Sets environment variables.
   - `ENTRYPOINT <command>`: Defines a command to run on container start.
   - `WORKDIR <directory>`: Sets the working directory.

### 5. **Network Management**

   - `docker network ls`: Lists all Docker networks.
   - `docker network create <network_name>`: Creates a new network.
   - `docker network rm <network_name>`: Removes a Docker network.
   - `docker network inspect <network_name>`: Displays network details.
   - `docker network connect <network_name> <container_name or ID>`: Connects a container to a network.
   - `docker network disconnect <network_name> <container_name or ID>`: Disconnects a container from a network.

### 6. **Volume Management**

   - `docker volume ls`: Lists all volumes.
   - `docker volume create <volume_name>`: Creates a new volume.
   - `docker volume rm <volume_name>`: Removes a volume.
   - `docker volume inspect <volume_name>`: Displays volume details.

### 7. **Inspect and Stats**

   - `docker ps`: Lists all running containers.
   - `docker ps -a`: Lists all containers (including stopped ones).
   - `docker inspect <container_name or ID>`: Retrieves low-level information about a container.
   - `docker stats`: Displays a live stream of container resource usage.
   - `docker top <container_name or ID>`: Displays processes running inside a container.

### 8. **Docker Compose**

   - `docker-compose up`: Builds and starts containers defined in a `docker-compose.yml` file.
   - `docker-compose down`: Stops and removes containers defined in a `docker-compose.yml`.
   - `docker-compose build`: Builds or rebuilds images.
   - `docker-compose start`: Starts services defined in a `docker-compose.yml`.
   - `docker-compose stop`: Stops services defined in a `docker-compose.yml`.

### 9. **Container Export and Import**

   - `docker export <container_name or ID> > <file_name.tar>`: Exports a container filesystem as a tar archive.
   - `docker import <file_name.tar> <new_image_name>`: Creates an image from a tar archive.

### 10. **Clean-Up Commands**

   - `docker system prune`: Removes unused data (stopped containers, networks, unused images, etc.).
   - `docker container prune`: Removes stopped containers.
   - `docker image prune`: Removes unused images.
   - `docker volume prune`: Removes unused volumes.
   - `docker network prune`: Removes unused networks.

### 11. **Miscellaneous Commands**

   - `docker rename <old_name> <new_name>`: Renames a container.
   - `docker history <image_name or ID>`: Shows the history of an image.
   - `docker update <container_name or ID> <options>`: Updates configuration of a container (like memory limits).

Each command can be extended with options, making Docker flexible and powerful for managing containerized applications.
----------------------------------------------------------------------------------------------
Docker Images and Docker Compose are integral parts of Docker that facilitate the creation and management of containerized applications. Below is an overview of both, along with essential commands.

---

### 1. Docker Images

A **Docker image** is a lightweight, standalone, executable software package that includes everything needed to run an application, including the code, runtime, libraries, environment variables, and configuration files. Images are read-only templates used to create containers.

#### Key Concepts
- **Layers**: Images are made up of layers, each representing a set of changes or instructions. Layers are cached and reused for efficiency.
- **Dockerfile**: A text file that contains instructions to build a Docker image. It defines what goes into the image and how it should be constructed.

#### Essential Docker Image Commands
1. **Image Management Commands**:
   - `docker images`: Lists all available images on the local machine.
   - `docker rmi <image_id_or_name>`: Deletes an image from the local machine.
   - `docker pull <image_name>`: Downloads an image from Docker Hub or another registry.
   - `docker push <image_name>`: Uploads an image to a registry.
   - `docker tag <source_image> <target_image>`: Tags an image with a new name or tag.

2. **Building Images**:
   - `docker build -t <image_name> .`: Builds a Docker image from a Dockerfile in the current directory. The `-t` flag tags the image with a name.
   - `docker build -t <image_name>:<tag> .`: Builds the image with a specific tag (e.g., `latest`, `v1.0`).

3. **Inspecting Images**:
   - `docker inspect <image_id_or_name>`: Displays detailed information about an image, including its configuration, layers, and history.
   - `docker history <image_id_or_name>`: Shows the history of an image, including each layer's size and creation time.

4. **Running Containers from Images**:
   - `docker run -d --name <container_name> <image_name>`: Creates and starts a new container from the specified image in detached mode.
   - `docker run -it <image_name> <command>`: Runs a container and opens an interactive terminal for executing commands.

#### Example of Building and Running a Docker Image
1. Create a `Dockerfile`:
   ```dockerfile
   FROM ubuntu:latest
   RUN apt-get update && apt-get install -y python3
   COPY app.py /app.py
   CMD ["python3", "/app.py"]
   ```

2. Build the image:
   ```bash
   docker build -t my-python-app .
   ```

3. Run the container:
   ```bash
   docker run -d --name my-python-container my-python-app
   ```

---
----------------------------
A **Dockerfile** is a script containing a series of instructions on how to build a Docker image. Each instruction in a Dockerfile creates a layer in the image, and Docker uses these layers to create a final image that can be run as a container. Below is an explanation of the main components of a Dockerfile and a detailed comparison of `ENTRYPOINT` and `CMD`.

### Key Components of a Dockerfile

1. **FROM**: Specifies the base image to use for the new image.
   ```dockerfile
   FROM ubuntu:latest
   ```

2. **RUN**: Executes a command in the container at build time, such as installing software.
   ```dockerfile
   RUN apt-get update && apt-get install -y python3
   ```

3. **COPY**: Copies files or directories from the host filesystem into the container.
   ```dockerfile
   COPY app.py /app.py
   ```

4. **ADD**: Similar to `COPY`, but also supports extracting tar files and fetching files from URLs.
   ```dockerfile
   ADD myapp.tar.gz /app
   ```

5. **CMD**: Provides defaults for an executing container. If there are multiple `CMD` instructions in a Dockerfile, only the last one takes effect.

6. **ENTRYPOINT**: Configures a container that will run as an executable. Unlike `CMD`, it allows you to set a command that cannot be overridden when running the container.

7. **ENV**: Sets environment variables in the container.
   ```dockerfile
   ENV APP_ENV=production
   ```

8. **WORKDIR**: Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions that follow it.
   ```dockerfile
   WORKDIR /app
   ```

9. **EXPOSE**: Informs Docker that the container listens on the specified network ports at runtime.
   ```dockerfile
   EXPOSE 80
   ```

10. **VOLUME**: Creates a mount point with the specified path and marks it as holding externally mounted volumes from native host or other containers.
   ```dockerfile
   VOLUME ["/data"]
   ```

### Difference Between `ENTRYPOINT` and `CMD`

Both `ENTRYPOINT` and `CMD` are used to define the commands that run when a container is started, but they have different purposes and behaviors.

| Feature               | `CMD`                                      | `ENTRYPOINT`                               |
|-----------------------|-------------------------------------------|-------------------------------------------|
| **Purpose**           | Provides defaults for executing a container. | Sets a command that is always executed.   |
| **Override Behavior** | Can be overridden by command-line arguments. | Cannot be overridden; command can be appended to it. |
| **Syntax**            | Can be used in two forms:                | Used in two forms:                        |
|                       | 1. Exec form: `CMD ["executable", "param1", "param2"]` | 1. Exec form: `ENTRYPOINT ["executable", "param1"]` |
|                       | 2. Shell form: `CMD command param1 param2` | 2. Shell form: `ENTRYPOINT command param1` |
| **Use Case**          | Best for specifying default arguments for the command. | Best for setting the main command of the container. |

### Examples

#### Using CMD
```dockerfile
FROM ubuntu:latest
COPY app.py /app.py
CMD ["python3", "/app.py"]  # Default command to run when container starts
```
- If you run `docker run <image_name>`, it will execute `python3 /app.py`.
- If you run `docker run <image_name> python3 other_app.py`, it will override the default command and run `other_app.py` instead.

#### Using ENTRYPOINT
```dockerfile
FROM ubuntu:latest
COPY app.py /app.py
ENTRYPOINT ["python3", "/app.py"]  # Main command that runs when the container starts
```
- If you run `docker run <image_name>`, it will execute `python3 /app.py`.
- If you run `docker run <image_name> other_app.py`, it will run `python3 other_app.py`, treating `other_app.py` as an argument to the `ENTRYPOINT`.

#### Combined Example
You can combine both `ENTRYPOINT` and `CMD` to provide default parameters.
```dockerfile
FROM ubuntu:latest
COPY app.py /app.py
ENTRYPOINT ["python3", "/app.py"]
CMD ["--help"]  # Default argument if none is provided
```
- Running `docker run <image_name>` will execute `python3 /app.py --help`.
- Running `docker run <image_name> --version` will execute `python3 /app.py --version`.

### Conclusion

- Use `CMD` when you want to provide default parameters that can be easily overridden.
- Use `ENTRYPOINT` when you want to specify the main command for your container that should always be executed, regardless of the arguments provided during container startup.
- Both can be used together to achieve flexible and effective command configurations in your Docker containers.
--------------------------------------------------------------
### 2. Docker Compose

**Docker Compose** is a tool that simplifies the management of multi-container Docker applications. It uses a YAML file (`docker-compose.yml`) to define the services, networks, and volumes needed for an application.

#### Key Concepts
- **Services**: Each service corresponds to a container and is defined in the `docker-compose.yml` file.
- **Networks**: Docker Compose automatically creates a network for the defined services to communicate.
- **Volumes**: Data persistence for services can be managed through volumes.

#### Essential Docker Compose Commands
1. **Basic Commands**:
   - `docker-compose up`: Starts the defined services in the background (detached mode). Use `-d` for detached mode.
   - `docker-compose down`: Stops and removes all containers defined in the `docker-compose.yml` file.
   - `docker-compose build`: Builds the images defined in the `docker-compose.yml` file.
   - `docker-compose logs`: Shows logs from the services started by Docker Compose.

2. **Service Management**:
   - `docker-compose ps`: Lists the running services (containers) defined in the Docker Compose file.
   - `docker-compose exec <service_name> <command>`: Executes a command in a running container of the specified service.
   - `docker-compose stop`: Stops the services defined in the Docker Compose file without removing them.
   - `docker-compose start`: Starts previously stopped services.

3. **Scaling Services**:
   - `docker-compose up --scale <service_name>=<number>`: Scales a service to the specified number of replicas.

#### Example of a `docker-compose.yml` File
```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  
  app:
    build: .
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

### Running Docker Compose
1. To start all services:
   ```bash
   docker-compose up -d
   ```

2. To stop all services:
   ```bash
   docker-compose down
   ```

3. To view logs:
   ```bash
   docker-compose logs
   ```

4. To scale the `web` service:
   ```bash
   docker-compose up --scale web=3
   ```

---

### Summary of Commands

| Command Type       | Command Example                               | Description                              |
|--------------------|-----------------------------------------------|------------------------------------------|
| **Image Management**| `docker images`                              | Lists all local images                   |
|                    | `docker rmi <image_id>`                      | Removes an image                         |
|                    | `docker pull <image_name>`                   | Pulls an image from a registry           |
|                    | `docker push <image_name>`                   | Pushes an image to a registry            |
| **Build Images**    | `docker build -t <image_name> .`             | Builds an image from a Dockerfile        |
|                    | `docker run -d --name <container_name> <image_name>` | Runs a container from an image        |
| **Docker Compose**  | `docker-compose up -d`                       | Starts services defined in `docker-compose.yml` |
|                    | `docker-compose down`                        | Stops and removes services                |
|                    | `docker-compose logs`                        | Shows logs from services                  |
|                    | `docker-compose ps`                          | Lists running services                    |

This guide provides a comprehensive overview of Docker Images and Docker Compose, including key commands that are essential for managing containerized applications effectively.
-----------------------------------------------------------------------------------


Docker Engine, Storage, and Networking are foundational components for containerizing applications. Below is a breakdown of each component along with commonly used commands.

---

### 1. Docker Engine

Docker Engine is the core of Docker, consisting of:
- **Docker Daemon (`dockerd`)**: The server-side process that manages Docker objects (images, containers, networks, and volumes).
- **Docker CLI (`docker`)**: Command-line interface to interact with Docker.
- **REST API**: Enables communication between CLI and Docker daemon.

#### Essential Docker Engine Commands
1. **System Commands**:
   - `docker version`: Displays Docker version and details about client and daemon.
   - `docker info`: Shows system-wide information about Docker.
   - `docker stats`: Monitors real-time CPU, memory, and network usage of containers.
   - `docker system prune`: Removes unused Docker objects to free up space.

2. **Container Commands**:
   - `docker run <image>`: Runs a container from an image.
   - `docker ps` or `docker container ls`: Lists running containers.
   - `docker stop <container_id>`: Stops a running container.
   - `docker start <container_id>`: Starts a stopped container.
   - `docker restart <container_id>`: Restarts a container.
   - `docker rm <container_id>`: Deletes a stopped container.

3. **Image Commands**:
   - `docker images`: Lists available images.
   - `docker build -t <image_name> .`: Builds an image from a Dockerfile in the current directory.
   - `docker pull <image_name>`: Downloads an image from a repository.
   - `docker rmi <image_id>`: Deletes an image.
   - `docker tag <source_image> <target_image>`: Tags an image with a new name or tag.

---

### 2. Docker Storage

Docker storage options help persist and manage data generated by containers. Key storage types are:

- **Volumes**: Docker-managed storage independent of the container lifecycle, ideal for persistent data.
- **Bind Mounts**: Host-managed storage tied to specific paths on the host filesystem.
- **tmpfs Mounts**: Temporary storage in host memory (RAM) that disappears when the container stops.

#### Essential Docker Storage Commands
1. **Volume Commands**:
   - `docker volume create <volume_name>`: Creates a new volume.
   - `docker volume ls`: Lists all volumes.
   - `docker volume inspect <volume_name>`: Shows details of a volume.
   - `docker volume rm <volume_name>`: Deletes a volume.
   
   **Example: Running a container with a volume**
   ```bash
   docker run -d --name mycontainer -v myvolume:/data <image_name>
   ```

2. **Bind Mount Commands**:
   **Example: Running a container with a bind mount**
   ```bash
   docker run -d --name mycontainer -v /path/on/host:/path/in/container <image_name>
   ```

3. **tmpfs Mounts**:
   **Example: Running a container with a `tmpfs` mount**
   ```bash
   docker run -d --name mycontainer --tmpfs /path/in/container <image_name>
   ```

4. **Container-specific Storage Commands**:
   - `docker inspect <container_id>`: Shows details about the container, including mounted volumes and storage.
   - `docker cp <container_id>:<path> <host_path>`: Copies files from the container to the host.

---

### 3. Docker Networking

Docker networking allows communication within and between containers, hosts, and external networks. It offers different network modes:

- **Bridge Network**: Default mode that allows communication between containers on the same host.
- **Host Network**: Containers share the host’s network stack, useful for high-performance scenarios.
- **Overlay Network**: Allows containers on different hosts to communicate, mainly used in Docker Swarm.
- **None Network**: Complete network isolation for a container.

#### Essential Docker Networking Commands
1. **Network Management Commands**:
   - `docker network ls`: Lists all networks.
   - `docker network create <network_name>`: Creates a new network.
   - `docker network inspect <network_name>`: Shows details of a specific network.
   - `docker network rm <network_name>`: Deletes a specified network.

2. **Connecting Containers to Networks**:
   - `docker network connect <network_name> <container_name>`: Connects a container to an existing network.
   - `docker network disconnect <network_name> <container_name>`: Disconnects a container from a network.

3. **Running Containers with Specific Networks**:
   **Example: Running a container on a user-defined network**
   ```bash
   docker network create my_network
   docker run -d --name mycontainer --network my_network <image_name>
   ```

4. **Port Mapping and Exposing Ports**:
   - `docker run -p <host_port>:<container_port> <image_name>`: Maps a host port to a container port to expose the service outside.
   - `docker-compose.yml`: Configure `ports` under each service to define port mappings when using Docker Compose.

#### Network Drivers
- **Bridge**: `docker network create -d bridge my_bridge_network`
- **Overlay**: `docker network create -d overlay my_overlay_network`
- **Macvlan**: `docker network create -d macvlan --subnet=<subnet> --gateway=<gateway> -o parent=<interface> my_macvlan_network`

---

### Quick Summary of Commands

| Command Type          | Command Example                                     | Description                               |
|-----------------------|-----------------------------------------------------|-------------------------------------------|
| **System Info**       | `docker info`                                       | Shows system-wide Docker info             |
| **Container Mgmt**    | `docker run -d <image_name>`                        | Runs a container in detached mode         |
|                       | `docker ps`                                         | Lists running containers                  |
|                       | `docker stop <container_id>`                        | Stops a container                         |
| **Image Mgmt**        | `docker build -t <image_name> .`                    | Builds an image from Dockerfile           |
|                       | `docker pull <image_name>`                          | Pulls an image from Docker Hub            |
|                       | `docker rmi <image_id>`                             | Deletes an image                          |
| **Storage Mgmt**      | `docker volume create <volume_name>`                | Creates a Docker volume                   |
|                       | `docker volume ls`                                  | Lists all volumes                         |
|                       | `docker volume rm <volume_name>`                    | Deletes a volume                          |
| **Networking Mgmt**   | `docker network create <network_name>`              | Creates a custom network                  |
|                       | `docker network connect <network_name> <container>` | Connects a container to a network         |
| **Port Mapping**      | `docker run -p 8080:80 <image_name>`                | Maps host port 8080 to container port 80  |

This command guide provides the essential tools for managing Docker's core Engine, Storage, and Networking functions. Each command plays a role in creating and maintaining Docker environments, offering powerful flexibility in containerized application development.

---------------------------------------------------------------------------


[Docker.pdf](https://github.com/user-attachments/files/17516688/Docker.pdf)


