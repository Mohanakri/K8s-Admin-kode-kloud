# Choosing a HA

  - Take me to [Lecture](https://kodekloud.com/topic/configure-high-availability/)


This lecture delves into high availability (HA) considerations within a Kubernetes cluster, particularly focusing on the implications of losing the master node and the strategies for ensuring continuous operation. Here are the key points summarized:

### High Availability Overview
- Losing the master node disrupts cluster management functions.
- Applications on worker nodes can continue running, but management tasks are affected.

### Importance of High Availability
- Multiple master nodes prevent single points of failure in production environments.
- HA ensures redundancy across all components to maintain continuous operation.

### Master Node Components
- Master node hosts control plane components: API server, controller manager, scheduler, and etcd server.
- HA setup replicates these components across multiple master nodes.

### Load Balancing API Servers
- Load balancer distributes traffic to API servers on multiple master nodes.
- Kubernetes control utility points to the load balancer for API access.

### Controller Manager and Scheduler
- These components must not run in parallel to avoid duplicating actions.
- Active-standby mode ensures only one instance performs actions while the other is passive.
- Leader election process determines the active instance.

### Etcd Configuration
- Two topologies: stacked control plane nodes and external etcd servers.
- Stacked topology: etcd runs on master nodes, easier setup but single point of failure.
- External etcd servers: etcd runs on separate servers, less risky but complex setup.

### API Server Configuration for Etcd
- API server configuration specifies etcd server addresses.
- Any API server instance can interact with any etcd server instance.

### Design Changes for HA
- Originally planned single master node, now configuring multiple masters.
- Introducing a load balancer for API servers.
- Total cluster nodes increased to five.

This lecture emphasizes the criticality of HA in Kubernetes clusters, detailing the redundancy strategies for master node components and etcd configuration to ensure continuous operation even in the event of failures.
