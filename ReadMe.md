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
Docker Desktop
 └── Kubernetes (single-node)
      └── Couchbase Operator
           └── Couchbase Cluster
                ├── Data Service
                ├── Query Service
                ├── Index Service
                └── Analytics Service

This setup mirrors how Couchbase is deployed on EKS / AKS / GKE, just scaled down for local use.


Prerequisites:
Make sure you have the following installed:
* Docker Desktop (Kubernetes enabled)
* kubectl
* Helm v3+
* At least 8 GB RAM allocated to Docker Desktop


Verify:
```bash
kubectl get nodes
helm version
```

1. Install Couchbase Operator using Helm

helm repo add couchbase https://couchbase-partners.github.io/helm-charts/
helm repo update

Install (or upgrade) the operator:
helm upgrade couchbase-operator couchbase/couchbase-operator \
  --namespace couchbase \
  --install

Verify:
helm list -n couchbase
kubectl get pods -n couchbase

2. Create Couchbase Admin Secret
Couchbase will not start without admin credentials.
kubectl create secret generic cb-admin-secret \
  --from-literal=username=Administrator \
  --from-literal=password=Password@123 \
  -n couchbase


3. Deploy Couchbase Cluster (Custom Resource)
kubectl apply -f cb-cluster.yaml -n couchbase
kubectl get pods -n couchbase -w


4. Access Couchbase Web Console
kubectl port-forward svc/cb-cluster-ui 8091:8091 -n couchbase


Open http://localhost:8091

Login Using Administrator / Password@123

Understanding Operator-Managed Buckets:
buckets:
  managed: true



