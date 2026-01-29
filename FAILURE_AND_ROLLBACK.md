# FAILURE AND ROLLBACK SCENARIOS

## 1. A faulty version is deployed to production – how do you rollback?

This project follows a GitOps deployment model.  
All Kubernetes manifests are stored in the gitops/ directory.  
If a faulty version is deployed, we rollback by reverting the last stable commit in the gitops repository.  
A GitOps tool like ArgoCD/Flux automatically syncs the cluster back to the previous stable version.  
No manual kubectl rollback is required.

---

## 2. Application crashes after deployment – what happens?

Kubernetes liveness and readiness probes are configured in the Deployment.  
If the application crashes, the liveness probe fails and Kubernetes automatically restarts the container.  
If the new pods are not healthy, Kubernetes stops routing traffic to them, ensuring high availability.

---

## 3. Jenkins is down – can deployment still occur?

Yes. Jenkins is only responsible for CI (building and pushing Docker images).  
Actual deployment is handled by the GitOps tool watching the gitops/ directory.  
If Jenkins is down, no new builds occur, but existing GitOps deployments and cluster operations continue running without interruption.

---

## 4. Secrets are leaked – what steps do you take?

If secrets are leaked:
1. Immediately revoke or rotate the compromised credentials.
2. Update Kubernetes Secret objects with new values.
3. Re-deploy applications to use updated secrets.
4. Audit logs to identify unauthorized access.
5. Ensure secrets are never stored in plain text in Git.

---

## 5. Kubernetes node fails – how does the system recover?

Kubernetes continuously monitors node health.  
If a node fails, pods running on that node are automatically rescheduled to healthy nodes in the cluster.  
The Service ensures traffic is routed only to healthy pods, maintaining application availability.
