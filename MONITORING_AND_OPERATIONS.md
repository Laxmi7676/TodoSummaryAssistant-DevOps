# MONITORING AND OPERATIONS

## Metrics to Monitor

- CPU and Memory usage of pods and nodes  
- Pod restart count  
- Application response time (HTTP latency)  
- Request throughput  
- Disk usage of cluster nodes  

These metrics help identify performance bottlenecks and resource issues early.

---

## Critical Logs

- Application logs (Spring Boot logs)  
- Container stdout/stderr logs  
- Kubernetes events and pod logs  
- Jenkins build logs  

These logs help in debugging application and deployment issues.

---

## Important Alerts

- Pod crash looping or frequent restarts  
- High CPU or memory utilization  
- Application not responding (failed readiness probes)  
- Kubernetes node not ready  
- Failed Jenkins pipeline builds  

Unnecessary alerts like minor temporary latency spikes should be avoided to prevent alert fatigue.

---

## Early Detection of Operational Issues

- Prometheus collects metrics from Kubernetes and applications.  
- Grafana dashboards visualize system health.  
- Alertmanager sends notifications when thresholds are breached.  
- Centralized logging (ELK / Loki) helps in quick root cause analysis.

This ensures issues are detected and resolved before impacting users.
