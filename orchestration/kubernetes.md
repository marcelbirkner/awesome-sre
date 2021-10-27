---
description: >-
  Kubernetes is an open-source container-orchestration system for automating
  computer application deployment, scaling, and management.
---

# Kubernetes

## Use kubectl commands

The kubectl command line tool lets you control Kubernetes clusters. For configuration, `kubectl` looks for a file named `config` in the `$HOME/.kube` directory. You can specify other [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) files by setting the KUBECONFIG environment variable or by setting the [`--kubeconfig`](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) flag.

### Flush Kubernetes DNS

```
kubectl scale deployment.apps/coredns -n kube-system --replicas=0
kubectl scale deployment.apps/coredns -n kube-system --replicas=2
```

### Get last 50 updated pods

```
watch 'kubectl get pods --sort-by=.status.startTime | tail -50'
```

### Get all pods that are not in "Running" state

```
watch 'kubectl get pods | grep -v Running'
```

### Get logs for pods

```
# by label
kubectl logs -f -l app=ui-client

# by deployment
kubectl -n kube-system logs -f deployment.apps/cluster-autoscaler
```

### Delete pod in case it is stuck in "terminating"

```
kubectl get namespace instana-agent -o json > instana-agent.json

# Remove "kubernetes" from the finalizers array in instana-agent.json

# Execute cleanup command
kubectl replace --raw "/api/v1/namespaces/instana-agent/finalize" -f ./instana-agent.json
```

### Port Forwarding

```
kubectl port-forward --namespace demo deployment/component 8600:8600

# afterwards you can access the pod locally via the forwarded port 8600
curl localhost:8600/...
```
