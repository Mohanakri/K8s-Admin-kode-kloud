# Core Concepts Section Introduction

 - Take me to the [Video Tutorial](https://kodekloud.com/topic/core-concepts-section-introduction/)

Kernal in linux meaning :responsible for managing hardware resources, processes, memory, file systems, devices, networking, and security. It provides a stable and efficient foundation for user-space applications to run and interact with the underlying hardware. The kernel's role is crucial in ensuring the proper functioning, performance, and security of the Linux operating system.


Hypervisors and container runtimes are both technologies used for virtualization and isolation but operate at different levels and have distinct characteristics:


In summary, while both hypervisors and container runtimes provide virtualization and isolation, they operate at different levels of the stack and have distinct characteristics. Hypervisors virtualize physical hardware resources and create isolated virtual machines with separate operating system instances, while container runtimes provide lightweight virtualization at the application level, enabling the creation of containers that share the host operating system kernel. Each technology has its own advantages and use cases, and they are often complementary in modern IT environments.

_______________________________
Comparison:


kubectl create:

Use when you want to create a resource and ensure it does not already exist.
Best for one-time resource creation from manifest files.
Returns an error if the resource already exists.


kubectl apply:

Use when you want to create or update a resource based on the manifest file.
Best for managing resources in a declarative way, allowing for easier updates.
Creates the resource if it does not exist, updates it if it does.

kubectl run:

Use for quick, imperative Pod or Deployment creation.
Best for one-time actions, testing, or development.
Creates Pods or Deployments directly without using a manifest file.




k8s reference docs:
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
- https://kubernetes.io/docs/concepts/architecture/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/services-networking/

