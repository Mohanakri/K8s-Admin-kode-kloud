# Core Concepts Section Introduction

 - Take me to the [Video Tutorial](https://kodekloud.com/topic/core-concepts-section-introduction/)
 
In this section, we will take a look at the below
- Cluster Architecture  
- API Primitives
- Services & Other Network Primitives


[Kubernetes Architecture and Components with Diagram.pdf](https://github.com/Mohanakri/K8s-Admin-kode-kloud/files/14468948/Kubernetes.Architecture.and.Components.with.Diagram.pdf)





Here's a summary o[Uploading Kubernetes Architecture and Components with Diagram.pdf…]()
f the article:

- **Overview of Kubernetes Cluster Architecture:**
  - High-level view of Kubernetes architecture and its components.
  - Explanation of roles, responsibilities, and configurations of components.
  - Practice test to identify details about the cluster components.

- **Analogy with Ships:**
  - Kubernetes hosts applications as containers for automated deployment.
  - Analogy with cargo ships (nodes) and control ships (master node).
  - Cargo ships load containers, while control ships manage the process.

- **Master Node and Control Plane Components:**
  - Master node manages the Kubernetes cluster.
  - Control plane components include etcd for key-value store, scheduler for pod placement, controllers for managing nodes and replication.

- **Schedulers:**
  - Assigns pods to nodes based on resource requirements, node capacity, and policies.
  - Uses criteria like taints, tolerations, and node affinity rules.
  - Customizable and will be discussed in detail later.

- **Controllers and Specialized Tasks:**
  - Controllers in Kubernetes handle various specialized tasks.
  - Node controller manages node onboarding and availability.
  - Replication controller ensures the desired number of containers in a group.

- **Kube API Server:**
  - Primary management component orchestrating cluster operations.
  - Exposes Kubernetes API for management operations.
  - Used by controllers to monitor and make changes.

- **Container Runtime Engine:**
  - Necessary for running containers.
  - Docker is a popular choice, but Kubernetes supports others like ContainerD and Rocket.
  - Required on all nodes, including master nodes if controlling components are containerized.

- **Kubelet and Kube Proxy Service:**
  - Kubelet is the "captain" on each node, managing containers based on instructions from the API server.
  - Kube Proxy Service enables communication between containers on worker nodes.
  - Monitors node and container status, sends reports to API server.

- **Summary of Components:**
  - Master nodes include etcd, kube-scheduler, controllers, and kube API server.
  - Worker nodes have kubelet and kube proxy service for container management and communication.
  
This overview covers the Kubernetes cluster architecture, components, their roles, and how they interact in managing and deploying applications as containers. Further details on each component will be discussed in upcoming lectures.

k8s reference docs:
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
- https://kubernetes.io/docs/concepts/architecture/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/services-networking/

