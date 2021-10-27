---
description: >-
  Kubernetes is an open-source container-orchestration system for automating
  computer application deployment, scaling, and management.
---

# Kubernetes

## Use kubectl commands

The kubectl command line tool lets you control Kubernetes clusters. For configuration, `kubectl` looks for a file named `config` in the `$HOME/.kube` directory. You can specify other [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) files by setting the KUBECONFIG environment variable or by setting the [`--kubeconfig`](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) flag.

Command overview: [https://kubernetes.io/docs/reference/kubectl/overview/](https://kubernetes.io/docs/reference/kubectl/overview/)

### List all running pods

```
kubectl get pods

# list pods in all namespaces
kubectl get pods -A
```

### Delete pod

```
kubectl delete po demo-889bb54fc-4brqx

# force pod deletion
kubectl delete po demo-889bb54fc-4brqx --grace-period 0 --force 
```

### Flush Kubernetes DNS

If you encounter DNS issues inside your K8s cluster it can sometimes help to restart the **coredns** service.

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
# by pod name
kubectl logs -f demo-889bb54fc-4brqx

# by label
kubectl logs -f -l app=demo

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

### Get all K8s worker nodes

```
kubectl get nodes --sort-by='.metadata.creationTimestamp'

NAME                                           STATUS   ROLES    AGE     VERSION
ip-10-255-216-218.us-west-2.compute.internal   Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-220-178.us-west-2.compute.internal   Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-216-41.us-west-2.compute.internal    Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-202-243.us-west-2.compute.internal   Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-202-137.us-west-2.compute.internal   Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-217-234.us-west-2.compute.internal   Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-204-221.us-west-2.compute.internal   Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-203-49.us-west-2.compute.internal    Ready    <none>   35d     v1.20.7-eks-135321
ip-10-255-202-81.us-west-2.compute.internal    Ready    <none>   35d     v1.20.7-eks-135321
...
```
