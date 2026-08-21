### Control-plane

#### 1). 노드 초기화
```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

#### 2) kubeconfig 설정
```bash
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
