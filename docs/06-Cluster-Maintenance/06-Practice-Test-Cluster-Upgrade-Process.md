# Practice Test - Cluster Upgrade Process
  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-cluster-upgrade-process/)

  - cluster upgrade means components cotrol plane and caemonsets in worker nodes.
    
Solutions to practice test cluster upgrade process
- What is the current version of the cluster?
  
  <details>
  $ kubectl get nodes  or kubeadm upgrade plan (current)
  </details>
  
- How many nodes are part of this cluster?
  
  <details>
   $ kubctl get nodes
  </details>
  
- How many nodes can host workloads in this cluster?  Inspect the applications and taints set on the nodes.
  
  <details>
  $ check taints on nodes k describe node &  kubectl get pods -o wide
  </details>
  
- How many applications are hosted on the cluster?   Count the number of deployments in the default namespace.
    
  <details>
  $ kubectl get deploy
  </details>
  
- What nodes are the pods hosted on?
  
  <details>
  $ kubectl get pods -o wide
  </details>
  
- You are tasked to upgrade the cluster. User's accessing the applications must not be impacted. And you cannot provision new VMs. What strategy would you use to upgrade the cluster?
  
  <details>
  Upgrade one node at a time while moving the workloads to the other
  </details>
  
- What is the latest version available for an upgrade with the current version of the kubeadm tool installed?    Use the kubeadm tool
  
  <details>
  $ kubeadm upgrade plan
  </details>
  
- We will be upgrading the controlplane node first. Drain the controlplane node of workloads and mark it UnSchedulable

  <details>
  $ kubectl drain controlplane --ignore-daemonsets
  </details>
  
- Upgrade the controlplane components to exact version v1.30.0 ?    Upgrade the kubeadm tool (if not already), then the controlplane components, and finally the kubelet. Practice referring to the Kubernetes documentation page.
  
<details>
  To seamlessly transition from Kubernetes v1.29 to v1.30 and gain access to the packages specific to the desired Kubernetes minor version, follow these essential steps during the upgrade 
  process. This ensures that your environment is appropriately configured and aligned with the features and improvements introduced in Kubernetes v1.30.

   On the controlplane node:

  Use any text editor you prefer to open the file that defines the Kubernetes apt repository.

vim /etc/apt/sources.list.d/kubernetes.list
Update the version in the URL to the next available minor release, i.e v1.30.

deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /
After making changes, save the file and exit from your text editor. Proceed with the next instruction.

apt update

apt-cache madison kubeadm
Based on the version information displayed by apt-cache madison, it indicates that for Kubernetes version 1.30.0, the available package version is 1.30.0-1.1. Therefore, to install kubeadm for Kubernetes v1.30.0, use the following command:

apt-get install kubeadm=1.30.0-1.1
Run the following command to upgrade the Kubernetes cluster.

kubeadm upgrade plan v1.30.0

kubeadm upgrade apply v1.30.0
Note that the above steps can take a few minutes to complete.

Now, upgrade the version and restart Kubelet. Also, mark the node (in this case, the "controlplane" node) as schedulable.

apt-get install kubelet=1.30.0-1.1

systemctl daemon-reload
systemctl restart kubelet
</details>
  
- Run the command kubectl uncordon controlplane
  
  <details>
  $ kubectl uncordon controlplane
  </details>
  
- Next is the worker node. Drain the worker node of the workloads and mark it UnSchedulable
  
  <details>
  $ kubectl drain node01 --ignore-daemonsets
  </details>
  
- Upgrade the worker node to the exact version v1.30.0

<details>
 On the node01 node, run the following commands:

If you are on the controlplane node, run ssh node01 to log in to the node01.

Use any text editor you prefer to open the file that defines the Kubernetes apt repository.

vim /etc/apt/sources.list.d/kubernetes.list
Update the version in the URL to the next available minor release, i.e v1.30.

deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /
After making changes, save the file and exit from your text editor. Proceed with the next instruction.

apt update

apt-cache madison kubeadm
Based on the version information displayed by apt-cache madison, it indicates that for Kubernetes version 1.30.0, the available package version is 1.30.0-1.1. Therefore, to install kubeadm for Kubernetes v1.30.0, use the following command:

apt-get install kubeadm=1.30.0-1.1

# Upgrade the node 
kubeadm upgrade node
Now, upgrade the version and restart Kubelet.

apt-get install kubelet=1.30.0-1.1

systemctl daemon-reload

systemctl restart kubelet
Type exit or logout or enter CTRL + d to go back to the controlplane node.
</details>
  
- Remove the restriction and mark the worker node as schedulable again.
  
  <details>
  $ kubectl uncordon node01
  </details>

