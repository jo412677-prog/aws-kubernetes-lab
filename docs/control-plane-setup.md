### Control-plane
공통 설정:
[Common Node Setup](./common-node-setup.md)

#### 1) 노드 초기화
```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

#### 2) kubeconfig 설정
```bash
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### 3) 클러스터 상태 확인
<img width="477" height="53" alt="image" src="https://github.com/user-attachments/assets/bb418212-9461-477d-aaf7-070ee1cf586e" />

#### 4) kubectl 자동완성 설정
```bash
echo 'source <(kubectl completion bash)' >> ~/.bashrc
echo 'alias k=kubectl' >> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >> ~/.bashrc
source ~/.bashrc
```
