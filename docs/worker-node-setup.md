# Worker Node Setup

Worker Node는 공통 노드 설정 완료 후 Control Plane에 Join합니다.

공통 설정:
[Common Node Setup](./common-node-setup.md)

#### 1) Worker Node Join
```bash
kubeadm join 10.100.10.133:6443 --token 0f9c1z.608bk0adkjngdw19 \
        --discovery-token-ca-cert-hash sha256:a814de4706f7936c4d00f8099a96e97a494ed455e8bbf62f843e9e97b1576b34
```

#### 2) Cluster join 확인
<img width="474" height="70" alt="image" src="https://github.com/user-attachments/assets/3cf0366f-3901-4b0b-b14e-102f308699df" />
