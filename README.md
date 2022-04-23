# Setting Up Sandbox Cluster

## Simple cluster with Ingress

To spin up personal cluster, this will create an ingress ready cluster:\
```
kind create cluster --name cluster1 --config simple-cluster.yaml
```

### To install Ingress Only
```
kubectl kustomize cluster-init/overlay/setup-ingress | k apply -f -
```

### To install Ingress + ArgoCD
```
kubectl kustomize cluster-init/overlay/setup-ingress-argocd | k apply -f -
# to check if argocd is ready
kubectl wait --namespace argocd --for=condition=ready pod --selector=app.kubernetes.io/name=argocd-server --timeout=90s
# to get the initial admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
Access argocd https://localhost/.

### To install ArgoWorkflow
```
kubectl apply -f cluster-init/cluster-apps/argo-workflow.yaml
```
Access argocd https://localhost/argo.

### To install KubeVela
```
kubectl apply -f cluster-init/cluster-apps/kubevela.yaml
```

## To Clean Up
```
kind delete cluster --name cluster1
```