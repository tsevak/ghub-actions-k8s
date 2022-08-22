# ghub-actions-k8s


1. Start a cluster

```bash
time minikube start
```

2.  Verify a cluster
```bash
k get pods -A
```

3. Get Personal Access token from Github

4.  Deploy action runner controller by Helm

It requires cert-manager

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
```
Search for cert-manager
```bash
helm search repo cert-manager
```
Install cert-manager
```bash
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.9.1 \
  --set prometheus.enable=false \
  --set installCRDs=true
```
Search for gh-actions

```bash
helm search repo actions
```

Install github actions runner controller
```bash
helm install gh-actions -f custom-values.yaml  \
    actions-runner-controller/actions-runner-controller \
    --namespace gh-actions \
    --create-namespace \
    --version 0.20.0 \
    --set syncPeriod=1m
```

5. Let deploy a runner and test with worflow

```bash
k apply -f self-runner.yaml
```