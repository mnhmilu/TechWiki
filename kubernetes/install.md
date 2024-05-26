# [install in Ubuntu 22.04](https://medium.com/@kvihanga/how-to-set-up-a-kubernetes-cluster-on-ubuntu-22-04-lts-433548d9a7d0) 

## MasterNode  Installation

```

Step 1 : Update and Upgrade Ubuntu
sudo apt-get update
sudo apt-get upgrade

Step 2 : Disable Swap

sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

Step 3 : Add Kernel Parameters

sudo tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF
sudo modprobe overlay
sudo modprobe br_netfilter
sudo tee /etc/sysctl.d/kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

Reload the change

sudo sysctl --system


Step 4 : Install Containerd Runtime

sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update
sudo apt install -y containerd.io

containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

sudo systemctl restart containerd
sudo systemctl enable containerd


Step 5 : Install Kubernetes Components:

Add the Kubernetes signing key and repository.

sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

Step 6 : Update the package list and install kubelet, kubeadm, and kubectl.

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

Step 7 : Initialize Kubernetes Master Node: Initialize the Kubernetes cluster using kubeadm.

sudo kubeadm init

To start using your cluster, set up the kubeconfig.

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


Step 8 : Deploy a Pod Network: Install a pod network so that your nodes can communicate with each other.

kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml

```

## Worker Node Installation

Proceed above step 1– 6 on the worker nodes
Join the Worker Nodes:
On each worker node, use the join command you got from the master node’s init output.

```

kubeadm join 10.10.10.1:6443 --token qyq7z7.5augt56vin856fj \
 --discovery-token-ca-cert-hash sha256:09a1fc88c9fc8cab5171333546456455dd54cf3d8cb3400362586185308c3

```

Verifying the Cluster Setup
Ensure your cluster is up and running.

`kubectl get nodes`

Output:
```
NAME               STATUS   ROLES           AGE     VERSION
fbb-k8s-master     Ready    control-plane   10m     v1.29.0
fbb-k8s-worker-1   Ready    <none>          2m31s   v1.29.0
```

## [Deploy a nging server in new cluster](https://www.geeksforgeeks.org/how-to-deploy-nginx-in-kubernetes/) 

Deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  replicas: 1
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
        image: nginx:latest
        ports:
        - containerPort: 80

```

Service.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80


```
Apply , it will install in worker node to verify execute `describe pod nginx-pod`

```
kubectl apply -f Deployment.yaml
kubectl apply -f Service.yaml
```

To expose port from cluster 


`sudo ufw allow 8083`

`kubectl port-forward nginx-pod 8083:80`


