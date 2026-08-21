# Worker Node Setup

Worker Node는 공통 노드 설정 완료 후 Control Plane에 Join합니다.

공통 설정:
[Common Node Setup](./common-node-setup.md)

#### 1) Worker Node Join
```bash
kubeadm join <CONTROL-PLANE-IP>:6443 \
  --token <TOKEN> \
  --discovery-token-ca-cert-hash sha256:<HASH>
```

#### 2) Cluster join 확인
<img width="474" height="70" alt="image" src="https://github.com/user-attachments/assets/3cf0366f-3901-4b0b-b14e-102f308699df" />
