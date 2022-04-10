# Setting Up Sandbox Cluster

## Simple cluster with Ingress

To spin up personal cluster, this will create an ingress ready cluster:\
```
kind create cluster --name cluster1 --config simple-kind.yaml
```

### To install Ingress
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
# To check if ingress is ready:
kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=90s
```

### To install ArgoCD
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### To install ArgoWorkflow
```
kubectl kustomize argo-workflows/overlay/dev | kubectl apply -f -
# when done navigate to localhost/ to access it
```

### To install KubeVela
```
helm upgrade --install kubevela kubevela/vela-core --namespace vela-system --create-namespace --wait
```

## To Clean Up
```
kind delete cluster --name cluster1
```