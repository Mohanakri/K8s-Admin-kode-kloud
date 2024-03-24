# Commands and Arguments in Docker
  - Take me to [Video Tutorial](https://kodekloud.com/topic/commands-and-arguments-in-docker/)


Here's a summary of the article:

### Introduction
- The lecture covers commands and arguments in a pod definition file in Kubernetes.
- Explains the importance of understanding these concepts, often overlooked in certifications.

### Docker and Commands
- Docker containers are meant to run specific tasks or processes, not to host an operating system.
- The `CMD` instruction in Dockerfiles defines the program to run within the container.
- When running a container from an Ubuntu image, the default command is usually `bash`.

### Specifying Commands
- To override the default command, append a command to `docker run`.
  - Example: `docker run ubuntu sleep 5`
- Create a new image with a specified command:
  - Use `CMD` instruction in Dockerfile.
  - Specify the command as a shell form or in a JSON array format.

### Entry Point Instruction
- The `ENTRYPOINT` instruction specifies the program to run when the container starts.
- Command line parameters are appended to the entry point.
- Difference between `CMD` and `ENTRYPOINT`:
  - `CMD`: Replaces entire command line parameters.
  - `ENTRYPOINT`: Appends command line parameters.

### Combining Entry Point and Command
- Use both `ENTRYPOINT` and `CMD` to set default and override commands.
- Example:
  ```dockerfile
  ENTRYPOINT ["sleep"]
  CMD ["5"]
  ```

### Runtime Modifications
- Modify the entry point during runtime with `docker run` options.
- Example:
  ```bash
  docker run --entrypoint sleep2.0 ubuntu-sleeper 10
  ```

### Summary
- Understanding commands and arguments is crucial for defining the behavior of containers in Kubernetes.
- `CMD` and `ENTRYPOINT` instructions in Dockerfiles control the default behavior of containers.
- Use JSON array format to specify commands and arguments.
- `ENTRYPOINT` appends command line parameters, while `CMD` replaces them entirely.

The lecture explains the significance of commands and arguments in Docker containers, how to override default commands, and the difference between `CMD` and `ENTRYPOINT` instructions. It also covers how to combine these instructions and modify the behavior of containers at runtime. Understanding these concepts is essential for managing and defining container behavior effectively in Kubernetes.

_____________________________________________________________________________________________________________________________-
In this section, we will take a look at commands and arguments in docker

- To run a docker container
  ```
  $ docker run ubuntu
  ```
- To list running containers
  ```
  $ docker ps 
  ```
- To list all containers including that are stopped
  ```
  $ docker ps -a
  ```
  
  ![dc](../../images/dc.PNG)
  
#### Unlike virtual machines, containers are not meant to host operating system.
- Containers are meant to run a specific task or process such as to host an instance of a webserver or application server or a database server etc.

  ![ex](../../images/ex.PNG)
  
  
#### How do you specify a different command to start the container?
- One Option is to append a command to the docker run command and that way it overrides the default command specified within the image.
  ```
  $ docker run ubuntu sleep 5
  ```
- This way when the container starts it runs the sleep program, waits for 5 seconds and then exists. How do you make that change permanent?
  
  ![sleep](../../images/sleep.PNG)
  
- There are different ways of specifying the command either the command simply as is in a shell form or in a JSON array format.
 
  ![sleep1](../../images/sleep1.PNG)
  
- Now, build the docker image
  ```
  $ docker build -t ubuntu-sleeper .
  ```
- Run docker container
  ```
  $ docker run ubuntu-sleeper
  ```
  
  ![sleep2](../../images/sleep2.PNG)
  
## Entrypoint Instruction
- The entrypoint instruction is like the command instruction as in you can specify the program that will be run when the container starts and whatever you specify on the command line.

#### K8s Reference Docs
- https://docs.docker.com/engine/reference/builder/#cmd
