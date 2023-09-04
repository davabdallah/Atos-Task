# The below Steps to Install Kubeadm and create Kubernets cluster 

### 1. Create 4 VMs (one for Master and three for Worker nodes)

### 2. Install Kubeadm components on each VM


```console
sudo apt-get update
sudo apt-get install -y docker.io
```
   	- Install https support
```console		
   sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl
```
    b. Get kubernetes repo key:
		Ø curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    c. Add the Kubernetes package repository
		Ø echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    d. Install kubeadm 
		Ø sudo apt-get update
		Ø sudo apt-get install -y kubelet kubeadm kubectl
		Ø sudo apt-mark hold kubeadm kubelet kubectl
	
	4. Initialize the Kubernetes on (Master Node)
		Ø sudo kubeadm init --pod-network-cidr=10.244.0.0/16
			§ we will need to use the join output from this command like the below
			§ kubeadm join 10.0.0.2:6443 --token u02t28.uehhkn3ly8hyqros --discovery-token-ca-cert-hash sha256:65201b27d98e030fc628f6a13187a21124298f2306011f27bd15df9ddee1a5e5
			
	5. On the Master Node copy admin.conf file to User home directory
		Ø mkdir -p $HOME/.kube
		Ø sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
		Ø sudo chown $(id -u):$(id -g) $HOME/.kube/config
	
	6. Installing a Pod network add-on (Master Node)
		Ø kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
		OR
		Ø kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml

	7. Install helm (Master Node)
		Ø sudo apt update
		Ø sudo apt upgrade -y
		Ø wget https://get.helm.sh/helm-v3.12.3-linux-amd64.tar.gz
		Ø tar -zxvf helm-v3.12.3-linux-amd64.tar.gz
		Ø sudo mv linux-amd64/helm /usr/local/bin/helm
		
	9. Create namespaces as required
		Ø kubectl create namespace
	
	10. Install Ingress Controller
		Ø https://projectcontour.io/quickstart/contour.yaml
	
	11. Join Worker node the Cluster
	EX) Ø sudo kubeadm join 10.0.0.9:6443 --token qfx9jy.bl7gkyruwvi857z4 \
		        --discovery-token-ca-cert-hash sha256:45203ed0d7de1b49a6db2db9885932f6d031fd81f90236079f9361f2610f8d2e 

Troubleshooting
 # If found the core dns is always crashed , You need to manually delete the config in /etc/resolv.conf and create new one
   nameserver 8.8.8.8
