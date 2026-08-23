## Gateway API

Kubernetes Gateway API의 Gateway와 HTTPRoute를 구성하여 nginx Service를 HTTP 80 포트로 외부에 노출

<img width="1317" height="779" alt="gate" src="https://github.com/user-attachments/assets/adfdc921-1aa0-44a2-8824-365eb4d62fd4" />

#### 1) Gateway API CRD 설치
참조 글) https://docs.nginx.com/nginx-gateway-fabric/get-started/#add-gateway-api-resources
```bash
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v2.6.7" | kubectl apply -f -
```
설치확인
<img width="960" height="193" alt="image" src="https://github.com/user-attachments/assets/7ff2de3a-c721-4878-8ba9-88b71d85b765" />


#### 2) helm 및 NGINX Gateway Fabric 설치
참조 글) https://helm.sh/docs/intro/install

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

helm repo add jetstack https://charts.jetstack.io
helm repo update

helm install ngf oci://ghcr.io/nginx/charts/nginx-gateway-fabric \
  --create-namespace \
  -n nginx-gateway
```

#### 3) 서비스 생성
```bash
kubectl expose deployment nginx --name nginx-svc --type ClusterIP --port 80 --target-port 80

```
<img width="566" height="66" alt="image" src="https://github.com/user-attachments/assets/bc52863a-f9d0-498b-b65e-639436764999" />

#### 4) gateway 생성
```bash
k create -f gateway.yaml
```
<img width="413" height="63" alt="image" src="https://github.com/user-attachments/assets/25ba2061-e615-48e5-a6fb-4a4b7e31cd40" />


##### Controller가 Gateway를 감지해서 실제 트래픽을 받을 데이터플레인 리소스들을 자동 생성, NGINX Gateway Fabric의 Gateway Service가 NodePort 30512로 생성된 것을 확인

<img width="712" height="81" alt="image" src="https://github.com/user-attachments/assets/c2ecf016-8d51-4ecf-a4fa-8f0a37225f9d" />


#### 5) HTTPRoute 생성
```bash
k create -f httproute.yaml
```

#### 6) AWS 구성
##### target-group
- Name: gateway-tg
- Protocol: HTTP
- Port: 30512
- Targets: worker-01, worker-02

##### alb 
- Name: gt-alb
- Subnets: Public-A, Public-B
- Security Group: alb-sg
- Listener: HTTP :80 → gateway-tg

#### 7) Worker Security Group
ALB -> NGINX Gateway Service NodePort(30512)
- Protocol: TCP
- Port: 30512
- Source: ALB Security Group

#### 8) External Access Test
```bash
curl -H "Host: nginx.example.com" http://<ALB-DNS>

```

<img width="936" height="444" alt="image" src="https://github.com/user-attachments/assets/f7f93543-e32f-48a4-a4c6-bbdebb896cfe" />

