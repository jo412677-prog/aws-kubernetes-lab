# aws-kubernetes-lab
AWS 환경에서 kubeadm 기반 Kubernetes 클러스터 구축 과정을 정리한 실습 저장소

<img width="1073" height="678" alt="architecture png" src="https://github.com/user-attachments/assets/354f9d08-5432-47db-a7e4-57c10da42e5d" />


## AWS Architecture
### 1. Subnet
  ap-northeast-2a
  - Public-A  10.100.1.0/24
  - Private-A 10.100.10.0/24

  ap-northeast-2b
  - Public-B  10.100.2.0/24
  - Private-B 10.100.20.0/24

### 2. security group
참고 글) https://kubernetes.io/ko/docs/reference/networking/ports-and-protocols/

  - #### control-plane
   <img width="1000" height="550" alt="image" src="https://github.com/user-attachments/assets/bee21d44-2150-4c84-ab1c-a87dfd976b2e" />
   
  - #### worker
   <img width="1000" height="410" alt="image" src="https://github.com/user-attachments/assets/967be99f-0c4c-44d5-afc4-6ed31858618d" />

### 3. instance
  

## Kubernetes Cluster Setup
### 1. Control-Plane
#### 1) iptables 브리지 트래픽 활성화
```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system 
```
#### 2) swap off
```bash
sudo swapoff -a
```
#### 3) Container Runtime install
Kubernetes 노드에서 사용할 Container Runtime으로 containerd를 구성했습니다. # containerd v2.3.4
```bash
sudo apt-get update   
wget https://github.com/containerd/containerd/releases/download/v2.3.4/containerd-2.3.4-linux-amd64.tar.gz
sudo tar Cxzvf /usr/local containerd-2.3.4-linux-amd64.tar.gz
```   
#### 4) containerd systemd 서비스 등록
```bash
sudo mkdir -p /usr/local/lib/systemd/system
sudo curl -L https://raw.githubusercontent.com/containerd/containerd/main/containerd.service -o /usr/local/lib/systemd/system/containerd.service
```

#### 5) runc install
```bash
wget https://github.com/opencontainers/runc/releases/download/v1.5.1/runc.amd64
sudo install -m 755 runc.amd64 /usr/local/sbin/runc
```

#### 6) CNI plugins install
```bash
sudo mkdir -p /opt/cni/bin
wget https://github.com/containernetworking/plugins/releases/download/v1.9.0/cni-plugins-linux-amd64-v1.9.0.tgz
sudo tar Cxzvf /opt/cni/bin cni-plugins-linux-amd64-v1.9.0.tgz
```

#### 7) containerd를 CNI 런타임을 사용하기 위한 설정
```bash
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

sudo systemctl daemon-reload
sudo systemctl enable --now containerd
```

#### 8) 설치 확인
<img width="762" height="146" alt="image" src="https://github.com/user-attachments/assets/0f01a362-cfb8-41eb-a7c3-b4313722a212" />
<img width="965" height="367" alt="image" src="https://github.com/user-attachments/assets/3df0efe9-da26-4e1b-aaea-673125fe9cde" />

#### 9) kubernetes package install
```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

sudo mkdir -p -m 755 /etc/apt/keyrings 
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.34/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.34/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list # add 1.34 repository

sudo apt-get update
sudo apt-cache madison kubeadm
sudo apt-get install -y kubelet=1.34.2-1.1 kubeadm=1.34.2-1.1 kubectl=1.34.2-1.1
sudo apt-mark hold kubelet kubeadm kubectl

```
#### 10) 노드 초기화
```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```
