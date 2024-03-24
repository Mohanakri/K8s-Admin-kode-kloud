# OS Upgrades
  - Take me to [Video Tutorial](https://kodekloud.com/topic/os-upgrades/)



Here's a summary of the article on handling node maintenance in Kubernetes:

### Introduction
- The lecture discusses scenarios where nodes in a cluster need to be taken down for maintenance purposes, such as software upgrades or applying security patches.

### Impact of Node Failure on Pods
- When a node goes down, the pods running on that node become inaccessible.
- Users accessing pods on the affected node may experience service disruption depending on the pod deployment strategy.
  - Example: Users accessing pods with multiple replicas (like the blue pod) are not impacted, as other replicas serve the requests.
  - Users accessing pods without replicas (like the green pod) are impacted as there is no other instance available.

### Kubernetes Handling of Node Failure
- If a node is down for less than the `pod-eviction-timeout` (default 5 minutes), Kubernetes considers the pods as potentially recoverable.
- When the node comes back online within the timeout, kubelet restarts, and pods are recreated.
- If the node is down for longer than the timeout, Kubernetes terminates the pods on that node.
- Pods in a replica set are recreated on other nodes to maintain desired replica count.
- Pods not in a replica set are not recovered and need manual intervention.

### Node Maintenance Strategies
- **Quick Reboot:**
  - If the node is expected to come back online within the timeout, a quick upgrade and reboot can be done.
  - Suitable for nodes with workloads having replicas, ensuring no prolonged downtime.

- **Drain Node:**
  - A safer method involves draining the node of all workloads before maintenance.
  - Draining moves pods to other nodes gracefully, ensuring no downtime for applications.
  - Node becomes unschedulable (`cordoned`) to prevent new pods until maintenance is done.
  - After maintenance, the node needs to be `uncordoned` to resume scheduling.

- **Cordon Node:**
  - Marks a node as unschedulable without moving or terminating existing pods.
  - Prevents new pods from being scheduled on the node.
  - Useful when you want to prevent new workload deployments on the node.

### Conclusion
- Node maintenance in Kubernetes involves considering the impact on running pods.
- Draining a node ensures minimal disruption by gracefully moving pods to other nodes.
- Cordon marks a node unschedulable without affecting existing pods.
- After maintenance, uncordon the node to allow pod scheduling.
- The lecture encourages practicing draining, cordoning, and uncordoning nodes in the practice test section.

This lecture provides insights into managing node maintenance in Kubernetes clusters. It covers strategies such as draining nodes for planned maintenance, marking nodes as unschedulable with `cordon`, and uncordoning nodes post-maintenance. These practices ensure minimal disruption to running applications during maintenance operations. Readers are encouraged to practice these commands in the provided practice test section.

____________________________________________________________________________________________________________________________________



  
In this section, we will take a look at OS upgrades.

#### If the node was down for more than 5 minutes, then the pods are terminated from that node

  ![os](../../images/os.PNG)
  
- You can purposefully **`drain`** the node of all the workloads so that the workloads are moved to other nodes.
  ```
  $ kubectl drain node-1
  ```
- The node is also cordoned or marked as unschedulable.
- When the node is back online after a maintenance, it is still unschedulable. You then need to uncordon it.
  ```
  $ kubectl uncordon node-1
  ```
- There is also another command called cordon. Cordon simply marks a node unschedulable. Unlike drain it does not terminate or move the pods on an existing node.

  ![drain](../../images/drain.PNG)
  
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
