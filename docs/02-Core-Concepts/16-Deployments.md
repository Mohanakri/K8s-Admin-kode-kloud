# Deployments
  - Take me to [Video Tutorial](https://kodekloud.com/topic/deployments-3/)

Here's a summary of the article on Kubernetes Deployments:

- **Kubernetes Deployments** provide capabilities for deploying applications in a production environment.
- **Objective**:
  - Deploy multiple instances of an application for scalability.
  - Upgrade instances seamlessly with newer versions of application builds.
  - Perform rolling updates to avoid service interruptions.
  - Ability to undo changes and perform rollbacks.
  - Pause and resume changes for multiple modifications to the environment.
- **Key Features of Deployments**:
  - **Scalability**: Deploy multiple instances of an application.
  - **Seamless Upgrades**: Upgrade instances with rolling updates to avoid service downtime.
  - **Rollback Changes**: Ability to revert to previous versions in case of errors.
  - **Pause and Resume**: Make multiple changes to the environment and apply them together.
- **Components**:
  - **Pods**: Instances of the application, encapsulated in containers.
  - **Replication Controllers or ReplicaSets**: Manage multiple pods.
  - **Deployment**: Kubernetes object for higher-level management.
- **Creating a Deployment**:
  - Write a deployment definition file in YAML format.
  - Contents similar to ReplicaSet definition file but with `kind: Deployment`.
  - Includes `API version`, `metadata` (name and labels), and `spec` (template, replicas, and selector).
  - Run `kubectl create` command with the deployment definition file to create the deployment.
- **Viewing Deployments**:
  - Use `kubectl get deployment` command to see the newly created deployment.
  - Deployment automatically creates a ReplicaSet.
  - Use `kubectl get ReplicaSet` to view the created ReplicaSet.
  - ReplicaSets create pods, view with `kubectl get pods`.
- **Differences between ReplicaSets and Deployments**:
  - Deployment is a higher-level Kubernetes object for application management.
  - Provides features for seamless upgrades, rolling updates, rollbacks, and pausing/resuming changes.
  - Use `kubectl get all` to see all the created objects at once, including the deployment, ReplicaSets, and pods.

Kubernetes Deployments offer a powerful way to manage and scale applications, providing flexibility and control in production environments.





In this section, we will take a look at kubernetes deployments

#### Deployment is a kubernetes object. 
  
 ![deployment](../../images/deployment.PNG)
  
#### How do we create deployment?

```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: myapp-deployment
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end
 ```
- Once the file is ready, create the deployment using deployment definition file
  ```
  $ kubectl create -f deployment-definition.yaml
  ```
- To see the created deployment
  ```
  $ kubectl get deployment
  ```
- The deployment automatically creates a **`ReplicaSet`**. To see the replicasets
  ```
  $ kubectl get replicaset
  ```
- The replicasets ultimately creates **`PODs`**. To see the PODs
  ```
  $ kubectl get pods
  ```
    
  ![deployment1](../../images/deployment1.PNG)
  
- To see the all objects at once
  ```
  $ kubectl get all
  ```
  ![deployment2](../../images/deployment2.PNG)


________________________________________________________________________________

A quick note on editing Pods and Deployments
Edit a POD
Remember, you CANNOT edit specifications of an existing POD other than the below.

spec.containers[*].image

spec.initContainers[*].image

spec.activeDeadlineSeconds

spec.tolerations

For example you cannot edit the environment variables, service accounts, resource limits (all of which we will discuss later) of a running pod. But if you really want to, you have 2 options:

1. Run the kubectl edit pod <pod name> command.  This will open the pod specification in an editor (vi editor). Then edit the required properties. When you try to save it, you will be denied. This is because you are attempting to edit a field on the pod that is not editable.



A copy of the file with your changes is saved in a temporary location as shown above.

You can then delete the existing pod by running the command:

kubectl delete pod webapp



Then create a new pod with your changes using the temporary file

kubectl create -f /tmp/kubectl-edit-ccvrq.yaml



2. The second option is to extract the pod definition in YAML format to a file using the command

kubectl get pod webapp -o yaml > my-new-pod.yaml

Then make the changes to the exported file using an editor (vi editor). Save the changes

vi my-new-pod.yaml

Then delete the existing pod

kubectl delete pod webapp

Then create a new pod with the edited file

kubectl create -f my-new-pod.yaml



Edit Deployments
With Deployments you can easily edit any field/property of the POD template. Since the pod template is a child of the deployment specification,  with every change the deployment will automatically delete and create a new pod with the new changes. So if you are asked to edit a property of a POD part of a deployment you may do that simply by running the command

kubectl edit deployment my-deployment
___________________________________________________________________________________________________________________


  
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/
- https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
