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

### External Access Test
```bash
curl -H "Host: nginx.example.com" http://<ALB-DNS>

```
<img width="1050" height="467" alt="image" src="https://github.com/user-attachments/assets/9edae8f1-3903-4c2d-951f-7a34c0b197ac" />


