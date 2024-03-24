# Commands and Arguments in Kubernetes
  - Take me to [Video Tutorial](https://kodekloud.com/topic/commands-and-arguments-in-kubernetes-2/)


Here's a summary of the article:

### Introduction
- The lecture focuses on commands and arguments in a Kubernetes pod.
- It builds upon a previous example of creating a Docker image (`Ubuntu sleeper`) that sleeps for a specified number of seconds.

### Creating a Pod
- To create a pod from the `Ubuntu sleeper` image:
  - Start with a blank pod definition template.
  - Specify the name of the pod and the image.

### Specifying Arguments in Pod Definition
- To override the default sleep time from 5 seconds to 10 seconds:
  - Use the `args` property in the pod definition file, in the form of an array.

### Relating to Dockerfile
- In the Dockerfile for `Ubuntu sleeper`:
  - `ENTRYPOINT` is the command run at startup.
  - `CMD` is the default parameter passed to the command.

### Corresponding Fields in Pod Definition
- `command` field in the pod definition:
  - Corresponds to the `ENTRYPOINT` instruction in the Dockerfile.
- `args` field in the pod definition:
  - Overrides the `CMD` instruction in the Dockerfile.

### Overriding Entry Point
- To override the `ENTRYPOINT` from `sleep` to `sleep 2.0`:
  - Use the `command` field in the pod definition.

### Summary
- Two fields in the pod definition correspond to Dockerfile instructions:
  1. `command` field: Overrides the `ENTRYPOINT` instruction.
  2. `args` field: Overrides the `CMD` instruction.
- Remember the distinction between `command` and `args` fields in the pod definition.

### Conclusion
- Understanding these fields is essential for configuring commands and arguments in Kubernetes pods.
- Practice viewing, configuring, and troubleshooting these aspects in Kubernetes to gain proficiency.

The lecture highlights how to specify commands and arguments in a Kubernetes pod definition file, with a focus on `command` and `args` fields. It also explains their relation to Dockerfile instructions (`ENTRYPOINT` and `CMD`). Understanding these concepts is crucial for effectively configuring and managing pod behaviors in Kubernetes deployments.


_______________________________________________________________________________________________________________




In this section, we will take a look at commands and arguments in kubernetes

- Anything that is appended to the docker run command will go into the **`args`** property of the pod definition file in the form of an array.
- The command field corresponds to the entrypoint instruction in the Dockerfile so to summarize there are 2 fields that correspond to 2 instructions in the Dockerfile.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: ubuntu-sleeper-pod
  spec:
   containers:
   - name: ubuntu-sleeper
     image: ubuntu-sleeper
     command: ["sleep2.0"]
     args: ["10"]
  ```
  ![args](../../images/args.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/
