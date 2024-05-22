# CNI weave
  
  - Take me to [Lecture](https://kodekloud.com/topic/cni-weave/)
In this lecture, the focus is on explaining how the Weaveworks Weave CNI plugin works within a Kubernetes cluster. Here's a summary:

1. **Introduction**: The instructor begins by highlighting the importance of understanding networking solutions based on CNI, such as Weave, within Kubernetes.

2. **Comparison to Manual Setup**: The lecture starts with a comparison to a manually set up networking solution, emphasizing the limitations of manual routing tables in larger environments with many nodes and pods.

3. **Analogy to Shipping**: An analogy to shipping logistics is provided to illustrate how Weave operates within a Kubernetes cluster. Weave agents act as shipping agents, managing communication and routing between different sites (nodes) and departments (pods).

4. **Weave Functionality**: Weave deploys an agent or service on each node, which communicates with other agents to exchange information about the cluster's topology. It creates its own bridge on the nodes, assigns IP addresses to each network, and ensures correct routing paths for packets.

5. **Packet Routing**: When a packet is sent from one pod to another on a different node, Weave intercepts the packet, encapsulates it with new source and destination information, sends it across the network, and then routes it to the correct destination pod.

6. **Deployment**: Weave can be deployed manually as services or daemons on each node, or more conveniently as pods within the Kubernetes cluster. A single `kubectl apply` command can deploy all necessary components, including Weave peers as a daemon set, ensuring deployment on all nodes.

7. **Troubleshooting**: For troubleshooting purposes, logs from Weave pods can be accessed using the `kubectl logs` command.

The lecture concludes by encouraging learners to explore Weave deployment in Kubernetes clusters and provides guidance on troubleshooting.



====================================================================



In this section, we will take a look at "CNI Weave in the Kubernetes Cluster"

## Deploy Weave

- Installing [weave net](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/) onto the Kubernetes cluster with a single command.

```
$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
serviceaccount/weave-net created
clusterrole.rbac.authorization.k8s.io/weave-net created
clusterrolebinding.rbac.authorization.k8s.io/weave-net created
role.rbac.authorization.k8s.io/weave-net created
rolebinding.rbac.authorization.k8s.io/weave-net created
daemonset.apps/weave-net created
```

## Weave Peers

```
$ kubectl get pods -n kube-system
NAME                                      READY   STATUS             RESTARTS   AGE
coredns-66bff467f8-894jf                  1/1     Running            0          52m
coredns-66bff467f8-nck5f                  1/1     Running            0          52m
etcd-controlplane                         1/1     Running            0          52m
kube-apiserver-controlplane               1/1     Running            0          52m
kube-controller-manager-controlplane      1/1     Running            0          52m
kube-keepalived-vip-mbr7d                 1/1     Running            0          52m
kube-proxy-p2mld                          1/1     Running            0          52m
kube-proxy-vjcwp                          1/1     Running            0          52m
kube-scheduler-controlplane               1/1     Running            0          52m
weave-net-jgr8x                           2/2     Running            0          45m
weave-net-tb9tz                           2/2     Running            0          45m
```

## View the logs of Weave Pod's

```
$ kubectl logs weave-net-tb9tz weave -n kube-system 
```

## View the default route in the Pod

```
$ kubectl run test --image=busybox --command -- sleep 4500
pod/test created

$ kubectl exec test -- ip route
default via 10.244.1.1 dev eth0
```


#### References Docs

- https://kubernetes.io/docs/concepts/cluster-administration/addons/
- https://www.weave.works/docs/net/latest/kubernetes/kube-addon/
