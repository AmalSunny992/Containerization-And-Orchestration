# Kubernetes Basics

## Setting up a Kubernetes Cluster

### Prerequisites
- Before setting up a Kubernetes cluster, ensure you have:
    - A set of machines (physical or virtual) with a supported operating system (e.g., Ubuntu, CentOS).
    - At least 2 GB of RAM and 2 CPUs per machine.
    - SSH access to all machines.
    - Docker installed on all machines.

### Using Minikube
For local development and learning, you can set up a single-node Kubernetes cluster using Minikube. Minikube runs a single-node Kubernetes cluster inside a VM on your local machine.

### Installing Minikube
**Install Minikube:**

On macOS:

```sh
brew install minikube
```

On Windows:

- Download the Minikube installer from the Minikube releases page and run it.

On Linux:
```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

- Start Minikube:

```sh
minikube start
```

- Verify Installation:

```sh
kubectl get nodes
```

### Using kubeadm
For a more production-like environment, you can set up a multi-node Kubernetes cluster using kubeadm.

- Installing kubeadm, kubelet, and kubectl
- Update Package Index:

```sh
sudo apt-get update
```

- Install Required Packages:

```sh
sudo apt-get install -y apt-transport-https ca-certificates curl
```
- Download Google Cloud Public Signing Key:

```sh
sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

- Add Kubernetes Repository:

```sh
sudo bash -c 'cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF'
```

- Install Packages:

```sh
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

#### Initializing the Master Node

1. Initialize the Master Node:

```sh
sudo kubeadm init
```

2.  Set up Local kubeconfig:

```sh
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

3. Install a Pod Network Add-on:

```sh
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

#### Joining Worker Nodes

1.  Get the Join Command from the Master Node:

```sh
kubeadm token create --print-join-command
```

2.  Run the Join Command on Each Worker Node:

```sh
sudo kubeadm join <master-ip>:<master-port> --token <token> --discovery-token-ca-cert-hash <hash>
```

3.  Verify the Cluster:

```sh
kubectl get nodes
```

## Deploying Applications on Kubernetes

### Creating a Deployment

1. Create a Deployment YAML File:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

2. Apply the Deployment:

```sh
kubectl apply -f nginx-deployment.yaml
```

3. Verify the Deployment:

```sh
kubectl get deployments
kubectl get pods
```

### Exposing the Deployment
1. Expose the Deployment:

```sh
kubectl expose deployment nginx-deployment --type=NodePort --port=80
```

2. Get the Service Details:

```sh
kubectl get services
```

3. Access the Application:

- Get the NodePort from the service details.
- Access the application using http://<node-ip>:<node-port>.

### Scaling the Deployment
1. Scale the Deployment:

```sh
kubectl scale deployment/nginx-deployment --replicas=5
```

2. Verify the Scaling:

```sh
kubectl get deployments
kubectl get pods
```

## Summary
Kubernetes simplifies the deployment, scaling, and management of containerized applications. Setting up a Kubernetes cluster can be done locally using Minikube or for a more production-like environment using kubeadm. Deploying applications on Kubernetes involves creating and managing resources such as deployments and services, which facilitate running and exposing applications.