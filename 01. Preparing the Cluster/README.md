# The below Steps to Install Kubeadm and create Kubernets cluster 

### 1. Create 4 VMs (one for Master and three for Worker nodes)

### 2. Install Kubeadm components on each VM

- Install Docker
```console
sudo apt-get update
```
```console
sudo apt-get install -y docker.io
```
- Install https support
```console		
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl
```
- Get kubernetes repo key:
```console	
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
- Add the Kubernetes package repository
```console
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
- Install kubeadm
```console
sudo apt-get update
```
```console
sudo apt-get install -y kubelet kubeadm kubectl
```
```console
sudo apt-mark hold kubeadm kubelet kubectl
```
	
### 3.Initialize the Kubernetes on (Master Node)
```console
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
- You will need to use the join output from Kubeadm init command
- Copy admin.conf file to User home directory
```console
mkdir -p $HOME/.kube
```
```console
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```
```console
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
- Install a Pod network add-on (Master Node)
```console
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
OR
kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml
```
### 4. Join Worker nodes to the Cluster (ex)
```console
sudo kubeadm join 10.0.0.9:6443 --token qfx9jy.bl7gkyruwvi857z4 \
		        --discovery-token-ca-cert-hash sha256:45203ed0d7de1b49a6db2db9885932f6d031fd81f90236079f9361f2610f8d2e 
```
### 5. Install helm on (Master Node)
```console
sudo apt update
```
```console
sudo apt upgrade -y
```
```console
wget https://get.helm.sh/helm-v3.12.3-linux-amd64.tar.gz
```
```console
tar -zxvf helm-v3.12.3-linux-amd64.tar.gz
```
```console
sudo mv linux-amd64/helm /usr/local/bin/helm
```
### 6. Install Ingress Controller
```console
kubectl apply -f https://projectcontour.io/quickstart/contour.yaml
```

### 7. Create namespaces as required
- you can find [namespaces.yaml](https://github.com/davabdallah/Atos-Task/tree/main/01.%20Preparing%20the%20Cluster)
```console
kubectl create -f namespaces.yaml
```	

### Troubleshooting
- If found the core dns is always crashed , You need to manually delete the config in /etc/resolv.conf and create new one
   nameserver 8.8.8.8
