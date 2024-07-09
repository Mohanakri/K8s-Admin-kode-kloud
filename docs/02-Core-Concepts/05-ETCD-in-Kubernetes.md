# ETCD in Kubernetes
  - Take me to [Video Tutorial](https://kodekloud.com/topic/etcd-in-kubernetes/)


This article focuses on explaining the role of **etcd** within **Kubernetes** and how it stores crucial information about the cluster. Here's a summary of the main points covered:

- **Role of etcd in Kubernetes**:
  - **etcd** serves as the data store for various elements in the Kubernetes cluster, including nodes, pods, configurations, secrets, accounts, roles, and role bindings.
  - When you execute **kubectl get** commands, the information displayed is retrieved from the **etcd** server.
  - Any changes made to the cluster, such as adding nodes or deploying pods/replica sets, are updated within the **etcd** server. A change is considered complete only after reflecting in **etcd**.

- **Deployment Methods**:
  - Depending on the cluster setup, **etcd** can be deployed differently.
  - Two primary deployment methods are discussed:
    - **Deploying from Scratch**: Download **etcd** binaries, install them, and configure **etcd** as a service in the master node manually.
    - **Using kubeadm**: If **kubeadm** is used to set up the cluster, it automatically deploys **etcd** as a pod in the **kube-system** namespace.

- **Configuration Options for etcd**:
  - When setting up **etcd** as a service, various options, especially related to certificates, need to be configured.
  - Details about configuring **etcd** as a cluster are discussed, with a focus on options for high availability setups in Kubernetes.
  - An essential option highlighted is the **advertised client URL**, which is the address at which **etcd** listens (defaulted to IP of the server and port 2379).

- **Exploring the etcd Database**:
  - The **etcdctl** utility within the **etcd** pod allows exploration of the **etcd** database.
  - To list all keys stored by Kubernetes, the command **etcdctl get** is used.
  - Kubernetes structures its data in a specific directory format, with the root directory as the **registry** containing various constructs like nodes, pods, replica sets, and deployments.

- **High Availability Setup**:
  - In a high availability environment, there are multiple master nodes and **etcd** instances distributed across these nodes.
  - Proper configuration is crucial to ensure that **etcd** instances recognize each other, achieved by setting the right parameters in the **etcd** service configuration.
  - The **initial cluster** option is used to specify different instances of the **etcd** service in a high availability setup.

The article concludes by mentioning that high availability setups will be discussed in more detail later in the course. It provides a comprehensive overview of **etcd**'s role, deployment methods, configuration options, and the importance of understanding these aspects in managing a Kubernetes cluster.





In this section, we will take a look at ETCD role in kubernetes

## ETCD Datastore
- The ETCD Datastore stores information regarding the cluster such as **`Nodes`**, **`PODS`**, **`Configs`**, **`Secrets`**, **`Accounts`**, **`Roles`**, **`Bindings`** and **`Others`**.
- Every information you see when you run the **`kubectl get`** command is from the **`ETCD Server`**.

## Setup - Manual
- If you setup your cluster from scratch then you deploy **`ETCD`** by downloading ETCD Binaries yourself
- Installing Binaries and Configuring ETCD as a service in your master node yourself.
  ```
  $ wget -q --https-only "https://github.com/etcd-io/etcd/releases/download/v3.3.11/etcd-v3.3.11-linux-amd64.tar.gz"
  ```

  ![etcd](../../images/etcd.PNG)
  

## Setup - Kubeadm
- If you setup your cluster using **`kubeadm`** then kubeadm will deploy etcd server for you as a pod in **`kube-system`** namespace.
  ```
  $ kubectl get pods -n kube-system
  ```
  ![etcd1](../../images/etcd1.PNG)

## Explore ETCD
- To list all keys stored by kubernetes, run the below command
  ```
  $ kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --cacert=/etc/kubernetes/pki/etcd/ca.crt get / --prefix --keys-only"
  ```
- Kubernetes Stores data in a specific directory structure, the root directory is the **`registry`** and under that you have varies kubernetes constructs such as **`minions`**, **`nodes`**, **`pods`**, **`replicasets`**, **`deployments`**, **`roles`**, **`secrets`** and **`Others`**.
  
  ![etcdctl1](../../images/etcdctl1.PNG)

## ETCD in HA Environment
   - In a high availability environment, you will have multiple master nodes in your cluster that will have multiple ETCD Instances spread across the master nodes.
   - Make sure etcd instances know each other by setting the right parameter in the **`etcd.service`** configuration. The **`--initial-cluster`** option where you need to specify the different instances of the etcd service.
     ![etcd-ha](../../images/etcd-ha.PNG)



(Optional) Additional information about ETCDCTL Utility

ETCDCTL is the CLI tool used to interact with ETCD.

ETCDCTL can interact with ETCD Server using 2 API versions - Version 2 and Version 3.  By default its set to use Version 2. Each version has different sets of commands.

For example ETCDCTL version 2 supports the following commands:

etcdctl backup
etcdctl cluster-health
etcdctl mk
etcdctl mkdir
etcdctl set


Whereas the commands are different in version 3

etcdctl snapshot save 
etcdctl endpoint health
etcdctl get
etcdctl put

To set the right version of API set the environment variable ETCDCTL_API command

export ETCDCTL_API=3



When API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don't work. When API version is set to version 3, version 2 commands listed above don't work.



Apart from that, you must also specify path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. We discuss more about certificates in the security section of this course. So don't worry if this looks complex:

--cacert /etc/kubernetes/pki/etcd/ca.crt     
--cert /etc/kubernetes/pki/etcd/server.crt     
--key /etc/kubernetes/pki/etcd/server.key


So for the commands I showed in the previous video to work you must specify the ETCDCTL API version and path to certificate files. Below is the final form:



kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key" 

https://github.com/techiescamp/kubernetes-projects/tree/main/01-kubernetes-the-hard-way-aws

K8s Reference Docs:
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/setup-ha-etcd-with-kubeadm/
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/#stacked-control-plane-and-etcd-nodes
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/#external-etcd-nodes
