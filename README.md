# Setting Up Sandbox Cluster

## Simple cluster with Ingress

To spin up personal cluster, this will create an ingress ready cluster:\
`kind create cluster --name cluster1 --config personal-kind.yaml`

### To install Ingress
`kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml`

To check if ingress is ready: \
`kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=90s`

## To Clean Up
`kind delete cluster --name cluster1`