# Core Concepts Section Introduction

 - Take me to the [Video Tutorial](https://kodekloud.com/topic/core-concepts-section-introduction/)
In Kubernetes, several directories and folders are integral to the operation and management of the cluster and its components. These directories contain configuration files, data, logs, and other necessary files. Here's a summary of the key Kubernetes folders and their descriptions:

### 1. `/etc/kubernetes/`
- **Description**: Contains configuration files for the Kubernetes cluster and its components.
- **Contents**:
  - `admin.conf`: Configuration for the cluster administrator.
  - `controller-manager.conf`: Configuration for the controller manager.
  - `scheduler.conf`: Configuration for the scheduler.
  - `kubelet.conf`: Configuration for the kubelet.
  - `apiserver.conf`: Configuration for the API server.
  - Certificates and other security-related files.

### 2. `/var/lib/kubelet/`
- **Description**: Used by the kubelet to manage pods, container storage, and other node-level operations.
- **Contents**:
  - Pod manifests and state information.
  - Volume mounts and data for persistent volumes.
  - Configuration and runtime data for the kubelet.

### 3. `/etc/cni/net.d/`
- **Description**: Contains configuration files for Container Network Interface (CNI) plugins.
- **Contents**:
  - Network configuration files that specify how pods are networked.
  - Configuration settings for different CNI plugins like Calico, Flannel, and Weave.

### 4. `/opt/cni/bin/`
- **Description**: Stores the binary files for CNI plugins.
- **Contents**:
  - Executable files for various CNI plugins required for pod networking.

### 5. `/var/lib/etcd/`
- **Description**: Data directory for the etcd key-value store used by Kubernetes.
- **Contents**:
  - Data files containing the state of the Kubernetes cluster.
  - Snapshot files and backup data for etcd.

### 6. `/etc/systemd/system/kubelet.service.d/`
- **Description**: Contains systemd drop-in files for customizing the kubelet service.
- **Contents**:
  - Custom configuration files that modify or extend the default kubelet service settings.

### 7. `/etc/docker/`
- **Description**: Contains Docker configuration files, which are often used as the container runtime in Kubernetes.
- **Contents**:
  - `daemon.json`: Configuration file for Docker daemon settings.
  - Other Docker-related configuration files.

### 8. `/var/log/`
- **Description**: Contains log files for various Kubernetes components and system logs.
- **Contents**:
  - `kube-apiserver.log`: Logs for the Kubernetes API server.
  - `kube-scheduler.log`: Logs for the Kubernetes scheduler.
  - `kube-controller-manager.log`: Logs for the Kubernetes controller manager.
  - `kubelet.log`: Logs for the kubelet service.
  - `docker.log`: Logs for the Docker daemon.
  - System logs and logs for other Kubernetes components.

### 9. `/var/run/secrets/kubernetes.io/`
- **Description**: Contains service account tokens and secrets mounted into pods.
- **Contents**:
  - Token files and secret data for accessing the Kubernetes API.
  - Other secret data used by applications running in the cluster.

### 10. `/etc/ssl/certs/`
- **Description**: Stores SSL certificates used by Kubernetes components for secure communication.
- **Contents**:
  - CA certificates.
  - SSL/TLS certificates for secure communication between Kubernetes components.

### 11. `/usr/local/bin/`
- **Description**: Typically contains user-installed binaries, including Kubernetes command-line tools.
- **Contents**:
  - Binaries for `kubectl`, `kubeadm`, `kubelet`, and other utilities.

### 12. `/etc/kubernetes/manifests/`
- **Description**: Contains static pod manifests that are managed by the kubelet on the master node.
- **Contents**:
  - Static pod definitions for control plane components like `kube-apiserver`, `kube-controller-manager`, `kube-scheduler`, and `etcd`.

### 13. `/var/lib/kubelet/plugins/`
- **Description**: Contains plugin-related data for the kubelet.
- **Contents**:
  - Information about various plugins used by the kubelet.

These directories are essential for the functioning of a Kubernetes cluster, containing critical configuration files, runtime data, logs, and security credentials necessary for managing and operating the cluster effectively.

================================================================================================================================



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

