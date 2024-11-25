Setting Up Kubernetes Cluster with Docker and Calico Networking
Step 1: Apply the following commands on both the Control Plane and Nodes

1. Update the system
# sudo apt update -y

2. Install Docker
# sudo apt install docker.io -y

3. Set up Kubernetes repository and install required components

# sudo apt-get install -y apt-transport-https ca-certificates curl gpg
# sudo mkdir -p -m 755 /etc/apt/keyrings
# curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
# echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
# sudo apt-get update
# sudo apt-get install -y kubelet kubeadm kubectl
# sudo apt-mark hold kubelet kubeadm kubectl
# sudo systemctl enable --now kubelet


Step 2: Commands to apply only on the Control Plane

Initialize the Kubernetes Control Plane
# sudo kubeadm init

After running this command, you'll receive a cluster join command. Use it on the nodes to join the cluster.

Set up kubeconfig for the current user

# mkdir -p $HOME/.kube
# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
# sudo chown $(id -u):$(id -g) $HOME/.kube/config

Download and apply Calico networking

# curl https://raw.githubusercontent.com/projectcalico/calico/v3.28.2/manifests/calico.yaml -O
# kubectl apply -f calico.yaml

Verify the Kubernetes nodes

# kubectl get nodes
