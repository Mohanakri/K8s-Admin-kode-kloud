# Ingress

  - Take me to [Lecture](https://kodekloud.com/topic/ingress/)
In this lecture, the instructor covers the concept of ingress in Kubernetes, explaining its purpose, differences from services, and its configuration. Here are the key points:

### Introduction to Services and Ingress
- **Services vs. Ingress**: Services expose applications within a Kubernetes cluster, while ingress manages external access.
- **Scenario**: An online store with applications (e.g., a web store and a video service).

### Deploying Applications
1. **Application Deployment**: The web store is deployed as a pod and exposed via a service.
2. **Database Setup**: MySQL database as a pod, exposed via a ClusterIP service.
3. **External Access**: The web application is exposed using a NodePort service, accessible via a high port number.

### Challenges with NodePort and Load Balancer
- Users must use IP and port numbers to access services, which is not user-friendly.
- High costs and complexity arise with multiple load balancers for different services.

### Introducing Ingress
- **Purpose**: Provides a single entry point to manage traffic routing, SSL, and URL-based routing.
- **Configuration**: Managed within Kubernetes using native definition files.

### How Ingress Works
1. **Ingress Controller**: A special build of solutions like NGINX, HAProxy, or Traefik deployed as a pod.
2. **Deployment**: Requires a deployment definition, a service to expose it, a config map for settings, and a service account with permissions.
3. **Ingress Resources**: Define rules for traffic routing based on URL paths or domain names.

### Creating Ingress Resources
1. **Basic Ingress Resource**: Routes all traffic to a single backend service.
2. **URL-based Routing**: Routes traffic to different services based on URL paths.
3. **Domain-based Routing**: Routes traffic based on different hostnames.

### Practical Configuration
- **Ingress Definition File**: Specifies rules, backend services, and optionally, default backends for handling unmatched paths.
- **Examples**: Configurations for splitting traffic by URL paths and hostnames, demonstrating how to handle multiple services within a cluster.

### Summary
- **Ingress Controllers**: Deploy and configure ingress controllers like NGINX.
- **Ingress Resources**: Use Kubernetes definition files to create rules for traffic routing.
- **Lab Practice**: Exercises include exploring pre-deployed environments and deploying ingress controllers from scratch.

This lecture emphasizes the benefits of using ingress for efficient traffic management and simplified configurations within Kubernetes.


=====================================================================================================




In this section, we will take a look at **Ingress**

- Ingress Controller
- Ingress Resources

## Ingress Controller

- Deployment of **Ingress Controller**

## ConfigMap

```
kind: ConfigMap
apiVersion: v1
metadata:
  name: nginx-configuration
```

## Deployment

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      serviceAccountName: ingress-serviceaccount
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
          args:
            - /nginx-ingress-controller
            - --configmap=$(POD_NAMESPACE)/nginx-configuration
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: http
              containerPort: 80
            - name: https
              containerPort: 443
```

## ServiceAccount

- ServiceAccount require for authentication purposes along with correct Roles, ClusterRoles and RoleBindings.

- Create a ingress service account
```
$ kubectl create -f ingress-sa.yaml
serviceaccount/ingress-serviceaccount created
```

## Service Type - NodePort

```
# service-Nodeport.yaml

apiVersion: v1
kind: Service
metadata:
  name: ingress
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https
  selector:
    name: nginx-ingress
```

- Create a service
```
$ kubectl create -f service-Nodeport.yaml
```
- To get the service

```
$ kubectl get service
```

## Ingress Resources

```
Ingress-wear.yaml

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear
spec:
     backend:
        serviceName: wear-service
        servicePort: 80
```

- To create the ingress resource
```
$ kubectl create -f Ingress-wear.yaml
ingress.extensions/ingress-wear created
```

- To get the ingress
```
$ kubectl get ingress
NAME           CLASS    HOSTS   ADDRESS   PORTS   AGE
ingress-wear   <none>   *                 80      18s
```

## Ingress Resource - Rules

- 1 Rule and 2 Paths.

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - http:
      paths:
      - path: /wear
        backend:
          serviceName: wear-service
          servicePort: 80
      - path: /watch
        backend:
          serviceName: watch-service
          servicePort: 80
```
- Describe the earlier created ingress resource

```
$ kubectl describe ingress ingress-wear-watch
Name:             ingress-wear-watch
Namespace:        default
Address:
Default backend:  default-http-backend:80 (<none>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /wear    wear-service:80 (<none>)
              /watch   watch-service:80 (<none>)
Annotations:  <none>
Events:
  Type    Reason  Age   From                      Message
  ----    ------  ----  ----                      -------
  Normal  CREATE  23s   nginx-ingress-controller  Ingress default/ingress-wear-watch

```

- 2 Rules and 1 Path each.
```
# Ingress-wear-watch.yaml

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - host: wear.my-online-store.com
    http:
      paths:
      - backend:
          serviceName: wear-service
          servicePort: 80
  - host: watch.my-online-store.com
    http:
      paths:
      - backend:
          serviceName: watch-service
          servicePort: 80
```






#### References Docs

- https://kubernetes.io/docs/concepts/services-networking/ingress/
- https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/
- https://thenewstack.io/kubernetes-ingress-for-beginners/
