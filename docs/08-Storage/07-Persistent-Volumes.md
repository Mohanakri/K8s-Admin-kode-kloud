# Persistent Volumes

  - Take me to [Lecture](https://kodekloud.com/topic/persistent-volumes-4/)
In this lecture, Mumshad introduces persistent volumes in Kubernetes as a solution for centrally managing storage across a cluster. Unlike volumes configured within pod definition files, persistent volumes allow administrators to create a cluster-wide pool of storage that users can access using persistent volume claims (PVCs).

Mumshad explains the process of creating a persistent volume by starting with a base template and updating the API version and kind to "persistent volume." He discusses setting access modes, capacity (amount of storage), and volume type. He demonstrates using the hostPath option for local storage on the node, cautioning against its use in production environments. Commands for creating and listing persistent volumes using `kubectl` are provided.

Lastly, Mumshad highlights the importance of replacing the hostPath option with supported storage solutions like AWS Elastic Block Store in production environments. He concludes by mentioning that the next lecture will cover how to use persistent volume claims to claim volumes configured with persistent volumes.
================================================================================================================================



In this section, we will take a look at **Persistent Volumes**

- In the large evnironment, with a lot of users deploying a lot of pods, the users would have to configure storage every time for each Pod.
- Whatever storage solution is used, the users who deploys the pods would have to configure that on all pod definition files in his environment. Every time a change is to be made, the user would have to make them on all of his pods.

![class-16](../../images/class16.PNG)


- A Persistent Volume is a cluster-wide pool of storage volumes configured by an administrator to be used by users deploying application on the cluster. The users can now select storage from this pool using Persistent Volume Claims.

  ```
  pv-definition.yaml
  
  kind: PersistentVolume
  apiVersion: v1
  metadata:
    name: pv-vol1
  spec:
    accessModes: [ "ReadWriteOnce" ]
    capacity:
     storage: 1Gi
    hostPath:
     path: /tmp/data
  ```

  ```
  $ kubectl create -f pv-definition.yaml
  persistentvolume/pv-vol1 created

  $ kubectl get pv
  NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
  pv-vol1   1Gi        RWO            Retain           Available                                   3min
  
  $ kubectl delete pv pv-vol1
  persistentvolume "pv-vol1" deleted
  ```

#### Kubernetes Persistent Volumes

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
- https://portworx.com/tutorial-kubernetes-persistent-volumes/

