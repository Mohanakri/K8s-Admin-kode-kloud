# ETCD in HA

 - Take me to [Lecture](https://kodekloud.com/topic/etcd-in-ha/)

This lecture focuses on configuring etcd in a high availability (HA) setup, which serves as a prerequisite for configuring Kubernetes in HA mode. Here's a summary of the key points covered:

### Etcd Overview
- Etcd is a distributed, reliable key-value store that is simple, secure, and fast.
- It stores data across multiple servers, ensuring redundancy and consistency.

### Distributed Consensus with Raft Protocol
- Etcd uses the Raft protocol for distributed consensus.
- Raft facilitates leader election among nodes and ensures consistency across the cluster.

### Quorum Calculation
- Quorum is the minimum number of nodes required for the cluster to function properly or make successful writes.
- Formula: Total nodes / 2 + 1.
- Odd numbers are preferred for better fault tolerance.

### Fault Tolerance and Network Segmentation
- Odd numbers of nodes provide better fault tolerance, minimizing the risk of cluster failure during network segmentation.
- Even numbers of nodes may result in quorum failure during network partitions.

### Installation and Configuration of Etcd
- Download the latest etcd binary, extract it, and configure the etcd service.
- Use etcdctl utility to store and retrieve data.
- Prefer using etcd API version 3 for compatibility.

### Design Considerations
- Minimum required nodes for fault tolerance in HA setup is three.
- Odd numbers of nodes are preferred for better fault tolerance.
- Choose the number of nodes based on fault tolerance requirements and resource capacity.
- In the provided scenario, three nodes are chosen for HA setup, but only two nodes are deployed due to hardware limitations.

### Summary of Design
- The design includes a minimum of three nodes for fault tolerance.
- Due to hardware limitations, only two master nodes are deployed.
- Etcd servers are configured on the master nodes (stacked topology).

### Conclusion
- Design considerations include fault tolerance, network segmentation, and resource capacity.
- The chosen design meets fault tolerance requirements within hardware limitations.

Overall, this lecture provides insights into configuring etcd for HA in Kubernetes clusters and offers practical guidance for design considerations and implementation.
