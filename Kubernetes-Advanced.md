# Advanced Kubernetes Concepts

## Kubernetes Services
A Kubernetes Service is an abstraction that defines a logical set of Pods and a policy by which to access them. Services enable loose coupling between dependent Pods.

## Types of Services

### ClusterIP
Exposes the Service on a cluster-internal IP. This type makes the Service only reachable from within the cluster.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

### NodePort
Exposes the Service on each Node’s IP at a static port (NodePort). A ClusterIP Service is automatically created, and the NodePort Service routes to it.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30007
  type: NodePort
```

### LoadBalancer
Exposes the Service externally using a cloud provider’s load balancer.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

### ExternalName
Maps a Service to a DNS name, not to a typical selector.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ExternalName
  externalName: my.database.example.com
```
## Kubernetes Ingress
An Ingress is an API object that manages external access to services in a cluster, typically HTTP. Ingress can provide load balancing, SSL termination, and name-based virtual hosting.

### Creating an Ingress Resource
Install an Ingress Controller depending on your environment (e.g., NGINX, Traefik, HAProxy).

Create an Ingress YAML File:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```

Apply the Ingress Resource:
```sh
kubectl apply -f my-ingress.yaml
```
## ConfigMaps and Secrets

### ConfigMaps
ConfigMaps allow you to decouple configuration artifacts from image content to keep containerized applications portable.

**Creating a ConfigMap**

From Literal Values:

```sh
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
```
From a File:

```sh
kubectl create configmap my-config --from-file=config.txt
```

Using a YAML File:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key1: value1
  key2: value2
```
### Secrets
Secrets are similar to ConfigMaps but are intended to hold sensitive information such as passwords, OAuth tokens, and SSH keys.

**Creating a Secret**

From Literal Values:

```sh
kubectl create secret generic my-secret --from-literal=username=myuser --from-literal=password=mypassword
```

Using a YAML File:

```yaml
Copy code
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: bXl1c2Vy
  password: bXlwYXNzd29yZA==
```

## Namespaces
Namespaces provide a mechanism to isolate groups of resources within a single Kubernetes cluster. They are useful in environments with many users spread across multiple teams or projects.

- Creating a Namespace
```sh
kubectl create namespace my-namespace
```

- Using a Namespace
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: my-namespace
spec:
  containers:
    - name: my-container
      image: my-image
```

## Deployments
Deployments provide declarative updates for Pods and ReplicaSets.

- Creating a Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image
          ports:
            - containerPort: 80
```

## ReplicaSets
ReplicaSets ensure a specified number of pod replicas are running at any given time.

- Creating a ReplicaSet
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image
```

## DaemonSets
DaemonSets ensure that all (or some) Nodes run a copy of a Pod.

- Creating a DaemonSet

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      name: my-daemonset
  template:
    metadata:
      labels:
        name: my-daemonset
    spec:
      containers:
        - name: my-container
          image: my-image
```

## PersistentVolumes
PersistentVolumes (PV) are a piece of storage in the cluster that has been provisioned by an administrator.

- Creating a PersistentVolume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

## StatefulSets
StatefulSets are used for applications that require stable, unique network identifiers and stable persistent storage.

- Creating a StatefulSet

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-statefulset
spec:
  serviceName: "my-service"
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-container
          image: my-image
          volumeMounts:
            - name: my-volume
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: my-volume
      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
```

## Roles and RoleBindings
Roles and RoleBindings are used for granting permissions within a namespace.

- Creating a Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: my-namespace
  name: my-role
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch"]
```

- Creating a RoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: my-rolebinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: my-role
subjects:
  - kind: User
    name: my-user
    namespace: my-namespace
```

## Summary

In Kubernetes, advanced concepts like Services, Ingress, ConfigMaps, Secrets, Namespaces, Deployments, ReplicaSets, DaemonSets, PersistentVolumes, StatefulSets, Roles, and RoleBindings are essential for deploying and managing containerized applications efficiently. Services provide networking abstraction for Pods, while Ingress allows external access and routing. ConfigMaps and Secrets manage configuration and sensitive data, respectively. Namespaces enable logical isolation of resources, and Deployments, ReplicaSets, and DaemonSets ensure scalable and fault-tolerant deployments. PersistentVolumes and StatefulSets handle storage needs, and Roles with RoleBindings manage access control within namespaces. These concepts collectively empower Kubernetes users to build resilient, scalable, and secure container-based architectures.