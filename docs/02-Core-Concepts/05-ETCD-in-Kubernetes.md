# ETCD in Kubernetes
  - Take me to [Video Tutorial](https://kodekloud.com/topic/etcd-in-kubernetes/)

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
  
AsChat GPT
To install etcd as a service on a Linux node, you typically follow these steps. Since you haven't mentioned the specific Linux distribution, I'll provide a general guide that should work for most distributions, especially those using systemd for managing services. If your system uses a different init system (like SysVinit), the steps might vary slightly.

### 1. Download etcd
First, you'll need to download the etcd binary. You can find the latest release on the [etcd GitHub releases page](https://github.com/etcd-io/etcd/releases).

For example, to download version 3.5.0:

```bash
wget https://github.com/etcd-io/etcd/releases/download/v3.5.0/etcd-v3.5.0-linux-amd64.tar.gz
tar xvf etcd-v3.5.0-linux-amd64.tar.gz
cd etcd-v3.5.0-linux-amd64
```

### 2. Move the etcd binaries
Move the etcd binaries to a location like `/usr/local/bin/` so they are in the system's path:

```bash
sudo mv etcd etcdctl /usr/local/bin/
```

### 3. Create etcd User (Optional)
It's a good practice to create a dedicated user for running etcd. You can skip this step if you prefer to run etcd as a different user.

```bash
sudo useradd -r -s /bin/false etcd
```

### 4. Create etcd Configuration File
Create a configuration file for etcd. This is an example `/etc/etcd/etcd.conf.yml`:

```yaml
name: node1
data-dir: /var/lib/etcd
initial-cluster-state: new
initial-cluster-token: my-etcd-cluster
initial-cluster: node1=http://<node1_ip>:2380,node2=http://<node2_ip>:2380,node3=http://<node3_ip>:2380
initial-advertise-peer-urls: http://<node1_ip>:2380
advertise-client-urls: http://<node1_ip>:2379
listen-peer-urls: http://<node1_ip>:2380
listen-client-urls: http://<node1_ip>:2379
```

Replace `<node1_ip>`, `<node2_ip>`, `<node3_ip>` with the respective IP addresses of your nodes if you're setting up a cluster. If you're setting up a single-node etcd, you can simplify this.

### 5. Create Systemd Service File
Create a systemd service file for etcd at `/etc/systemd/system/etcd.service`:

```ini
[Unit]
Description=etcd
Documentation=https://github.com/etcd-io/etcd
After=network.target

[Service]
User=etcd
Type=notify
ExecStart=/usr/local/bin/etcd --config-file /etc/etcd/etcd.conf.yml
Restart=always
RestartSec=10s
LimitNOFILE=40000

[Install]
WantedBy=multi-user.target
```

### 6. Reload systemd and Start etcd Service
Reload systemd to read the new service file and start the etcd service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable etcd
sudo systemctl start etcd
```

### 7. Verify etcd Status
Check the status to ensure that etcd is running properly:

```bash
sudo systemctl status etcd
```

That's it! Etcd should now be installed as a service on your node, running in the background.


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

K8s Reference Docs:
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/setup-ha-etcd-with-kubeadm/
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/#stacked-control-plane-and-etcd-nodes
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/#external-etcd-nodes
