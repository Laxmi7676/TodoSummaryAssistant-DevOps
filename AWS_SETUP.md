# AWS EKS Deployment Design

This section describes how the Kubernetes workloads can be deployed on AWS using Amazon EKS.

---

## Architecture Diagram (High Level)

Developer → GitHub Repository
|
v
Jenkins CI Server
|
v
Docker Image pushed to DockerHub
|
v
GitOps Repository (gitops/)
|
v
ArgoCD / Flux in EKS Cluster
|
v
Amazon EKS Cluster
/
Worker Node Group Worker Node Group
| |
Todo App Pods Todo App Pods
|
Kubernetes Service
|
AWS Load Balancer
|
End Users



---

## AWS Setup Explanation

1. An Amazon EKS cluster is created in a dedicated VPC.
2. Managed Node Groups provide worker nodes for running application pods.
3. ArgoCD (or Flux) is installed in the cluster to enable GitOps.
4. Jenkins CI builds Docker images and pushes them to DockerHub.
5. Kubernetes manifests stored in the gitops/ directory are synced by ArgoCD.
6. A Kubernetes Service of type LoadBalancer exposes the application externally.
7. AWS Application Load Balancer routes external traffic to EKS pods.

---

## Security Best Practices

- IAM roles are used for EKS cluster and node group permissions.
- No AWS access keys are stored in code or Git.
- Kubernetes Secrets are used for sensitive application configuration.
- VPC security groups restrict inbound and outbound traffic.
- Private subnets are used for worker nodes.

---

## Optional Infrastructure as Code (Terraform Example)

```hcl
module "eks" {
  source          = "terraform-aws-modules/eks/aws"
  cluster_name    = "todo-eks-cluster"
  cluster_version = "1.29"
  subnet_ids      = ["subnet-abc123", "subnet-def456"]
  vpc_id          = "vpc-xyz123"

  eks_managed_node_groups = {
    default = {
      desired_size = 2
      instance_types = ["t3.medium"]
    }
  }
}

