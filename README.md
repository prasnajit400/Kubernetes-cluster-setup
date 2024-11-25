# Kubernetes-cluster-setup
#Apply coomand in both Control plane and nodes
#Update the system
sudo apt update -y 
#install docker
sudo apt install docker.io -y

sudo apt-get install -y apt-transport-https ca-certificates curl gpg
sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo systemctl enable --now kubelet

#Command from here to onwords should apply only control plane 
sudo kubeadm init
#After this command "sudo kubeadm init", You will get cluster joining link, you can execute this link any node to joining the cluster.

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
#Download cluster networking "calico"
curl https://raw.githubusercontent.com/projectcalico/calico/v3.28.2/manifests/calico.yaml -O
#Apply the calico yaml
kubectl apply -f calico.yaml
#Now check kubernet nodes
kubectl get nodes
