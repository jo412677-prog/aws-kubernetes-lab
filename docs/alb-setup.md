## Target Group
ALB의 HTTP 요청을 Kubernetes Worker Node의 NGINX Ingress Controller NodePort로 전달하도록 Target Group을 구성

- Target type: Instances
- Protocol: HTTP
- Port: ingress - 31050, gateway - 30512
- VPC: lab_vpc
- Targets:
  - worker-01
  - worker-02

## ALB
외부 HTTP 요청을 Kubernetes 클러스터로 전달하기 위해 Internet-facing Application Load Balancer를 구성

- Name: (ingress → k8s-ingress-alb), (gateway → gt-alb) 
- Scheme: Internet-facing
- IP address type: IPv4
- VPC: lab_vpc

Subnets:
- Public-A
- Public-B

Security Group:
- alb-sg

Listener:
ingress - HTTP :80 → alb-tg 
gateway - HTTP :80 → gateway-tg
