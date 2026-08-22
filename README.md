# AWS-Kubernetes-Lab
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

### 2. Instance
<img width="1011" height="210" alt="image" src="https://github.com/user-attachments/assets/3b0784b9-81aa-4a0d-8071-5433b6720131" />

### 3. Security Group
참고 글) https://kubernetes.io/ko/docs/reference/networking/ports-and-protocols/

  - #### control-plane
   <img width="1000" height="550" alt="image" src="https://github.com/user-attachments/assets/bee21d44-2150-4c84-ab1c-a87dfd976b2e" />
   
  - #### worker
  <img width="1260" height="677" alt="image" src="https://github.com/user-attachments/assets/c713f52b-2ed7-4df6-8886-58f6c37056a5" />

  - #### ALB
<img width="1240" height="227" alt="image" src="https://github.com/user-attachments/assets/a0523b20-126c-4a97-a6ca-0438307acafe" />
## Kubernetes Cluster Setup

Kubernetes 클러스터는 kubeadm 기반으로 구성했습니다.

- Container Runtime: containerd v2.3.4
- CNI: Calico v3.32.1

### Setup
1. [Common Node Setup](./docs/common-node-setup.md)
2. [Control Plane Setup](./docs/control-plane-setup.md)
3. [Worker Node Setup](./docs/worker-node-setup.md)
4. [Calico Network Setup](./docs/calico-network.md)
5. [Workload & Ingress](docs/workload-ingress.md)

