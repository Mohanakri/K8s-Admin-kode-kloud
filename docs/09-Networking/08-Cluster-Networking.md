# Pre-requisite Cluster Networking

  - Take me to [Lecture](https://kodekloud.com/topic/cluster-networking/)
In this lecture, the focus is on the networking configurations necessary for both master and worker nodes within a Kubernetes cluster. Here's a summary:

1. **Cluster Components**: A Kubernetes cluster consists of master and worker nodes. Each node requires at least one network interface with a configured address. It's crucial to ensure that each node has a unique hostname and MAC address, especially when cloning VMs.

2. **Required Ports**: Several ports need to be opened to facilitate communication between cluster components. These ports include:
   - Port 6443: Used by the API server for connections from kube control tools, external users, and control plane components.
   - Port 10250: Listened to by kubelets on both master and worker nodes.
   - Port 10259: Required by the kube scheduler.
   - Port 10257: Necessary for the kube controller manager.
   - Ports 30000 to 32767: Exposed by worker nodes for external service access.
   - Port 2379: Listened to by the ETCD server.
   - Port 2380: Needed for ETCD client communication in multi-master setups.

3. **Documentation Reference**: The lecture emphasizes consulting Kubernetes documentation for a comprehensive list of required ports, aiding in setting up networking configurations in firewalls, IP tables, or network security groups, especially in cloud environments like GCP, Azure, or AWS.

4. **Practice Session**: Learners are encouraged to explore networking setups in existing Kubernetes environments during the practice session. Simple exercises are provided to familiarize users with retrieving information about interfaces, IPs, hostnames, and ports.

5. **Future Exercises**: The lecture promises more challenging exercises in subsequent sessions but starts with simpler tasks to help learners acclimate to the environment and build foundational knowledge.

Overall, the lecture serves as a practical guide for setting up networking configurations in Kubernetes clusters, highlighting essential ports and emphasizing hands-on exploration for better understanding.


=================================================================================================



In this section, we will take a look at **Pre-requisite of the Cluster Networking**

- Set the unique hostname.
- Get the IP addr of the system (master and worker node).
- Check the Ports.

## IP and Hostname

- To view the hostname

```
$ hostname 
```

- To view the IP addr of the system

```
$ ip a
```


## Set the hostname

```
$ hostnamectl set-hostname <host-name>

$ exec bash
```

## View the Listening Ports of the system

```
$ netstat -nltp
```



#### References Docs

- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports
- https://kubernetes.io/docs/concepts/cluster-administration/networking/
