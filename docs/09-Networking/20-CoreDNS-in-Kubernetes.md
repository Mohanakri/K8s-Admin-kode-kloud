# CoreDNS in Kubernetes

  - Take me to [Lecture](https://kodekloud.com/topic/coredns-in-kubernetes/)
### Summary: Implementing DNS in Kubernetes

**Objective**: 
Understanding how Kubernetes implements DNS to facilitate communication between services and pods within a cluster.

**Key Points**:

1. **DNS Basics**:
   - To resolve pod addresses, an entry could be added to each pod's `/etc/hosts` file. However, this method is impractical for large, dynamic clusters.
   - A central DNS server can be used, with pods pointing to this server via entries in their `/etc/resolv.conf` file.

2. **Kubernetes DNS Implementation**:
   - Kubernetes deploys a DNS server within the cluster to manage DNS resolution.
   - Prior to version 1.12, Kubernetes used `kube-dns`. The recommended DNS server from version 1.12 onwards is CoreDNS.

3. **CoreDNS Setup**:
   - CoreDNS is deployed as pods within the `kube-system` namespace, typically in a redundant setup.
   - These pods run the CoreDNS executable and use a configuration file (`Corefile`) located at `/etc/coredns`.
   - The `Corefile` includes multiple plugins for handling errors, health monitoring, metrics, caching, and Kubernetes-specific functionalities.

4. **Kubernetes Plugin in CoreDNS**:
   - The Kubernetes plugin in the `Corefile` sets the top-level domain for the cluster (e.g., `cluster.local`).
   - It watches for new pods and services, adding records for these in its database.
   - The `pods` option in the plugin can enable DNS records for individual pods, formatted by replacing dots in IP addresses with dashes (disabled by default).

5. **DNS Server Service**:
   - CoreDNS is exposed to other cluster components via a service named `kube-dns`.
   - The IP address of this service is automatically set as the nameserver in pods' `/etc/resolv.conf`.

6. **Kubelet's Role**:
   - The `kubelet` component is responsible for setting DNS configurations on pods, including the IP of the DNS server and the domain.

7. **Resolving Names**:
   - Pods can resolve services using various forms of the service name (e.g., `web-service`, `web-service.default`, `web-service.default.svc`, `web-service.default.svc.cluster.local`).
   - The `resolv.conf` file has search entries that facilitate these lookups by adding default domain suffixes.

8. **Service Resolution**:
   - DNS lookups can return fully qualified domain names (FQDNs), even when shorter names are used in requests.
   - The search entries in `resolv.conf` allow flexibility in how services are addressed but do not apply to individual pods.

**Conclusion**:
This lecture covers the DNS setup in Kubernetes, focusing on how CoreDNS is configured and how it facilitates service discovery within a cluster. For hands-on experience, refer to the practice exercises related to DNS in Kubernetes.

---

This summary encapsulates the main points discussed in the lecture about DNS implementation in Kubernetes, including the transition to CoreDNS, its configuration, and how Kubernetes ensures that services and pods can be resolved within the cluster.


===============================================================================================================================






In this section, we will take a look at **CoreDNS in the Kubernetes**


## To view the Pod

```
$ kubectl get pods -n kube-system
NAME                                      READY   STATUS    RESTARTS   AGE
coredns-66bff467f8-2vghh                  1/1     Running   0          53m
coredns-66bff467f8-t5nzm                  1/1     Running   0          53m
```

## To view the Deployment

```
$ kubectl get deployment -n kube-system
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
coredns                   2/2     2            2           53m
```

## To view the configmap of CoreDNS

```
$ kubectl get configmap -n kube-system
NAME                                 DATA   AGE
coredns                              1      52m
```

## CoreDNS Configuration File

```
$ kubectl describe cm coredns -n kube-system

Corefile:
---
.:53 {
    errors
    health {       lameduck 5s
    }
    ready
    kubernetes cluster.local in-addr.arpa ip6.arpa {
       pods insecure
       fallthrough in-addr.arpa ip6.arpa
       ttl 30
    }
    prometheus :9153
    forward . /etc/resolv.conf
    cache 30
    loop
    reload
}
```

## To view the Service 

```
$ kubectl get service -n kube-system
NAME       TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
kube-dns   ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   62m
```

## To view Configuration into the kubelet 

```
$ cat /var/lib/kubelet/config.yaml | grep -A2  clusterDNS
clusterDNS:
- 10.96.0.10
clusterDomain: cluster.local

```

## To view the fully qualified domain name

- With the `host` command, we will get fully qualified domain name (FQDN).

```
$ host web-service
web-service.default.svc.cluster.local has address 10.106.112.101

$ host web-service.default
web-service.default.svc.cluster.local has address 10.106.112.101

$ host web-service.default.svc
web-service.default.svc.cluster.local has address 10.106.112.101

$ host web-service.default.svc.cluster.local
web-service.default.svc.cluster.local has address 10.106.112.101
```

## To view the `/etc/resolv.conf` file

```
$ kubectl run -it --rm --restart=Never test-pod --image=busybox -- cat /etc/resolv.conf
nameserver 10.96.0.10
search default.svc.cluster.local svc.cluster.local cluster.local
options ndots:5
pod "test-pod" deleted
```

## Resolve the Pod 

```
$ kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
test-pod   1/1     Running   0          11m     10.244.1.3   node01   <none>           <none>
nginx      1/1     Running   0          10m     10.244.1.4   node01   <none>           <none>

$ kubectl exec -it test-pod -- nslookup 10-244-1-4.default.pod.cluster.local
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local

Name:      10-244-1-4.default.pod.cluster.local
Address 1: 10.244.1.4 
```

## Resolve the Service

```
$ kubectl get service
NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
kubernetes    ClusterIP   10.96.0.1        <none>        443/TCP   85m
web-service   ClusterIP   10.106.112.101   <none>        80/TCP    9m

$ kubectl exec -it test-pod -- nslookup web-service.default.svc.cluster.local
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local

Name:      web-service.default.svc.cluster.local
Address 1: 10.106.112.101 web-service.default.svc.cluster.local

```


#### References Docs

- https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#services
- https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pods
