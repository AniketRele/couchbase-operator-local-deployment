
kubectl delete couchbasecluster cb-cluster -n couchbase

kubectl get pods -n couchbase

kubectl delete couchbasebucket --all -n couchbase

kubectl get couchbasebuckets -n couchbase

kubectl delete secret cb-admin-secret -n couchbase

helm uninstall couchbase-operator -n couchbase

helm list -n couchbase


kubectl get crds | Select-String couchbase | ForEach-Object {
  kubectl delete crd ($_.ToString().Split()[0])
}

kubectl get crds | findstr couchbase
