# RBAC
  - Take me to [Video Tutorial](https://kodekloud.com/topic/role-based-access-controls/)
This lecture focuses on Role-Based Access Control (RBAC) in Kubernetes, providing detailed steps on how to create roles and link users to those roles. Here's a summary:

### Role Creation
1. **Create Role Definition File**: 
   - Define a role object with API version `rbac.authorization.k8s.io/v1` and kind set to `Role`.
   - Specify the rules for the role, including API groups, resources, and verbs (actions).
   - Example: Allow developers to access pods for actions like list, get, create, and delete.

2. **Create Role**: 
   - Use the `kubectl create role` command to create the role based on the definition file.

### Role Binding
1. **Create Role Binding Definition File**:
   - Define a role binding object with kind set to `RoleBinding`.
   - Specify the user details in the `subjects` section.
   - Reference the role created earlier in the `roleRef` section.

2. **Create Role Binding**: 
   - Use the `kubectl create rolebinding` command to create the role binding based on the definition file.

### Namespace Scope
- Roles and role bindings are scoped to namespaces.
- Specify the namespace within the metadata of the definition files to limit access to resources within that namespace.

### Viewing Roles and Role Bindings
- Use `kubectl get roles` to view created roles.
- Use `kubectl get rolebindings` to list role bindings.
- Use `kubectl describe role <role_name>` to view details about a specific role.
- Use `kubectl describe rolebinding <rolebinding_name>` to view details about a specific role binding.

### Checking User Permissions
- Use `kubectl auth can-i` command to check if a user has permission to perform specific actions on resources.
- Administrators can impersonate other users to check their permissions using the `--as` option.
- Namespace can be specified to check permissions within a specific namespace.

### Resource Names
- Access to specific resources within a namespace can be controlled by adding a `resourceNames` field to the rule definition.

### Conclusion
- RBAC provides a structured approach to access control in Kubernetes, allowing fine-grained control over user permissions.
- Testing user permissions and resource access is facilitated using `kubectl auth can-i` command.
- Resource names can be used to further refine access control at the resource level.

The lecture concludes by encouraging learners to practice working with RBAC through practice exercises.


=======================================================================================



In this section, we will take a look at RBAC

## How do we create a role?
- Each role has 3 sections
  - apiGroups
  - resources
  - verbs
- create the role with kubectl command
  ```
  $ kubectl create -f developer-role.yaml
  ```

## The next step is to link the user to that role.
- For this we create another object called **`RoleBinding`**. This role binding object links a user object to a role.
- create the role binding using kubectl command
  ```
  $ kubectl create -f devuser-developer-binding.yaml
  ```
- Also note that the roles and role bindings fall under the scope of namespace.
  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: developer
  rules:
  - apiGroups: [""] # "" indicates the core API group
    resources: ["pods"]
    verbs: ["get", "list", "update", "delete", "create"]
  - apiGroups: [""]
    resources: ["ConfigMap"]
    verbs: ["create"]
  ```
  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: devuser-developer-binding
  subjects:
  - kind: User
    name: dev-user # "name" is case sensitive
    apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: Role
    name: developer
    apiGroup: rbac.authorization.k8s.io
  ```
  ![rbac1](../../images/rbac1.PNG)
  

## View RBAC
  
- To list roles
  ```
  $ kubectl get roles
  ```
- To list rolebindings
  ```
  $ kubectl get rolebindings
  ```
- To describe role 
  ```
  $ kubectl describe role developer
  ```
  
  ![rbac2](../../images/rbac2.PNG)
    
- To describe rolebinding
  ```
  $ kubectl describe rolebinding devuser-developer-binding
  ```
  
  ![rbac3](../../images/rbac3.PNG)
  
#### What if you being a user would like to see if you have access to a particular resource in the cluster.
## Check Access

- You can use the kubectl auth command
  ```
  $ kubectl auth can-i create deployments
  $ kubectl auth can-i delete nodes
  ```
  ```
  $ kubectl auth can-i create deployments --as dev-user
  $ kubectl auth can-i create pods --as dev-user
  ```
  ```
  $ kubectl auth can-i create pods --as dev-user --namespace test
  ```
  
  ![rbac5](../../images/rbac5.PNG)
  
## Resource Names
- Note on resource names we just saw how you can provide access to users for resources like pods within the namespace.
  ```
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: developer
  rules:
  - apiGroups: [""] # "" indicates the core API group
    resources: ["pods"]
    verbs: ["get", "update", "create"]
    resourceNames: ["blue", "orange"]
  ```  
  ![rbac4](../../images/rbac4.PNG)
  
#### K8s Reference Docs
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities
