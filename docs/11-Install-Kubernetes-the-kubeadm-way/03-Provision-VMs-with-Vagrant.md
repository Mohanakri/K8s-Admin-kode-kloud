# Provision VMs with Vagrant tool

  - Take me to [Lecture](https://kodekloud.com/topic/deploy-with-kubeadm-provision-vms-with-vagrant/)

If you want to build your own cluster, check these out. They are more up to date than the video currently :smile:

* [Kubeadm Clusters](../../kubeadm-clusters/)
* [Managed Clusters](../../managed-clusters/)


In this transcript, the instructor guides through the process of setting up a Kubernetes cluster using the kubeadm tool. Here's a summary of the steps covered:

1. **Node Creation**: Three nodes are created: one master and two worker nodes.

2. **Documentation Reference**: The instructor refers to the Kubernetes documentation, specifically the "Installing kubeadm" page, for detailed instructions.

3. **Prerequisites**: The prerequisites for the Linux host, including memory and CPU requirements, are discussed.

4. **Installing Container Runtime**: Containerd is chosen as the container runtime, and instructions for installation are provided.

5. **Cgroup Driver Configuration**: Explanation and configuration of the Cgroup driver, ensuring compatibility between kubelet and container runtime.

6. **Installing Kubeadm, Kubelet, and Kubectl**: Instructions for installing these components on all nodes.

7. **Initializing Master Node**: Running `kubeadm init` on the master node with specified flags.

8. **Configuring Permissions**: Creation of admin.conf file for cluster access and setting permissions.

9. **Deploying Pod Network**: Selection of Weave Net as the pod network add-on and deployment on the master node.

10. **Joining Worker Nodes**: Running `kubeadm join` command on worker nodes to join them to the cluster.

11. **Verification**: Verification of cluster setup by deploying an Nginx container and confirming its status.

The transcript provides detailed commands and explanations for each step, ensuring a comprehensive understanding of the Kubernetes cluster setup process.
