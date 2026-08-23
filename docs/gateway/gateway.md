## Gateway API

Kubernetes Gateway API의 Gateway와 HTTPRoute를 구성하여 nginx Service를 HTTP 80 포트로 외부에 노출

참조 글) https://docs.nginx.com/nginx-gateway-fabric/get-started/#add-gateway-api-resources

#### 1) Gateway API CRD 설치

```bash
kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v2.6.7" | kubectl apply -f -
```
설치확인
<img width="960" height="193" alt="image" src="https://github.com/user-attachments/assets/7ff2de3a-c721-4878-8ba9-88b71d85b765" />


#### 2) helm 및 NGINX Gateway Fabric 설치
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
k create -f gateway.taml
```
<img width="413" height="63" alt="image" src="https://github.com/user-attachments/assets/25ba2061-e615-48e5-a6fb-4a4b7e31cd40" />

#### 5) HTTPRoute 생성
```bash
k create -f httproute.yaml
```
