# Practice Test - OS Upgrades
  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-os-upgrades/)
  
Solutions to practice test - OS Upgrades
- Let us explore the environment first. How many nodes do you see in the cluster?

  Including the controlplane and worker nodes.
  
  <details>
  $ kubectl get nodes
  </details>
  
- How many applications do you see hosted on the cluster?

   Check the number of deployments in the default namespace.
  
  <details>
  $ kubectl get deploy
  </details>
  
- Which nodes are the applications hosted on?
  
  <details>
  ```
  $ kubectl get pods -o wide
  ```
  </details>
  
- We need to take node01 out for maintenance. Empty the node of all applications and mark it unschedulable.
  
  <details>
  ```
  $ kubectl drain node01 --ignore-daemonsets
  ```
  </details>
  
- What nodes are the apps on now?
  
  <details>
  ```
  $ kubectl get pods -o wide
  ```
  </details>
  
- The maintenance tasks have been completed. Configure the node node01 to be schedulable again.
  
  <details>
  ```
  $ kubectl uncordon node01
  ```
  </details>
  
- How many pods are scheduled on node01 now in the default namespace?
  
  <details>
  ```
  $ kubectl get pods -o wide
  ```
  </details>
  
- Why are there no pods on node01?
  
  <details>
  ```
  Only when new pods are created they will be scheduled
  ```
  </details>
  
- Why are the pods placed on the controlplane node?

  Check the controlplane node details.
  
  <details>
  ```
  $ kubectl describe node master
  ```
  </details>
  
- Run the command kubectl drain node02 --ignore-daemonsets
  
  <details>
  ```
  $ kubectl drain node02 --ignore-daemonsets
  ```
  </details>
  
- Check the applications hosted on the node02.
  
  <details>
  ```
  node02 has a pod not part of a replicaset
  $ kubectl get pods -o wide
  ```
  </details>
  
- Check the list of pods
  
  <details>
  ```
  $ kubectl get pods -o wide
  ```
  </details>
    
- What would happen to hr-app if node01 is drained forcefully?
  
  <details>
  ```
  $ kubectl drain node01 --ignore-daemonsets --force
  hr-app will be lost forever
  ```
  </details>
    
- Run the command kubectl drain node02 --ignore-daemonsets --force

  <details>
  ```
  $ kubectl drain node01 --ignore-daemonsets --force
  ```
  </details>
  
- hr-app is a critical app and we do not want it to be removed and we do not want to schedule any more pods on node01.
  Mark node01 as unschedulable so that no new pods are scheduled on this node.


  Make sure that hr-app is not affected.
  
  <details>
  ```
  $ kubectl cordon node01
  ```
  </details>

