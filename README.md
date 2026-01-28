# platform-hardening-checklist
## AWS EKS Hardening
This document provides hardening recommendations and best practices for the Kubernetes resources

## 1. ConfigMap
- Do not store sensitive data (like passwords or secrets) in ConfigMaps. Use Kubernetes Secrets or AWS Secrets Manager for sensitive information.
- Limit access to ConfigMaps using RBAC.

## 2. Deployment
- Set resource `requests` and `limits` for all containers (already present).
- Use liveness and readiness probes (already present).
- Use immutable image tags (avoid `:latest`).
- Use IAM Roles for Service Accounts (IRSA) for AWS permissions if your app interacts with AWS services.

## 3. Service
- Use `type: LoadBalancer` for external access, or use AWS Load Balancer Controller with TargetGroupBinding.
- Restrict Service exposure to only required ports.
- Use security groups to restrict access at the AWS level.

## 4. HorizontalPodAutoscaler
- Set reasonable min/max replicas and CPU utilization thresholds.
- Monitor scaling events and adjust thresholds as needed.

## 5. NetworkPolicy (Recommended)
- Apply NetworkPolicies to restrict pod-to-pod and namespace-to-namespace traffic.
- Allow only required ingress/egress for frontend pods (e.g., allow from ALB, allow egress to backend).

## 6. Image & Supply Chain Security
- Use images from trusted registries (ECR, Docker Hub official).
- Scan images for vulnerabilities (Amazon Inspector, ECR scan).
- Use immutable image tags (avoid `:latest`).

## 7. Secrets Management
- Store secrets in AWS Secrets Manager or SSM Parameter Store, not in ConfigMaps.
- Use Kubernetes Secrets with encryption at rest enabled.
- Mount secrets as environment variables or files, not hardcoded.

## 8. Networking
- Use VPC private subnets for worker nodes.
- Restrict security groups to only required ports (e.g., 80/443 for frontend).
- Use AWS NLB/ALB with security groups for external access (see TargetGroupBinding in manifest).
- Apply Kubernetes NetworkPolicies to restrict pod-to-pod and namespace-to-namespace traffic.

## 9. IAM & Access Control
- Use IAM roles for service accounts (IRSA) for pod-level AWS permissions.
- Grant least privilege IAM permissions to nodes and service accounts.
- Disable unused IAM users/roles and rotate credentials regularly.

## 10. Ingress & External Exposure
- Use AWS ALB Ingress Controller (now AWS Load Balancer Controller) for HTTP/S traffic.
- Use WAF with ALB for web application protection.
- Restrict allowed hosts and paths in Ingress resources.
- Use TLS certificates from AWS ACM for HTTPS.

