# Core Concepts Section Introduction

 - Take me to the [Video Tutorial](https://kodekloud.com/topic/core-concepts-section-introduction/)
 
In this section, we will take a look at the below
- Cluster Architecture  
- API Primitives
- Services & Other Network Primitives


Hypervisors and container runtimes are both technologies used for virtualization and isolation but operate at different levels and have distinct characteristics:

1. **Hypervisor**:

   - **Type**: Hypervisors are classified as either Type 1 (bare-metal) or Type 2 (hosted).
   
   - **Resource Allocation**: Hypervisors virtualize physical hardware resources, including CPU, memory, storage, and networking, and allocate them to virtual machines.
   
   - **Isolation**: Each virtual machine created by a hypervisor operates as if it were running on a dedicated physical machine. They are isolated from each other and from the host system.
   
   - **Operating System**: Each virtual machine typically runs its own operating system instance.
   
   - **Overhead**: Hypervisors tend to have higher overhead compared to container runtimes due to the emulation of hardware resources and the need for separate operating system instances for each VM.
   
   - **Use Cases**: Hypervisors are commonly used for server virtualization in data centers, desktop virtualization, and cloud computing platforms.

2. **Container Runtime**:

   - **Type**: Container runtimes provide lightweight virtualization at the application level.
   
   - **Resource Allocation**: Container runtimes leverage the host operating system's kernel and resources. Containers share the host OS kernel but have separate namespaces for processes, networking, and file systems.
   
   - **Isolation**: Containers are isolated from each other using namespaces and control groups (cgroups). However, they share the same underlying host kernel.
   
   - **Operating System**: Containers share the host operating system kernel, which reduces overhead compared to virtual machines.
   
   - **Overhead**: Container runtimes have lower overhead compared to hypervisors because they do not need to emulate hardware resources or maintain separate operating system instances for each container.
   
   - **Use Cases**: Containers are commonly used for microservices architectures, application packaging, deployment, and scaling in cloud-native environments. They are also used for software development, testing, and continuous integration/continuous deployment (CI/CD) pipelines.

In summary, while both hypervisors and container runtimes provide virtualization and isolation, they operate at different levels of the stack and have distinct characteristics. Hypervisors virtualize physical hardware resources and create isolated virtual machines with separate operating system instances, while container runtimes provide lightweight virtualization at the application level, enabling the creation of containers that share the host operating system kernel. Each technology has its own advantages and use cases, and they are often complementary in modern IT environments.



k8s reference docs:
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
- https://kubernetes.io/docs/concepts/architecture/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/services-networking/

