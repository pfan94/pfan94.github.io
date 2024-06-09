# 1. disable swap
1. `sudo swapoff -a`
2. edit `/etc/fstab` and disable `swap` line

# 2. install docker
`sudo apt install docer.io -y`

# 3. install k8s
copied from https://v1-29.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
```shell
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo systemctl enable --now kubelet
```


# 4. setup in master node
Initialize k8s: `sudo kubectl init`

After this you will see some thing like this:
```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.16.202.129:6443 --token 2uf6g0.ioz83zlt1nzmq2mt \
	--discovery-token-ca-cert-hash sha256:b5ca24bd288c99375e556509e3311ca4bd310bf780f685dd11fff0197ea7fb47
```
Follow the instruction:
```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

# 5. setup slave nodes
jusr copy the `kubeadm` line and run: 
```shell
kubeadm join 172.16.202.129:6443 --token 2uf6g0.ioz83zlt1nzmq2mt \
	--discovery-token-ca-cert-hash sha256:b5ca24bd288c99375e556509e3311ca4bd310bf780f685dd11fff0197ea7fb47
```
