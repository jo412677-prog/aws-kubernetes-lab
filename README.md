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

Kubernetes 클러스터는 kubeadm 기반으로 구성했습니다.

- Container Runtime: containerd
- CNI: Calico

### Setup
1. [Common Node Setup](./docs/common-node-setup.md)
2. [Control Plane Setup](./docs/control-plane-setup.md)
3. [Worker Node Setup](./docs/worker-node-setup.md)
4. [Calico Network Setup](./docs/calico-network.md)


