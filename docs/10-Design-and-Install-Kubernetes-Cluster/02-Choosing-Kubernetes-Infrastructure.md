# Choosing a Kubernetes Infrastructure

  - Take me to [Lecture](https://kodekloud.com/topic/choosing-kubernetes-infrastructure/)
In this lecture, the instructor elaborates on the various infrastructure choices for hosting a Kubernetes cluster, expanding on the deployment options discussed previously. Here are the key points:

### Deployment Options
1. **Local Machine Deployment**:
   - **Linux Machines**: Install binaries manually or use automated solutions.
   - **Windows Machines**: Use virtualization software (Hyper-V, VMware Workstation, VirtualBox) to create Linux VMs for running Kubernetes.
   - **Solutions**:
     - **Minikube**: Deploys a single-node cluster using virtualization software.
     - **Kubeadm**: Can deploy single or multi-node clusters, but requires pre-provisioned hosts.

### Purpose of Deployment
- Local deployments are typically for learning, testing, and development.
- For production, both private and public cloud environments offer viable options.

### Types of Production Deployment
1. **Turnkey Solutions**:
   - Provision VMs and use tools or scripts to configure Kubernetes.
   - Responsibility for VM maintenance and upgrades lies with the user.
   - Example: **KOPS** for deploying on AWS.
2. **Hosted Solutions**:
   - Provider handles VM deployment, Kubernetes configuration, and maintenance.
   - Example: **Google Container Engine**, **OpenShift Online**, **Azure Kubernetes Service**, **Amazon Elastic Container Service for Kubernetes**.

### Popular Turnkey Solutions
- **OpenShift**: Red Hat's platform built on Kubernetes with additional tools and GUI.
- **Cloud Foundry Container Runtime**: Uses BOSH for managing Kubernetes clusters.
- **VMware Cloud PKS**: Integrates Kubernetes with VMware environments.
- **Vagrant**: Scripts for deploying Kubernetes on various cloud providers.

### Popular Hosted Solutions
- **Google Container Engine**: Easy deployment on Google Cloud.
- **OpenShift Online**: Red Hat's fully functional online Kubernetes cluster.
- **Azure Kubernetes Service**: Microsoft's hosted solution.
- **Amazon Elastic Container Service for Kubernetes**: AWS's offering.

### Choice for Learning
- For learning purposes, a local setup with VirtualBox is chosen.
- Design includes a three-node cluster (one master, two workers) on a laptop with VMs provisioned on VirtualBox.

### Conclusion
- References for further reading and more solutions are provided.
- Emphasis on practical learning by deploying a local Kubernetes cluster.

This lecture underscores the variety of ways Kubernetes can be deployed, catering to different environments and purposes, from local setups for learning to robust solutions for production in both private and public clouds.
