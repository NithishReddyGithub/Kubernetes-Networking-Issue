# Kubernetes Networking Troubleshooting Guide

This document summarizes common steps to troubleshoot networking issues in Kubernetes deployments.

---

## 1. Check Pod Status

```bash
kubectl get pods -n <namespace> -o wide
```

- Verify pods are Running and not in CrashLoopBackOff or Pending.
- Use -o wide to check which node the pod is running on and the pod IP.

## 2. Inspect Pod Logs

```bash
kubectl logs <pod-name> -n <namespace>
```

- Look for application errors (e.g., port binding issues, service not starting).

If multiple containers are in the pod:

```bash
kubectl logs <pod-name> -c <container-name> -n <namespace>
```

## 3. Verify Container Ports
```bash
kubectl describe pod <pod-name> -n <namespace>
```
- onfirm the container is exposing the expected port (e.g., 80).
- Check if probes (livenessProbe, readinessProbe) are failing.

## 4. Test In-Pod Connectivity
Exec into the pod and test network reachability:

```bash
kubectl exec -it <pod-name> -n <namespace> -- /bin/sh
curl http://localhost:80
```
- Confirms if the app is serving traffic inside the pod.

## 5. Check Service
```bash
kubectl get svc -n <namespace>
kubectl describe svc <service-name> -n <namespace>
```

## 6. LoadBalancer External IP
```bash
kubectl get svc my-profile-service -n <namespace>
```

If EXTERNAL-IP is <pending>:

- Check if cloud provider integration is correct (AKS, EKS, GKE).
- Verify that your cluster supports LoadBalancer services.

## 8. Connectivity from Outside
Once EXTERNAL-IP is assigned:
```bash
curl http://<external-ip>
```
- Confirms external access to the service.
