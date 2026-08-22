## ALB
외부 HTTP 요청을 Kubernetes 클러스터로 전달하기 위해 Internet-facing Application Load Balancer를 구성

- Name: k8s-ingress-alb
- Scheme: Internet-facing
- IP address type: IPv4
- VPC: lab_vpc

Subnets:
- Public-A
- Public-B

Security Group:
- alb-sg

Listener:
- HTTP :80 → alb-tg


