# platform-hardening-checklist

## 1. RBAC
- create RBAC and restrice to least privelige acess
- limit user's to minimum in AWS-auth.yaml
- make sure every user and resources going through RBAC.
- create role's and role binding accordingly.

## 2. SA
- create our own service account at namespace level and granting secret reading access and configuration reading access so that the user creating any resources in that namespace can also operate with least privelage access.
- every namespace will have default SA.

## 3. Service
- don't Use `type: LoadBalancer` for external access, only use AWS Load Balancer Controller with TargetGroupBinding.
- Restrict Service exposure to only required ports.
- Use security groups to restrict access at the AWS level.
- always go for cluster IP.
- always use network poliy to restrict the access of the pods it give more control.

## 4. Image Security
- Use images from trusted registries (ECR, Docker Hub official).
- Scan images for vulnerabilities (Amazon Inspector, ECR scan).
- Use immutable image tags (avoid `:latest`).
- use cosign imanges for untampering.

## 5. creating our own ALB.
we can create our own ALB so that we can bind the target groups and listeners. without creating ingress resource in cluster.

## 6. use custom admission policy.
- Use custom admission policy tools like kyverno to implement custom policies (like blocking unsigned images )
- where it provides more control to manages the control.

## 7. IAM & Access Control
- Use IAM roles for service accounts (IRSA) for pod-level AWS permissions.
- Grant least privilege IAM permissions to nodes and service accounts.
- Disable unused IAM users/roles and rotate credentials regularly.

## 8. managed nodes
- always go for managed resources with your nodes managed resources is eqaul to more access.
- so that we can place nodes in private vpc more control on this aspect.
- and we can create our pwn manged security groups.

## 9. ConfigMap
- in static application placing your dynamic configurating will help us to deplay the application fastly.
- Do not store sensitive data (like passwords or secrets) in ConfigMaps. Use Kubernetes Secrets or AWS Secrets Manager for sensitive information.
- Limit access to ConfigMaps using RBAC.

## 10. Deployment
- Set resource `requests` and `limits` for all containers (already present).
- Use liveness and readiness probes (already present).
- Use immutable image tags (avoid `:latest`).
- Use IAM Roles for Service Accounts (IRSA) for AWS permissions if your app interacts with AWS services.

## 11. HorizontalPodAutoscaler
- Set reasonable min/max replicas and CPU utilization thresholds.
- Monitor scaling events and adjust thresholds as needed.

## 12. Secrets Management
- Store secrets in AWS Secrets Manager or SSM Parameter Store, not in ConfigMaps.
- Use Kubernetes Secrets with encryption at rest enabled.
- Mount secrets as environment variables or files, not hardcoded.

## 13. Networking
- Use VPC private subnets for worker nodes.
- Restrict security groups to only required ports (e.g., 80/443 for frontend).
- Use AWS NLB/ALB with security groups for external access (see TargetGroupBinding in manifest).
- Apply Kubernetes NetworkPolicies to restrict pod-to-pod and namespace-to-namespace traffic.

## 14. IAM & Access Control
- Use IAM roles for service accounts (IRSA) for pod-level AWS permissions.
- Grant least privilege IAM permissions to nodes and service accounts.
- Disable unused IAM users/roles and rotate credentials regularly.

## 15. Ingress & External Exposure
- Use AWS ALB Ingress Controller (now AWS Load Balancer Controller) for HTTP/S traffic.
- Use WAF with ALB for web application protection.
- Restrict allowed hosts and paths in Ingress resources.
- Use TLS certificates from AWS ACM for HTTPS.

