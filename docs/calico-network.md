## Calico Network Setup
pod networking의 사용할 Calico CNI 적용

- Pod CIDR: `192.168.0.0/16`
- Routing: BGP
- Encapsulation: `IPIPCrossSubnet`

#### 1) Calico Operator 설치
참조 글) https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart

```bash
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.32.1/manifests/v1_crd_projectcalico_org.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.32.1/manifests/tigera-operator.yaml
```

#### 2) Calico custom-resource 다운로드
```bash
curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.32.1/manifests/custom-resources.yaml

vi custom-resources.yaml

cidr: 192.168.0.0/16
encapsulation: IPIPCrossSubnet # VXLANCrossSubnet -> IPIPCrossSubnet
```

#### 3) 상태 확인
<img width="959" height="467" alt="image" src="https://github.com/user-attachments/assets/2f045140-6b9a-4b8b-8d82-6d27fe54113c" />


#### 4) Pod Network Test
서로 다른 Worker Node에 Pod를 배치한 뒤 Cross-node Pod 통신을 확인

<img width="854" height="387" alt="image" src="https://github.com/user-attachments/assets/ee657fe1-d148-4a03-9f1f-ada65542793d" />

