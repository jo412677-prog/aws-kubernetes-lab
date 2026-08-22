## Workload & Ingress
Kubernetes 클러스터에 웹 애플리케이션을 배포하고, Service와 Ingress Controller를 구성하여 AWS ALB를 통해 외부에서 접근할 수 있도록 구성

#### 1) Deployment
```bash
 k create deployment nginx --image nginx --replicas 2
```
생성 확인
<img width="959" height="95" alt="image" src="https://github.com/user-attachments/assets/41166dad-ffbb-4f43-8e4f-ecb07e157349" />

#### 2) Service
```bash
k expose deployment nginx --name nginx-svc --type ClusterIP --port 80 --target-port 80
```
생성 확인

<img width="646" height="66" alt="image" src="https://github.com/user-attachments/assets/35d4580c-a35d-4996-9f8b-dcabc344c7c0" />

#### 3) Ingress Controller
참조 글) https://docs.nginx.com/nginx-ingress-controller/install/manifests/#option-1-create-a-nodeport-service

NGINX Ingress Controller install
```bash
git clone https://github.com/nginx/kubernetes-ingress.git --branch v5.5.4
cd kubernetes-ingress
```
<img width="924" height="98" alt="image" src="https://github.com/user-attachments/assets/a9457ab5-e796-4b12-9dce-6d563346d5ee" />

```bash
# RBAC
kubectl apply -f deployments/common/ns-and-sa.yaml
kubectl apply -f deployments/rbac/rbac.yaml


# ConfigMap, IngressClass
kubectl apply -f deployments/common/nginx-config.yaml
kubectl apply -f deployments/common/ingress-class.yaml

# CRDS
kubectl apply -f https://raw.githubusercontent.com/nginx/kubernetes-ingress/v5.5.4/deploy/crds.yaml

```

#### 4) NGINX Ingress Controller 배포
deployment 방식으로 배포
```bash
kubectl apply -f deployments/deployment/nginx-ingress.yaml
```
배포 확인

<img width="959" height="65" alt="image" src="https://github.com/user-attachments/assets/cc49d215-c48c-4267-900e-aec6f97c6e75" />

#### 5) NodePort 서비스 생성
외부 Load Balancer에서 NGINX Ingress Controller로 트래픽을 전달할 수 있도록 NodePort Service를 생성
```bash
kubectl apply -f deployments/service/nodeport.yaml
```

생성 확인

<img width="735" height="52" alt="image" src="https://github.com/user-attachments/assets/9506dff5-ad66-4394-af83-af8dad10c176" />

#### 6) Ingress 생성
ingerss.yaml
```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: nginx.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-svc
            port:
              number: 80
```
```bash
k create -f ingress.yaml
```
<img width="955" height="274" alt="image" src="https://github.com/user-attachments/assets/2c1bb92c-0cad-4441-9fbc-c2ed47f8ec89" />


#### 7) Worker Security Group
ALB -> NGINX Ingress Controller NodePort(31050)
- Protocol: TCP
- Port: 31050
- Source: ALB Security Group

#### 8) External Access Test
```bash
curl -H "Host: nginx.example.com" http://<ALB-DNS>

```
<img width="1050" height="467" alt="image" src="https://github.com/user-attachments/assets/9edae8f1-3903-4c2d-951f-7a34c0b197ac" />

