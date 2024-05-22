# Introduction - Kubeadm

Note that this section is for your own practice only. To our knowledge there has never been an exam question that requires you to install a cluster. Questions asking you to _upgrade an existing cluster_ are not uncommon!

  - Take me to [Introduction](https://kodekloud.com/topic/introduction-to-deployment-with-kubeadm/)

If you have an Apple M1 or M2 (Apple Silicon) machine, then please follow the separate instructions [here](../../apple-silicon/README.md).



In this lecture, the instructor introduces the kubeadm tool, which simplifies the process of setting up a Kubernetes cluster by automating various tasks and following Kubernetes best practices. Here's a summary of the key steps involved in setting up a Kubernetes cluster using kubeadm:

1. **Provision Nodes**: Start by provisioning multiple systems or virtual machines (VMs) to serve as nodes for the Kubernetes cluster. These can be physical or virtual machines.

2. **Designate Master and Worker Nodes**: Choose one node as the master and designate the rest as worker nodes.

3. **Install Container Runtime**: Install a container runtime (e.g., containerd) on all nodes to manage container lifecycle.

4. **Install kubeadm Tool**: Install the kubeadm tool on all nodes, which will assist in bootstrapping the Kubernetes cluster by installing necessary components in the correct order.

5. **Initialize Master Server**: Use kubeadm to initialize the master server, where all required components will be installed and configured.

6. **Configure Network Prerequisites**: Before joining worker nodes to the cluster, ensure that the network prerequisites are met. Kubernetes requires a specific networking solution called the POD Network, which needs to be set up between master and worker nodes.

7. **Join Worker Nodes**: Once the network setup is complete, worker nodes can be joined to the master node.

8. **Deploy Application**: With the cluster set up and worker nodes joined, you can deploy your applications onto the Kubernetes environment.

The instructor mentions that they will demonstrate setting up a Kubernetes cluster using the kubeadm tool in a local environment, highlighting the ease and simplicity of the process.

