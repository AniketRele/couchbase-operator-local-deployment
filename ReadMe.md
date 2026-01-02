Deploying Couchbase on Local Kubernetes using Couchbase Operator

Introduction

In this tutorial, we will deploy Couchbase locally on Kubernetes using Docker Desktop and the Couchbase Autonomous Operator.
We will go from a clean environment to running Analytics queries with indexes, closely mirroring a real production setup.

This guide is ideal for:
* Data Engineers
* Platform Engineers
* Anyone learning Kubernetes operators
* Developers exploring Couchbase Analytics

Architecture Overview:
```bash
Docker Desktop
 └── Kubernetes (single-node)
      └── Couchbase Operator
           └── Couchbase Cluster
                ├── Data Service
                ├── Query Service
                ├── Index Service
                └── Analytics Service
```
This setup mirrors how Couchbase is deployed on EKS / AKS / GKE, just scaled down for local use.


Prerequisites:
Make sure you have the following installed:
```bash
* Docker Desktop (Kubernetes enabled)
* kubectl
* Helm v3+
* At least 8 GB RAM allocated to Docker Desktop
```

Verify:
```bash
kubectl get nodes
```
Install Helm on 
Windows:
```bash
winget install Helm.Helm
```
Mac:
```bash
brew install helm
```

1. Install Couchbase Operator using Helm

```bash
helm repo add couchbase https://couchbase-partners.github.io/helm-charts/
helm repo update
```

Install (or upgrade) the operator:
```bash
helm upgrade couchbase-operator couchbase/couchbase-operator \
  --namespace couchbase \
  --install
```

Verify:
```bash
helm list -n couchbase
kubectl get pods -n couchbase
```

2. Create Couchbase Admin Secret
Couchbase will not start without admin credentials.
```bash
kubectl create secret generic cb-admin-secret \
  --from-literal=username=Administrator \
  --from-literal=password=Password@123 \
  -n couchbase
```

3. Deploy Couchbase Cluster (Custom Resource)
```bash
kubectl apply -f cb-cluster.yaml -n couchbase
kubectl get pods -n couchbase -w
```

4. Access Couchbase Web Console
```bash
kubectl port-forward svc/cb-cluster-ui 8091:8091 -n couchbase
```

Open
```bash
http://localhost:8091
```
Login Using Administrator / Password@123

Understanding Operator-Managed Buckets:
```bash
buckets:
  managed: true
```

Get couchbase Bucket:
```bash
kubectl get couchbasebuckets -n couchbase
```

Delete any couchbase bucket:
```bash
kubectl delete couchbasebucket <-bucketname-> -n couchbase
```

Add a new Couchbase Bucket:
```bash
kubectl apply -f cb-buckets.yaml -n couchbase
```

